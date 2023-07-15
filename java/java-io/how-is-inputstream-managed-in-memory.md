# How is InputStream managed in memory?

I am familiar with the concept of `InputStream`,buffers and why they are useful _(when you need to work with data that might be larger then the machines RAM for example)_.

I was wondering though, how does the `InputStream` actually carry all that data?. Could a `OutOfMemoryError` be caused if there is TOO much data being transfered?

### Case-scenario

If I connect from a client to a server,requesting a 100GB file, the server starts iterating through the bytes of the file with a buffer, and writing the bytes back to the client with `outputStream.write(byte[])`. The client is not ready to read the `InputStream` right now,for whatever reason. Will the server continue sending the bytes of the file indefinitely? and if so, won't the `outputstream/inputstream` be larger than the RAM of one of these machines?

asked Dec 30, 2018 at 13:01



Daniel B.Daniel B.

2,2231 gold badge9 silver badges22 bronze badges



`InputStream` and `OutputStream` implementations do not generally use a lot of memory. In fact, the word "Stream" in these types _means_ that it does not need to hold the data, because it is accessed in a sequential manner -- in the same way that a stream can transfer water between a lake and the ocean without holding a lot of water itself.

But "stream" is not the best word to describe this. It's more like a pipe, because when you transfer data from a server to a client, every stage transfers _back-pressure_ from the client that controls the rate at which data gets sent. This is similar to how your faucet controls the rate of flow through your pipes all the way to the city reservoir:

1. As the client reads data, it's `InputStream` only requests more data from the OS when its internal (small) buffers are empty. Each request allows only a limited amount of data to be transferred;
2. As data is requested from the OS, its own internal buffer empties, and it notifies the server about how much space there is for new data. The server can send only this much (that's called 'flow control' in TCP: [https://en.wikipedia.org/wiki/Transmission\_Control\_Protocol#Resource\_usage](https://en.wikipedia.org/wiki/Transmission\_Control\_Protocol#Resource\_usage))
3. On the server side, the server-side OS sends out data from its own internal buffer when the client has space to receive it. As its own internal buffer empties, it allows the writing process to re-fill it with more data.
4. As the server-side process write()s to its `OutputStream`, the `OutputStream` will try to write data to the OS. When the OS buffer is full, it will make the server process wait until the server-side buffer has space to accept new data.w

Notice that a slow client can make the server process take a very long time. If you're writing a server, and you don't control the clients, then it's very important to consider this and to ensure that there are not a lot of server-side resources tied up while a long data transfer takes place.

answered Dec 30, 2018 at 14:37



Matt TimmermansMatt Timmermans

45.2k2 gold badges35 silver badges75 bronze badges

Your question is as interesting as difficult to answer properly.

* First: `InputStream` and `OutputStream` are not a storage means, but an _access_ means: They describe that the data shall be accessed in **sequential, unidirectional order**, but not how it shall be stored. The actual way of storing the data is **implementation-dependent**.

So, would there be an InputStream that stores the whole amount of data simultaneally in memory? Yes, could be, though it would be an appalling implementation. The most common and sensitive implementation of InputStreams / OutputStreams is by storing **just a fixed and short amount of data** into a temporary buffer of 4K-8K, for example.

(So far, I supposed you already knew that, but it was necessary to tell.)

* Second: What about connected writting / reading streams between a server and a client? In a common scenario of buffered writting, the server will not write more data than the buffer allows. So, if the server starts writing, and the client then goes down (for whatever reason), the server will just keep writing until the buffer is full, and then set it as ready for reading, and until the read is not completed (by the client peer), the server won't fill the buffer again. Remember: This kind of read/write is **blocking**: The client blocks until there is a buffer ready to be read, and the server blocks (or, at least, the server thread bound to this connection, it's understood) until the last read is completed.

How many time will the server block? Typically, a server should have a security **timeout** to ensure that long blocks will break the connection, thus releasing the blocked thread. The same should have the client.

The timeouts set for the connection depend on the implementation, and the protocol.

answered Dec 30, 2018 at 13:56



Little SantiLittle Santi

8,3452 gold badges17 silver badges43 bronze badges

No, it does not need to hold all data. I just advances forward in the file (usually using buffered data). The stream can discard old buffers as it pleases.

Note that there are a a lot of very different implementations of inputstreams, so the exact behaviour varies a lot.

answered Dec 30, 2018 at 13:06

