# Hello World

```
docker run hello-world
```

(Command Output)

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
9db2ca6ccae0: Pull complete
Digest: sha256:4b8ff392a12ed9ea17784bd3c9a8b1fa3299cac44aca35a85c90c5e3c7afacdc
Status: Downloaded newer image for hello-world:latest
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

Run the following command to take a look at the container image it pulled from Docker Hub:

```
docker images
```

(Command Output)
```
REPOSITORY     TAG      IMAGE ID       CREATED       SIZE
hello-world    latest   1815c82652c0   6 days ago    1.84 kB
```

Let's run the container again:

```
docker run hello-world
```

(Command Output)

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
To generate this message, Docker took the following steps:
...
```

Finally, look at the running containers by running the following command:

```
docker ps
```

(Command Output)

```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

finished executing:

```
docker ps -a
```

(Command Output)

```
CONTAINER ID      IMAGE           COMMAND      ...     NAMES
6027ecba1c39      hello-world     "/hello"     ...     elated_knuth
358d709b8341      hello-world     "/hello"     ...     epic_lewin
```

The container Names are also randomly generated but can be specified with docker run --name [container-name] hello-world.

# Build

To create and switch into a folder named test.

```
mkdir test && cd test
```

Create a Dockerfile:

```
cat > Dockerfile <<EOF
# Use an official Node runtime as the parent image
FROM node:6
# Set the working directory in the container to /app
WORKDIR /app
# Copy the current directory contents into the container at /app
ADD . /app
# Make the container's port 80 available to the outside world
EXPOSE 80
# Run app.js using node when the container launches
CMD ["node", "app.js"]
EOF
```

This file instructs the Docker daemon on how to build your image.


Run the following to create the node application:

```
cat > app.js <<EOF
const http = require('http');
const hostname = '0.0.0.0';
const port = 80;
const server = http.createServer((req, res) => {
    res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
        res.end('Hello World\n');
});
server.listen(port, hostname, () => {
    console.log('Server running at http://%s:%s/', hostname, port);
});
process.on('SIGINT', function() {
    console.log('Caught interrupt signal and will exit');
    process.exit();
});
EOF
```

Now let's build the image.

Note again the ".", which means current directory so you need to run this command from within the directory that has the Dockerfile:

```
docker build -t node-app:0.1 .
```

Your output should resemble the following:

```
Sending build context to Docker daemon 3.072 kB
Step 1 : FROM node:6
6: Pulling from library/node
...
...
...
Step 5 : CMD node app.js
 ---> Running in b677acd1edd9
 ---> f166cd2a9f10
Removing intermediate container b677acd1edd9
Successfully built f166cd2a9f10
```

The -t is to name and tag an image with the name:tag syntax. The name of the image is node-app and the tag is 0.1. The tag is highly recommended when building Docker images. If you don't specify a tag, the tag will default to latest and it becomes more difficult to distinguish newer images from older ones. Also notice how each line in the Dockerfile above results in intermediate container layers as the image is built.

Now, run the following command to look at the images you built:

```
docker images
```

Your output should resemble the following:

```
REPOSITORY     TAG      IMAGE ID        CREATED            SIZE
node-app       0.1      f166cd2a9f10    25 seconds ago     656.2 MB
node           6        5a767079e3df    15 hours ago       656.2 MB
hello-world    latest   1815c82652c0    6 days ago         1.84 kB
```

Notice node is the base image and node-app is the image you built. 

You can't remove node without removing node-app first. The size of the image is relatively small compared to VMs. Other versions of the node image such as node:slim and node:alpine can give you even smaller images for easier portability.

# Run

To run containers based on the image you built:

```
docker run -p 4000:80 --name my-app node-app:0.1
```

(Command Output)

```
Server running at http://0.0.0.0:80/
```

- The --name flag allows you to name the container if you like. 
- The -p instructs Docker to map the host's port 4000 to the container's port 80. 

Now you can reach the server at http://localhost:4000. Without port mapping, you would not be able to reach the container at localhost.

Open another terminal (in Cloud Shell, click the + icon), and test the server:

```
curl http://localhost:4000
```

(Command Output)

```
Hello World
```

The container will run as long as the initial terminal is running. If you want the container to run in the background (not tied to the terminal's session), you need to specify the -d flag.

Close the initial terminal and then run the following command to stop and remove the container:

```
docker stop my-app && docker rm my-app
```

Now run the following command to start the container in the background:

```
docker run -p 4000:80 --name my-app -d node-app:0.1
docker ps
```

(Command Output)

```
CONTAINER ID   IMAGE          COMMAND        CREATED         ...  NAMES
xxxxxxxxxxxx   node-app:0.1   "node app.js"  16 seconds ago  ...  my-app
```

Notice the container is running in the output of docker ps. You can look at the logs by executing docker logs [container_id].

Tip: You don't have to write the entire container ID, as long as the initial characters uniquely identify the container.

```
docker logs [container_id]
```

(Command Output)

```
Server running at http://0.0.0.0:80/
```

Let's modify the application.

```
cd test
```

Replace "Hello World" with another string:

```
....
const server = http.createServer((req, res) => {
    res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
        res.end('Welcome to Cloud\n');
});
....
```

Build this new image and tag it with 0.2:

```
docker build -t node-app:0.2 .
```

(Command Output)

```
Step 1/5 : FROM node:6
 ---> 67ed1f028e71
Step 2/5 : WORKDIR /app
 ---> Using cache
 ---> a39c2d73c807
Step 3/5 : ADD . /app
 ---> a7087887091f
Removing intermediate container 99bc0526ebb0
Step 4/5 : EXPOSE 80
 ---> Running in 7882a1e84596
 ---> 80f5220880d9
Removing intermediate container 7882a1e84596
Step 5/5 : CMD node app.js
 ---> Running in f2646b475210
 ---> 5c3edbac6421
Removing intermediate container f2646b475210
Successfully built 5c3edbac6421
Successfully tagged node-app:0.2
```

Notice in Step 2 we are using an existing cache layer. From Step 3 and on, the layers are modified because we made a change in app.js.

Run another container with the new image version. Notice how we map the host's port 8080 instead of 80. We can't use host port 4000 because it's already in use.

```
docker run -p 8080:80 --name my-app-2 -d node-app:0.2
docker ps
```

(Command Output)
```

CONTAINER ID     IMAGE             COMMAND            CREATED             
xxxxxxxxxxxx     node-app:0.2      "node app.js"      53 seconds ago      ...
xxxxxxxxxxxx     node-app:0.1      "node app.js"      About an hour ago   ...
```
Test the containers:

```
curl http://localhost:8080
```

(Command Output)
```
Welcome to Cloud
```

And now test the first container you made:

```
curl http://localhost:4000
```

(Command Output)

```
Hello World
```

# Debug

Now that we're familiar with building and running containers, let's go over some debugging practices.

You can look at the logs of a container using docker logs [container_id]. If you want to follow the log's output as the container is running, use the -f option.

```
docker logs -f [container_id]
```

(Command Output)

```
Server running at http://0.0.0.0:80/
```

Sometimes you will want to start an interactive Bash session inside the running container. You can use docker exec to do this. Open another terminal (in Cloud Shell, click the + icon) and enter the following command:

```
docker exec -it [container_id] bash
```

The -it flags let you interact with a container by allocating a pseudo-tty and keeping stdin open. Notice bash ran in the WORKDIR directory (/app) specified in the Dockerfile. From here, you have an interactive shell session inside the container to debug.

(Command Output)

```
root@xxxxxxxxxxxx:/app#
```
Look at the directory

```
ls
```

(Command Output)

```
Dockerfile  app.js
```

Exit the Bash session:

```
exit
```

You can examine a container's metadata in Docker by using Docker inspect:

```
docker inspect [container_id]
```
(Command Output)

```
[
    {
        "Id": "xxxxxxxxxxxx....",
        "Created": "2017-08-07T22:57:49.261726726Z",
        "Path": "node",
        "Args": [
            "app.js"
        ],
...
```

Use --format to inspect specific fields from the returned JSON. For example:

```
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [container_id]
```

(Example Output)

```
192.168.9.3
```

Be sure to check out the following resources for more information on debugging:

- Docker inspect [reference](https://docs.docker.com/engine/reference/commandline/inspect/#examples)
- Docker exec reference [reference](https://docs.docker.com/engine/reference/commandline/exec/)


# Publish

To push your image to the Google Container Registry (gcr). After that you'll remove all containers and images to simulate a fresh environment, and then pull and run your containers. This will demonstrate the portability of Docker containers.

To push images to your private registry hosted by gcr, you need to tag the images with a registry name. 

The format is [hostname]/[project-id]/[image]:[tag].

For gcr:

- [hostname]= gcr.io
- [project-id]= your project's ID
- [image]= your image name
- [tag]= any string tag of your choice. If unspecified, it defaults to "latest".
  
You can find your project ID by running:

```
gcloud config list project
```

(Command Output)

```
[core]
project = [project-id]
Your active configuration is: [default]
```

Tag node-app:0.2. Replace [project-id] with your configuration..

```
docker tag node-app:0.2 gcr.io/[project-id]/node-app:0.2
docker images
```

(Command Output)
```
REPOSITORY                      TAG         IMAGE ID          CREATED
node-app                        0.2         76b3beef845e      22 hours ago
gcr.io/[project-id]/node-app    0.2         76b3beef845e      22 hours ago
node-app                        0.1         f166cd2a9f10      26 hours ago
node                            6           5a767079e3df      7 days ago
hello-world                     latest      1815c82652c0      7 weeks ago
```
Push this image to gcr. Remember to replace [project-id].

```
docker push gcr.io/[project-id]/node-app:0.2
```

Command output (yours may differ):

```
The push refers to a repository [gcr.io/[project-id]/node-app]
057029400a4a: Pushed
342f14cb7e2b: Pushed
903087566d45: Pushed
99dac0782a63: Pushed
e6695624484e: Pushed
da59b99bbd3b: Pushed
5616a6292c16: Pushed
f3ed6cb59ab0: Pushed
654f45ecb7e3: Pushed
2c40c66f7667: Pushed
0.2: digest: sha256:25b8ebd7820515609517ec38dbca9086e1abef3750c0d2aff7f341407c743c46 size: 2419
```

Check that the image exists in gcr by visiting the image registry in your web browser. You can navigate via the console to Navigation menu > Container Registry and click node-app or visit: http://gcr.io/[project-id]/node-app. You should land on a similar page:

![container_registry.png](https://cdn.qwiklabs.com/3zK6t5DQ5tEtApMnM24COKFT2IEPl54kLKR6etaWULE%3D)

Let's test this image. You could start a new VM, ssh into that VM, and install gcloud. For simplicity, we'll just remove all containers and images to simulate a fresh environment.

Stop and remove all containers:

```
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
```

You have to remove the child images (of node:6) before you remove the node image. Replace [project-id].

```
docker rmi node-app:0.2 gcr.io/[project-id]/node-app node-app:0.1
docker rmi node:6
docker rmi $(docker images -aq) # remove remaining images
docker images
```

(Command Output)

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
```

At this point you should have a pseudo-fresh environment. Pull the image and run it. Remember to replace the [project-id].

```
docker pull gcr.io/[project-id]/node-app:0.2
docker run -p 4000:80 -d gcr.io/[project-id]/node-app:0.2
curl http://localhost:4000
```

(Command Output)

Welcome to Cloud
Test Completed Task