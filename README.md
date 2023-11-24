Demo app - Developing with Docker

This demo app shows a simple user profile app set up using 
- index.html with pure js and css styles
- nodejs backend with express module
- mongodb for data storage

All components are docker-based

#### To start the application

Step 1 : Clone the repository

    git clone https://github.com/Arun-hn/Docker_1.git

Step 2: Move to directory where index.html present and edit index.html
        In place of localhost, use ec2-instance-public-ip

        in service.js >> let mongoUrlDocker = "mongodb://admin:password@localhost:27017";
        inplace of localhost use dockercontainer name as mongodb
        
Step 3: Create Docker network

    docker network create mongo-network 

Step 4: start mongodb 

    docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo    

Step 5: start mongo-express
    
    docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express   

_NOTE: creating docker-network in optional. You can start both containers in a default network. In this case, just emit `--net` flag in `docker run` command_


Step 5: To build a docker image from the application

    docker build -t my-app .       
    
The dot "." at the end of the command denotes location of the Dockerfile


Step 7 : Run the container 

    docker run -d -p 3000:3000 --network=mongo-network  --name app my-app
