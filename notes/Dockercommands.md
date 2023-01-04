## Docker commands

- List running containers

```
docker ps
```

- List history of all continers

```
docker ps --all
```
- To build a Docker file

```
docker build .
```

- Tagging a docker image

```
docker tag <imageid> DOCKERID/NAME:VERSION
eg: docker tag beae ozer550/busy-box-copy:2
```
- Removing a Docker container (soft)
```
docker rm <Container_ID>
```
- Removing a docker container (forcefully)
```
docker rm -f < Container_ID>
```
- Removing a pulled docker image
```
docker rmi -f <Image_name>
```
- View downloaded or pulled docker images

```
docker images
```

- Tagging a docker image while building

```
docker build -t DOCKERID/NAME:VERSION .
```

- Build container according to a specefic file with diffrent name then Dockerfile.

```
docker build -f Dockerfile.dec .
```

## Creating a image out of running docker
As we can run a container out of a given image the reverse is also possible and thats what we see in build process, we can create an image out of a container by taking its snapshot, the command to do that is:

```
docker commit -c 'CMD ["redis-server"]' <CONTAINER ID>
```
here we use -c flag to specify a start command which in our case is redis-server above.


## docker commands with -it flag

In the upcomming command we will use -it flag which actually consist of 2 flags that is -i and -t
* -i flag: is used to bind the terminal input to STDIN of the docker process that is running it means to say that feed the input command that we type in terminal to stdin of the process.
* -t flag: is used to pretify or the output of the processs to the terminal it has many more than that but this is what we need.
* -it flag: is the combination of both flags written above.

- Execute an additional command in in running docker container

```
docker exec -it <container id> <command>
```

- Execute shell inside a container

```
docker exec -it <container id> sh
```

- Start a container with shell

```
docker run -it <name of the image> sh
```

## Basic Docker File Structure 

![dockerfilestruct](https://user-images.githubusercontent.com/51698593/149553935-c7166baf-6a7f-4c18-8637-4e509561432a.jpg)

This is how a basic docker file structure looks like it has 3 parts:
- A base image for our container is to be stated (FROM)
- Some commands to download and install dependecies on that base image (RUN)
- Start up command for our final container (CMD)

Above example uses alpine as a base image and goes through following process:
### Step 1 (base image creation FROM command)
![Screenshot from 2022-01-14 23-01-11](https://user-images.githubusercontent.com/51698593/149559750-b3e6a11b-1a7b-40c7-8250-b5107e9ea400.png)

- Looks if the base image in our case image for alpine already exist locally, if not it downloads it from the docker server.
- Creates a temoprary container with the file system as that of alpine base image.
- Takes a screenshot of the container that was created and creates an image out of it.
- Deletes the temporary container.
### step 2 (downloading and installing dependencies specified RUN command)
![Screenshot from 2022-01-14 23-01-25](https://user-images.githubusercontent.com/51698593/149559799-26d7a520-8f4c-4574-83d5-74c08f2c8841.png)

- Now we create a temporary container out of the image created in step 1.
- We download and install dependencies specified by RUN command.
- In our case it downloads and installs redis with the package manager provided in alpine.
- Take snap shot of file system.
- Create an image representing the temporary container that has both base image and dependencies installed. 
- Deletes the temporary container.
### step 3 (embending the start command specified by CMD in the file to image created in step 2)
![Screenshot from 2022-01-14 23-03-03](https://user-images.githubusercontent.com/51698593/149559829-4e1a6cbd-3a03-475f-ae1f-58e2df4b244a.png)

- We take the image created in step 2 and create a temporary container out of it.
- We embed the start command specified in CMD in the file.
- In our case its starting up the redis server whenever we create container out of final image.
- Create an image out of the temporary container that has all 3 things base image, dependencies downloaded and installed, and a start command embeded.
- Deletes the temporary container.

NOTE: Docker behind the scenes caches the images created during build process(to make build process fast) so when second time we build the file it uses the cached images untill the command flow is changed compared to previous builds. for example in the above example if we add another line RUN apk -add update gcc then the build process will use the cached images untill the first run command that is untill downloading and installing redis after that as there is a new command from previous build to add gcc it will then create a proper new image out of the previously cahced image.

## Copying the current working directory to the containers working directory
![Screenshot from 2022-01-15 15-21-34](https://user-images.githubusercontent.com/51698593/149617678-b0e62e4e-8848-454b-be4d-48b8fe8c6164.png)

In the above example we are copying the contents of the working directory of where our Dockerfile exist to the current working directory of container. This is neccesary as the container will not know about the files that are present in the directory where Dockerfile exist and this may causes errors as many things depend upon files that are located in the working directory same as that of Dockerfile. For example in the above case there will be error popping up saying no package.json file found as the initial container would not know that a file named package.json exist while executing `npm install` so we COPY contents of the current working directory to Current working directory of container line 5.

The containers root folder will look like this:

![Screenshot from 2022-01-15 16-07-43](https://user-images.githubusercontent.com/51698593/149618764-365e9a99-0002-487f-91a2-006de6cacd29.png)


## Port Mapping 

![Screenshot from 2022-01-15 15-38-44](https://user-images.githubusercontent.com/51698593/149618230-68bc7841-e985-498e-a9ad-3d5248960216.png)

As we know that the cointaners are somewhat of an isolated operating systems, there is a subtle bug which will arise when we try to open localhost:8080 or any web application hosted on some port. When we run npm start in the container it will say hosting on port 8080, now this port is very difrrent from our local machines 8080 port and when we try to acces this on our local machine via browser it wont show up. This is due to fact that containers ports are isolated and are very different than local machines port. Hence we need to do port forwarding meanning that we need to map the containers port to local machines port of the user so that they can see the result in the browser. This can be done by the following command (port forwarding is done in runtime).

![Screenshot from 2022-01-15 15-38-49](https://user-images.githubusercontent.com/51698593/149618446-a09c3f9d-9f16-4948-8b85-5103dd706a20.png)


```
docker run -p 5000:8080  <image id>
```
here we are mappping 8080 port of container to 5000 port of local machine.

## Specifing a working directory in container 

We can specify the working directory in the container by adding this (line 4) in the Dockerfile:

![Screenshot from 2022-01-15 16-10-39](https://user-images.githubusercontent.com/51698593/149619045-dfed1133-82ec-4a68-be08-3182955c51b3.png)

If the specified directory does not exist then it will create on and then workon on it. In the above exapmle the working directory is set to /usr/app. All the commands that will be execute are also respective to this directory.

## Change in Syntax in Dockerfile for build optimization
![Screenshot from 2022-01-15 16-30-56](https://user-images.githubusercontent.com/51698593/149619509-bfd08243-2ee1-4605-9b8d-821e4c499a34.png)

The lines 7,8 and 9 are the right way to setup Dockerfile for build optimizations as when we do docker build after changing some code  we need not run to npm install while building this reduces build time and unneccesary installing dependencies again.

## Docker compose

![Screenshot from 2022-01-16 20-00-38](https://user-images.githubusercontent.com/51698593/149822925-20c86b3f-9559-483f-8941-03e78089ca54.png)

Docker Compose is a tool that was developed to help define and share multi-container applications. With Compose, we can create a YAML file to define the services and with a single command, can spin everything up or tear it all down.



![Screenshot from 2022-01-16 20-03-42](https://user-images.githubusercontent.com/51698593/149823398-9b8b3ecc-5c16-462f-8469-1e68296bea9c.png)

docker-compose.yml is kind of like make file.


![Screenshot from 2022-01-18 00-47-39](https://user-images.githubusercontent.com/51698593/149826366-97335de8-bcaf-43a2-9f7a-e6470428ea64.png)


This is a minimilistic docker-compose.yml file.

- build the yml file 

```
docker-compose up --build 
```
- shut down containers 

```
docker-build down
```

- List running dockers (must be in same directory as .yml file)

```
docker-compose ps
```

## Docker Volumes

There is quite a big problem in our primitive setup of docker files. We know that a container is basically snapshot of our directory. The real problem arises here as whenever we have change in source code we need to rebuild the image everytime to refelect the changes. WE EVERYTIME COPY THE DIRECTORY TO THE CONTAINER WHICH IS INSANE!!!!. This is where docker volumes come into play. what they do is they map the folder in container to the main directory in local machine. So basically there is a reference to the main folder in local machine within the container. Hence everytime we make changes to our source directory it gets refelcted in the container. Its like pointer refrencing in C or like port mapping.


![Screenshot from 2022-01-19 16-17-43](https://user-images.githubusercontent.com/51698593/150119742-2030501c-f45d-48e5-b858-80469ece946d.png)

We can create volumes by the following command:

![Screenshot from 2022-01-19 16-18-58](https://user-images.githubusercontent.com/51698593/150119824-eba1b331-f9df-4563-a4f6-c4c29737f739.png)

- The -v flag is used to specify the volumes
- In the above command first we use -v flag to leave the /node_modules folder and do not map it to refrence this is neccesary as the node_modules folder recides in the container itself it does not need any referencing.
- In the second -v flag is where the real refrencing happens. we map pwd of the local machine to the /app folder of container.

BUT THIS COMMAND IS TOO LONG ISINT IT? LETS USE DOCKER-COMPOSE THEN:

![Screenshot from 2022-01-19 16-34-27](https://user-images.githubusercontent.com/51698593/150121415-9f612e51-03bb-4856-90c8-b47928a49815.png)

Here we have added two sub-feilds in build:
- context: tells where to look for main directory that will get copied.
- dockerfile specifies what file to build upon

Also there is a field for volumes:
- first line with no semicolon tells to not to refrence the node_modules folder
- second line specifies the mapping 



