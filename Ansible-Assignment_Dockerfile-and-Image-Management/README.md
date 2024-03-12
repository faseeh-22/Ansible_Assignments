# Assignment:

 Create a ansbile playbook for following tasks: Create a dockerfile. Copy dockerfile on specific location (any location) Build Docker image Running Container from same docker image
To complete the assignment follow the below steps:



## Steps and Commands
 
**Step1.** First create a directory for dockerfile  and navigate  to it.
```
mkdir  Docker
cd Docker
``` 


**Step2.** Create a dockerfile with an editor e.g vim editor.

`vim dockerfile`



**Step3.** Now write following content in it
``` 
FROM ubuntu:latest
 
RUN apt-get update
 
WORKDIR /app
 
COPY . /app
 
RUN echo "Hello, World"
``` 
**Explanation:**
- This Dockerfile starts with a base image of Ubuntu's latest version.
- It then updates the package list using “apt-get update”.
- Next, it sets the working directory to “/app” within the container.
- The dockerfile copies all the files from the current directory (where the dockerfile is located) to the ”/app” directory inside the container.
- Finally, it prints "Hello, World" when the container is run.
 
**Step4.** Also create another  directory for dockerfile
`vim Docker2`
 
**Step5.** Create the ansible playbook  using following command
`vim docker.yml`
 
**Step6.** Now  add the following contents in it. 
```
---
- name: Copy Dockerfile and build Docker image
  hosts: localhost
  connection: local
  become: true
  tasks:
	- name: Copy Dockerfile to a specific location
  	copy:
    	src: /home/faseeh/Docker/dockerfile
    	dest: /home/faseeh/Docker2/dockerfile
 
	- name: Build Docker image
  	shell: docker build -t ubuntu:latest /home/faseeh/Docker2
  	args:
    	chdir: /home/faseeh/Docker2
 
	- name: Run Docker container
  	shell: docker run -d --name mycontainer ubuntu:latest
``` 
**Explanation:**
- The ansible playbook consists of three tasks.
- The first task copies the dockerfile from the source directory “/home/faseeh/Docker/dockerfile” to the destination directory “/home/faseeh/Docker2/dockerfile”.
- The second task builds a docker image using the dockerfile located in “/home/faseeh/Docker2” and tags it as “ubuntu:latest”.
- The third task runs a docker container in detached mode (`-d`) with the name `mycontainer` based on the `ubuntu:latest` image.
-This args section allows you to specify additional arguments or options for the task in the playbook.
-The chdir option is used to change the working directory (i.e., the current directory) for the command being executed by the task
 
**Step7.** Run the Ansible playbook:
 
To execute the ansible playbook, open a terminal and navigate to the directory where the playbook is saved. Then run the following command:
 
`ansible-playbook -b -i invent.ini  docker.yml`
 
Once the playbook runs successfully, it will perform the following actions:
 
- Copy the dockerfile from “/home/faseeh/Docker/dockerfile” to “/home/faseeh/Docker2/dockerfile”.
- Build a docker image named ”ubuntu:latest” using the dockerfile located in `/home/faseeh/Docker2`.
- Run a docker container named “mycontainer” based on the “ubuntu:latest” image.
 
 
 


