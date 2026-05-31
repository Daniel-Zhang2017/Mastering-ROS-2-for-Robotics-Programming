***Installing ROS 2 Jazzy in Docker***
Docker is an open-source technology that helps software developers quickly develop and deploy Windows, Linux, and macOS applications. Docker is also widely used in robotics software development and deployment. The main advantage of using Docker is that we can quickly develop and deploy your ROS-based application in any ROS distribution and Linux distribution. Even if your host OS is Ubuntu 20.04, you can develop ROS 2 applications in ROS 2 Jazzy (which uses Ubuntu 24.04) using Docker. It will help to build and test your ROS 2 application in different ROS 2 distributions. The only requirement is to install Docker software on these operating systems. Docker is one important technology we use in this course. We will discuss Docker in more detail in this section.
Docker is a software tool to create, deploy, and run applications using a technology called containers. Each container in Docker has a lightweight instance of the software environment to run our application. The environment has code, libraries, and dependencies. This helps containers work in different environments. Each container has no separate OS, like the VM we have seen before. The containers work alongside the host Linux kernel and create an abstraction to run different environments. So, like a VM, we do not have to install a full OS to run an application. Before diving into Docker, let us see how to install it on Ubuntu 24.04 LTS as the host machine.
The official installation of Docker is on their website. In this course, we have added an automatic script to do the same. It will install Docker in Ubuntu 24.04 as well as the NVIDIA Container Toolkit. The NVIDIA Container Toolkit enables users to build and run GPU-accelerated containers that will work alongside Docker.
Installing Docker and the NVIDIA Container Toolkit

Follow these steps to install Docker and the NVIDIA Container Toolkit:

•	Open the course's GitHub repository and navigate to Chapter02 | ros2_jazzy_docker/docker_setup_scripts. You can find setup_docker_ubuntu.sh there. You can run this script by opening a terminal inside this folder:

chmod +x setup_docker_ubuntu.sh
./setup_docker_ubuntu.sh

This script helps install all the dependencies of Docker. If you have an NVIDIA graphics card and driver installed properly, it will install the NVIDIA Container Toolkit, which gives the container graphics acceleration.
•	If everything is installed properly, you can check that Docker is running in the background using the following command:
•	systemctl status docker

After installing Docker on your machine, you can directly pull the Docker image of ROS 2 Jazzy from Docker Hub. Open Robotics publishes all the ROS 1 and 2 Docker images in their account. You can pull any image from their account.
The following command helps to pull the base image of Jazzy from Docker Hub. This will take you directly into the ROS 2 Jazzy environment without installing anything:
docker pull osrf/ros:jazzy-desktop-full

***Creating a container out of the ROS 2 Jazzy image***
To create a container, you can use the following command:
docker run -it --name master_ros2 osrf/ros:jazzy-desktop-full bash

After running this command in your terminal, you will be able to see a new terminal with a different user, which might be a root user, like the following:
root@eafa922c9072:/#
This line is the shell of the ROS 2 Jazzy container with the name master_ros2. If you check the command above, you can see that we use the docker run command to create a container. We must add the image name; we can also mention the container name using the --name argument. The --it argument in Docker helps interact with the container by providing text commands through the bash shell. The bash shell tells the Docker container to run the bash command once it starts. So, this command creates a Docker container with an interactive bash shell with a container name of master_ros2. The container's name is optional here. If you do not include a name for the container, it will randomly assign a name. It will be better to put a name so we can start, stop, and delete this container easily.
After creating the container from the image, you will get a ROS 2 Jazzy environment where you can do anything. Your progress will be lost if you delete the container. The changes you make in the container are cached, so you can start and stop the container without losing data. Only rebuilding it will cause you to lose any data that is not mounted on the host system.
You can perform the following test to make sure ROS 2 Jazzy is working correctly.
Execute the sample publisher node in ROS 2, which publishes a Hello World string. This must execute in the Docker terminal:
root@eafa922c9072:/# ros2 run demo_nodes_cpp talker

If you get a message such as the following, it means it is good to go:
[INFO] [1726068567.949579583] [talker]: Publishing: 'Hello World: 1'
[INFO] [1726068568.949567985] [talker]: Publishing: 'Hello World: 2'
[INFO] [1726068569.949575774] [talker]: Publishing: 'Hello World: 3'

Now, we can run the next command to subscribe to this message. To run a new shell in Docker, you can follow the next section.
Running a new command in the ROS 2 Jazzy container
After creating a container and accessing the shell, we executed the publisher program in ROS 2, and we can see it is working. Now, how do you access another terminal of this container and run the subscriber code? That is where docker exec commands come in. The docker exec commands help us run another program or command in the same container. So, take a new terminal in your host OS and execute the following command to get access to the container terminal:
docker exec -it master_ros2 bash
This command attaches a new shell to the running container. Now, you can source the ROS 2 Jazzy environment using the following command. This command makes the ROS 2 tools visible in the current terminal.
root@eafa922c9072:/# source /opt/ros/jazzy/setup.bash

After sourcing this command, you can run the listener node:
root@eafa922c9072:/# ros2 run demo_nodes_cpp listener

You will get the following output if everything is working well:
[INFO] [1726069883.429240302] [listener]: I heard: [Hello World: 3]
[INFO] [1726069884.429221835] [listener]: I heard: [Hello World: 4]
[INFO] [1726069885.429212317] [listener]: I heard: [Hello World: 5]
Press Ctrl + C to terminate each running node and press Ctrl + D to exit from the shell. After exiting the shell, the container may still be running in the background. You can stop the container using the command in the next section.
Step 4: Starting, stopping, and removing the container
Here is the command to stop the running container:
docker stop master_ros2

If you want to find the status of all containers in your computer, use:
docker ps -a
This will show all the existing containers and their status, such as whether it is stopped or running. After stopping the container, if you want to start it again, you can use the docker start command with the container name:
docker start master_ros2

After starting the container, you can attach a shell using the docker exec command and access the container's shell:
docker exec -it master_ros2 bash

So, once you create a container, you do not need to create it again unless you have any changes in the Docker image. You can simply start and stop the container whenever you want.
If you want to remove the current container, you can use the docker rm command. Make sure you stop the container before you delete it:
docker stop master_ros2
docker rm master_ros2

