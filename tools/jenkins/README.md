# Jenkins

\
\
[Installation](https://www.jenkins.io/doc/book/installing/)

Java requirements - 8/11  &#x20;

Ubuntu:

```
$ wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
$ sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
$ sudo apt-get update
$ sudo apt-get install jenkins
```

Docker

```
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts
--- with volume
docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts

1 - create network bridge
$ docker network create jenkins

2 - creaete volumes
$ docker volume create jenkins-docker-certs
$ docker volume create jenkins-data

3 - run image
$ docker container run --name jenkins-docker --rm --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 docker:dind
```

`$ sudo service jenkins [status | start | stop | restart]`

* Plugins - extensions to add to Jenkins
* Jobs - work unit
* Build queue - jobs can wait their queue
* Build executors - jobs can be executed parallel.
* Agent - specifies **where** the Pipeline (or specific stages) will be executed.

```
agent {
  label 'название-метки'
}
```

```
agent any | none | label | node | docker | dockerfile | kubernetes
```

* label - execute with the provided label.



* var/lib/jenkins/pugins
* var/jenkins\_home/plugins
* var/jenkins\_home/jobs/\<project>/builds/1 - builds folder - output log here
* http://localhost:8080/env-vars.html/    - list of actual env vars



PublishOverSsh - plugin to publish artifact over ssh.

sshAgents  + sshSlaves - plugins to add slave nodes over ssh

node is defined by labels

* when we add node we create his label
* in job we define label to use concrete node



CLI

{$JENKINS\_URL}/jnlpJars/jenkins-cli.jar

1. download in ui    [http://localhost:8080/cli](http://localhost:8080/cli)/
2. download inside server    wget  [http://localhost:8080/](http://localhost:8080/cli)jnlpJars/jenkins-cli.jar

save to env

```
java -jar jenkins-cli.jar -s http://localhost:8080/ who-am-i

#get job to xml file
java -jar jenkins-cli.jar -s <server> get-job MyJob1 > myjob.xml

#create job from file
java -jar jenkins-cli.jar -s <server> get-job MyJob1 < myjob.xml

#power shell
get-content myjob.xml | java -jar jenkins-cli.jar -s <server> get-job MyJob1
```

```
JENKINS_USER_ID
JENKINS_API_TOKEN

--- ubuntu
export JENKINS_USER_ID="user"
export JENKINS_API_TOKEN="0fd0sfds8f38rsd9af8sd09vsd"
--- power shell
$env:JENKINS_USER_ID="user"
$env:JENKINS_API_TOKEN="0fd0sfds8f38rsd9af8sd09vsd"
```

For clone git project use ssh key:

* create key
* add public key to github
* add private key to jenkins

Jenkins > Credentials > System > Global credentials > Add Credentials then selecting SSH Username with private key.

Build triggers

1. Trigger build remotely - you get url for start job
2. Build after other project are built
3. Build periodically
4. Poll SCM - check git for new commit
5. GitHub trigger

`$ curl http://user-name:password@<server>/job/<job-name>/build?token=<generatedToken>`

github webhook

set github project in general block in job

payload url - `http://<server>/github-webhook/  content-type: application/json`\
\
\
[https://www.jenkins.io/doc/book/pipeline/getting-started](https://www.jenkins.io/doc/book/pipeline/getting-started)/\
