# ssh inside container

[https://medium.com/trabe/use-your-local-ssh-keys-inside-a-docker-container-ea1d117515dc](https://medium.com/trabe/use-your-local-ssh-keys-inside-a-docker-container-ea1d117515dc)

&#x20;

It's a harder problem if you need to use SSH at build time. For example if you're using git clone, or in my case pip and npm to download from a private repository.

The solution I found is to add your keys using the --build-arg flag. Then you can use the new experimental --squash command (added 1.13) to merge the layers so that the keys are no longer available after removal. Here's my solution:

**Build command**

```
$ docker build -t example --build-arg ssh_prv_key="$(cat ~/.ssh/id_rsa)" --build-arg ssh_pub_key="$(cat ~/.ssh/id_rsa.pub)" --squash .
```

**Dockerfile**

```
FROM python:3.6-slim

ARG ssh_prv_key
ARG ssh_pub_key

RUN apt-get update && \
    apt-get install -y \
        git \
        openssh-server \
        libmysqlclient-dev

# Authorize SSH Host
RUN mkdir -p /root/.ssh && \
    chmod 0700 /root/.ssh && \
    ssh-keyscan github.com > /root/.ssh/known_hosts

# Add the keys and set permissions
RUN echo "$ssh_prv_key" > /root/.ssh/id_rsa && \
    echo "$ssh_pub_key" > /root/.ssh/id_rsa.pub && \
    chmod 600 /root/.ssh/id_rsa && \
    chmod 600 /root/.ssh/id_rsa.pub

# Avoid cache purge by adding requirements first
ADD ./requirements.txt /app/requirements.txt

WORKDIR /app/

RUN pip install -r requirements.txt

# Remove SSH keys
RUN rm -rf /root/.ssh/

# Add the rest of the files
ADD . .

CMD python manage.py runserver
```



**Update:** If you're using Docker 1.13 and have experimental features on you can append --squash to the build command which will merge the layers, removing the SSH keys and hiding them from docker history.



It starts off easy. Just run: `docker run --rm -it -v ~/.ssh:/root/.ssh:ro alpine` .

If you’re running Linux or MacOS, that’s all you need to do. I like adding in the extra :ro  bit to the volume mount to make it read-only. This just removes the  possibility of you overriding your SSH keys in the container by  accident.

#### But What about Windows (specifically WSL)?

There’s 2 problems.

**PROBLEM 1. Permissions are busted:**

Unfortunately  Windows doesn’t respect Linux file permissions. Your SSH folder and  files will all be set to 777 and SSH won’t work with those permissions.

As  of Windows 10 Spring 2018 edition, you can mount in your drives into  WSL using metadata which supports Linux file permissions, and that’s  fantastic, but Docker for Windows mounts that drive in through CIFS and  that will strip out the metadata.

So you’re stuck with 777 permissions when mounting in your SSH keys through Windows.

**PROBLEM 2. You can’t Docker volume mount files from directly inside of WSL:**

In  addition to the above problem, you can’t mount in files that exist  directly in your WSL drive. They will come up empty, so mounting \~/.ssh doesn’t work.

But  you can get around this one easy enough. You just need to copy your SSH  keys to somewhere that gets mounted into WSL, rather than living  directly in WSL’s file system.

I personally [back up my files to an external HD](https://nickjanetakis.com/blog/automatic-offline-file-backups-with-bash-and-rsync), so I was doing that anyways.

**Fixing the problem with a Docker ENTRYPOINT script:**

First up, make a docker-entrypoint.sh script in the same directory as your Dockerfile. Then add this content to that file:

```
#!/bin/sh
set -e

cp -R /tmp/.ssh /root/.ssh
chmod 700 /root/.ssh
chmod 644 /root/.ssh/id_rsa.pub
chmod 600 /root/.ssh/id_rsa

exec "$@"
```

Then, in your Dockerfile, add this right above your CMD instruction:

```
COPY docker-entrypoint.sh /bin/docker-entrypoint.sh
RUN chmod +x /bin/docker-entrypoint.sh
ENTRYPOINT ["/bin/docker-entrypoint.sh"]
```

Now, run your container like this: docker run --rm -it -v /e/backup/home/nick/.ssh:/tmp/.ssh:ro your\_image.

_(Replace /e/backup/home/nick/.ssh with where your SSH keys are located)_

The  idea here is, you’re mounting in your SSH keys from outside of WSL’s  main file system into the container at a temporary location (and it’s  still read-only).

Then when the container starts, the ENTRYPOINT  kicks in and copies everything to the correct location with the correct  permissions.

Problem solved, you can now use your SSH keys!

If  you’re a Docker image author and you require users to volume mount in  their SSH keys, please think about people running Windows by including  the above ENTRYPOINT script. They will really appreciate it.
