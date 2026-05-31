*Installing ROS 2 Jazzy in Docker*
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
