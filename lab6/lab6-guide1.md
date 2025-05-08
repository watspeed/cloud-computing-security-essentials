# Lab 5: Public-Key Infrastructure (PKI) Lab

```
Copyright © 2018 by Wenliang Du.
This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International
License. If you remix, transform, or build upon the material, this copyright notice must be left intact, or
reproduced in a way that is reasonable to the medium in which the work is being re-published.
```
## 1 Overview

Public key cryptography is the foundation of today’s secure communication, but it is subject to man-in-the-
middle attacks when one side of communication sends its public key to the other side. The fundamental
problem is that there is no easy way to verify the ownership of a public key, i.e., given a public key and its
claimed owner information, how do we ensure that the public key is indeed owned by the claimed owner?
The Public Key Infrastructure (PKI) is a practical solution to this problem.
The learning objective of this lab is for students to gain the first-hand experience on PKI. By doing the
tasks in this lab, students should be able to gain a better understanding of how PKI works, how PKI is used
to protect the Web, and how Man-in-the-middle attacks can be defeated by PKI. Moreover, students will be
able to understand the root of the trust in the public-key infrastructure, and what problems will arise if the
root trust is broken. This lab covers the following topics:

- Public-key encryption, Public-Key Infrastructure (PKI)
- Certificate Authority (CA), X.509 certificate, and root CA
- Apache, HTTP, and HTTPS
- Man-in-the-middle attacks


## 2 Lab Environment

### Step 1: Download and Extract the Lab Files
Download Labsetup.zip from the lab’s website and unzip it. This will provide you with the necessary files, including the docker-compose.yml for setting up the lab environment.

```
# Download the lab setup files
$ sudo wget https://seedsecuritylabs.org/Labs_20.04/Files/Crypto_PKI/Labsetup.zip

# Unzip the lab setup files
$ sudo unzip Labsetup.zip
```

### Step 2: Navigate to the Lab Setup Directory
Move into the extracted Labsetup folder, where you will find the docker-compose.yml file and other necessary files.

```
# Enter the Labsetup folder
$ cd Labsetup
```

### Step 3: Build the Docker Container
Use Docker Compose to build the container image. This step prepares the environment for running your web server with the required configurations.

```
# Build the Docker container
$ docker-compose build

# OR use the alias
$ dcbuild
```

### Step 4: Start the Docker Container
This command initializes and runs the container based on the configurations specified in the docker-compose.yml file.

```
# Start the Docker container
$ docker-compose up

# OR use the alias
$ dcup
```

### Step 5: Stop and Shut Down the Docker Container
When you’re finished or need to reset the environment, shut down the running container to release resources.

```
# Stop and shut down the Docker container
$ docker-compose down

# OR use the alias
$ dcdown
```
 
All the containers will be running in the background. To run commands on a container, we often need
to get a shell on that container. We first need to use the `"docker ps"` command to find out the ID of
the container, and then use `"docker exec"` to start a shell on that container. We have created aliases for
them in the `.bashrc` file.

#### 1. List Running Docker Containers
Use the alias dockps to view a list of running containers, displaying each container's ID and name in a simplified format.

```
$ dockps
```

#### Output
The output will list all running Docker containers, each with its unique ID and assigned name. An example output might look like this:

```
b1004832e275 hostA-10.9.0.5
0af4ea7a3e2e hostB-10.9.0.6
9652715c8e0a hostC-10.9.0.7
```
Each line includes the container ID and its corresponding name (in this case, hostA, hostB, and hostC), along with their assigned IP addresses.

#### 2. Access a Specific Container’s Shell
To open a shell inside a specific container, use the alias docksh followed by the first few characters of the container's ID. For example, to access hostC-10.9.0.7, use the ID prefix 96 (from the third line in the previous output).

```
$ docksh 96
```

#### Output
You will be logged into a shell session inside the specified container. The prompt will change to show the container ID, indicating that you are now inside the container:

```
root@9652715c8e0a:/#
```

#### Note: If a Docker command requires the container ID, you only need to type the first few characters, as long as they are unique among all running containers.


### DNS setup.
In this document, we will use bank32.com as the server name for the HTTPS web server throughout the lab.
The bank32.com server name will be recognized within the container setup as specified in the docker-compose.yml and related configuration files.

```
10.9.0.80 [http://www.bank32.com](http://www.bank32.com)
```
