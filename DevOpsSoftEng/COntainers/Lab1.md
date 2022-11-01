### Objectives

In this lab, you will:

    - Pull an image from Docker Hub
    - Run an image as a container using docker
    - Build an image using a Dockerfile
    - Push an image to IBM Cloud Container Registry

Verify the environment and command line tools

    1. Open a terminal window by using the menu in the editor: Terminal > New Terminal.

    2. Verify that docker CLI is installed.

        - docker --version

    3.Verify that ibmcloud CLI is installed.

    4. Change to your project folder.

        Note: If you are already on the '/home/project' folder, please skip this step.

      cd /home/project

    5. Clone the git repository that contains the artifacts needed for this lab, if it doesn't already exist.

        [ ! -d 'CC201' ] && git clone https://github.com/ibm-developer-skills-network/CC201.git



![imagen](https://user-images.githubusercontent.com/63612112/199302310-ea507e68-4939-4d8e-a717-6da01d63e53c.png)


    6. Change to the directory for this lab by running the following command. cd will change the working/current directory to the directory with the name specified, in this case CC201/labs/1_ContainersAndDcoker.

cd CC201/labs/1_ContainersAndDocker/

    7. List the contents of this directory to see the artifacts for this lab.
    8. Change to the directory for this lab by running the following command.
      cd will change the working/current directory to the directory with the name specified, 
      in this case CC201/labs/1_ContainersAndDcoker.

Pull an image from Docker Hub and run it as a container

    Use the docker CLI to list your images.

docker images

You should see an empty table (with only headings) since you don't have any images yet.


    Pull your first image from Docker Hub.

docker pull hello-world
![imagen](https://user-images.githubusercontent.com/63612112/199303234-f2c35703-aaa8-4997-b1b5-bf7b4efda73f.png)


    List images again.

docker images

You should now see the hello-world image present in the table.


    Run the hello-world image as a container.

docker run hello-world

You should see a 'Hello from Docker!' message.

There will also be an explanation of what Docker did to generate this message.


    List the containers to see that your container ran and exited successfully.

docker ps -a

Among other things, for this container you should see a container ID, the image name (hello-world), and a status that indicates that the container exited successfully.


    Note the CONTAINER ID from the previous output and replace the \ tag in the command below with this value. This command removes your container.

docker container rm <container_id>


   ![imagen](https://user-images.githubusercontent.com/63612112/199303716-ce79870c-b1fd-4eb6-9faf-eff20391f593.png)

   Verify that that the container has been removed. Run the following command.

docker ps -a

Build an image using a Dockerfile

    The current working directory contains a simple Node.js application that we will run in a container. The app will print a hello message along with the hostname. The following files are needed to run the app in a container:

    app.js is the main application, which simply replies with a hello world message.
    package.json defines the dependencies of the application.
    Dockerfile defines the instructions Docker uses to build the image.

    Use the Explorer to view the files needed for this app. Click the Explorer icon (it looks like a sheet of paper) on the left side of the window, and then navigate to the directory for this lab: CC201 > labs > 1_ContainersAndDocker. Click Dockerfile to view the commands required to build an image.

Dockerfile in Explorer


Congratulations on pulling an image from Docker Hub and running your first container! Now let's try and build our own image.
![imagen](https://user-images.githubusercontent.com/63612112/199304321-a2e95af0-d401-4329-8f4d-2f8efe48bfa6.png)
Build an image using a Dockerfile

    The current working directory contains a simple Node.js application that we will run in a container. The app will print a hello message along with the hostname. The following files are needed to run the app in a container:

    app.js is the main application, which simply replies with a hello world message.
    package.json defines the dependencies of the application.
    Dockerfile defines the instructions Docker uses to build the image.

    Use the Explorer to view the files needed for this app. Click the Explorer icon (it looks like a sheet of paper) on the left side of the window, and then navigate to the directory for this lab: CC201 > labs > 1_ContainersAndDocker. Click Dockerfile to view the commands required to build an image.


docker build . -t myimage:v1
docker images

You can refresh your understanding of the commands mentioned in the Dockerfile below:
The FROM instruction initializes a new build stage and specifies the base image that subsequent instructions will build upon.
The COPY command enables us to copy files to our image.
The RUN instruction executes commands.
The EXPOSE instruction exposes a particular port with a specified protocol inside a Docker Container.
The CMD instruction provides a default for executing a container, or in other words, an executable that should run in your container. 

Run the image as a container

    Now that your image is built, run it as a container with the following command:

docker run -dp 8080:8080 myimage:v1


The output is a unique code allocated by docker for the application you are running.

    Run the curl command to ping the application as given below.

curl localhost:8080


If you see the output as above, it indicates that 'Your app is up and running!'.

    Now to stop the container we use docker stop followed by the container id. The following command uses docker ps -q to pass in the list of all running containers:

docker stop $(docker ps -q)


    Check if the container has stopped by running the following command.

docker ps

Push the image to IBM Cloud Container Registry

    The environment should have already logged you into the IBM Cloud account that has been automatically generated for you by the Skills Network Labs environment. The following command will give you information about the account you're targeting:

ibmcloud target


    The environment also created an IBM Cloud Container Registry (ICR) namespace for you. Since Container Registry is multi-tenant, namespaces are used to divide the registry among several users. Use the following command to see the namespaces you have access to:

ibmcloud cr namespaces


You should see two namespaces listed starting with sn-labs:

    The first one with your username is a namespace just for you. You have full read and write access to this namespace.
    The second namespace, which is a shared namespace, provides you with only Read Access

    Ensure that you are targeting the region appropriate to your cloud account, for instance us-south region where these namespaces reside as you saw in the output of the ibmcloud target command.

ibmcloud cr region-set us-south


    Log your local Docker daemon into IBM Cloud Container Registry so that you can push to and pull from the registry.

ibmcloud cr login


    Export your namespace as an environment variable so that it can be used in subsequent commands.

export MY_NAMESPACE=sn-labs-$USERNAME


    Tag your image so that it can be pushed to IBM Cloud Container Registry.

docker tag myimage:v1 us.icr.io/$MY_NAMESPACE/hello-world:1


    Push the newly tagged image to IBM Cloud Container Registry.

docker push us.icr.io/$MY_NAMESPACE/hello-world:1


    Note: If you have tried this lab earlier, there might be a possibility that the previous session is still persistent. In such a case, you will see a 'Layer already Exists' message instead of the 'Pushed' message in the above output. We recommend you to proceed with the next steps of the lab.

    Verify that the image was successfully pushed by listing images in Container Registry.

ibmcloud cr images


Optionally, to only view images within a specific namespace.

ibmcloud cr images --restrict $MY_NAMESPACE


You should see your image name in the output.

Congratulations! You have completed the second lab for the first module of this course.
Changelog
Date 	Version 	Changed by 	Change Description
2022-04-08 	1.1 	K Sundararajan 	Updated Lab instructions
2022-04-19 	1.2 	K Sundararajan 	Updated Lab instructions
2022-08-26 	1.3 	K Sundararajan 	Updated Lab instructions
© IBM Corporation 2022. All rights reserved. 

![imagen](https://user-images.githubusercontent.com/63612112/199305014-f96a7aaf-a74a-4f6f-8da2-6a98310f3dc2.png)
