# Java Messaging System



An API for create, read and send messages.

* Loosely coupled
* reliable - надежный
* asynchronous

\
2 types of messaging

* **Point-to-Point** Messaging Domain

\


![](https://www.javatpoint.com/ejbpages/images/jms-point-to-point-model.png)

* **Publisher/Subscriber Messaging** Domain

\


![](https://www.javatpoint.com/ejbpages/images/jms-publisher-subscriber-model.png)

### **javax.jms example**

* In any application server we need to create 2 JNDI resources
  * **myQueueConnectionFactory**
  * **myQueue**
* create server and receiver application
* create connection factory and destination resource in application server
*   **Create sender and receiver application**



sender

```
//Create and start connection  
InitialContext ctx=new InitialContext();  
QueueConnectionFactory f=(QueueConnectionFactory)ctx.lookup("myQueueConnectionFactory");  
QueueConnection con=f.createQueueConnection();  
con.start();  

//2) create queue session  
QueueSession ses=con.createQueueSession(false, Session.AUTO_ACKNOWLEDGE);  

//3) get the Queue object  
Queue t=(Queue)ctx.lookup("myQueue");  

//4)create QueueSender object         
QueueSender sender=ses.createSender(t);  

//5) create TextMessage object  
TextMessage msg=ses.createTextMessage();

//6) write message
msg.setText(s);

//7) send message
sender.send(msg);

//8) connection close  
con.close();
```

reciever

```
//4)create QueueReceiver  
QueueReceiver receiver=ses.createReceiver(t);  
              
//5) create listener object  
MyListener listener=new MyListener();  
              
//6) register the listener object with receiver  
receiver.setMessageListener(listener);
```

listener

```
import javax.jms.*;  
public class MyListener implements MessageListener {  
      
public void onMessage(Message m) {  
            try{  
            TextMessage msg=(TextMessage)m;  
          
            System.out.println("following message is received:"+msg.getText());  
            }catch(JMSException e){System.out.println(e);}  
        }  
}
```
