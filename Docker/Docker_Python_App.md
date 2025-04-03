A python app needed to be Dockerized, and then it needs to be deployed on App Server 3. We have already copied a requirements.txt file (having the app dependencies) under /python_app/src/ directory on App Server 3.  
Further complete this task as per details mentioned below:  
        Create a Dockerfile under /python_app directory:  
        Use any python image as the base image.  
        Install the dependencies using requirements.txt file.  
        Expose the port 5004.  
        Run the server.py script using CMD.  

Build an image named nautilus/python-app using this Dockerfile.  
Once image is built, create a container named pythonapp_nautilus:  
Map port 5004 of the container to the host port 8099.  
Once deployed, you can test the app using curl command on App Server 3.  
curl http://localhost:8099/    

Solution:  
```
ssh banner@stapp03
sudo su
vi /python_app/Dockerfile
```
Create Dockerfile  
```
FROM python:3.12-alpine
WORKDIR /code
COPY ./src/. ./
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 5004
CMD ["python", "server.py"]
```
Build and run  
```
cd /python_app
docker build -t nautilus/python-app .
docker run -it -d --name pythonapp_nautilus -p 8099:5004 nautilus/python-app
docker ps
curl http://localhost:8099
```

[back](https://github.com/MederD/Kodekloud-Engineer-Tasks)
