
# install Jenkins BlueOcean using Docker
In my environment, to make sure the network accessibility, I have to configure the proxy and Docker image mirror repository base on my local network situation.
- Then, get the docker image `docker pull jenkinsci/blueocean:latest`
- Running the container:  
```
docker run -it -detach --name atjenkinsci -p 18080:8080 -p 15000:5000 -v /data/container/atjenkinsci:/var/jenkins_home jenkinsci/blueocean:latest
```  
Create, start and run in background docker container named jenkins using the jenkinsci/blueocean:latest image.  
Publish and forward all traffic sent to local port 18080 into containers port 8080.  
Link local folder $HOME/docker/volumes/jenkins to folder /var/jenkins_home inside the container for storing data we want to keep.
- Once the jenkins constainer is started, just browse the web pages via the corresponding hostname/IP and port.
- When Jenkins runs the first time, it will generate a password to get into admin ui of the Jenkins application. This password is shown to you in the terminal window by default, but since we used the --detach flag in the command, we won’t see it. But you could find it in your file system, according to the hint in the jenkins web page you will see.