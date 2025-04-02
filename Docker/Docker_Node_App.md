There is a requirement to Dockerize a Node app and to deploy the same on App Server 3. Under /node_app directory on App Server 3, we have already placed a package.json file that describes the app dependencies and server.js file that defines a web app framework.  
Create a Dockerfile (name is case sensitive) under /node_app directory:  
Use any node image as the base image.  
Install the dependencies using package.json file.  
Use server.js in the CMD.  
Expose port 6000.  
The build image should be named as nautilus/node-web-app.  
Now run a container named nodeapp_nautilus using this image.  
Map the container port 6000 with the host port 8092.  
Once deployed, you can test the app using a curl command on App Server 3:  
curl http://localhost:8092  

Solution:  
```
ssh banner@stapp03
sudo su
vi /node_app/Dockerfile
```
Create Dockerfile  
```
FROM node:18-alpine
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 6000
CMD ["node", "server.js"]
```
Build and run  
```
docker build -t nautilus/node-web-app /node_app
docker run -it -d --name nodeapp_nautilus -p 8092:6000 nautilus/node-web-app
docker ps
curl http://localhost:8092
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)


