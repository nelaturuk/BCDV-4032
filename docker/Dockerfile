<<<<<<< HEAD
FROM ubuntu:latest

# Install python and pip 
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install -y curl
=======
FROM alpine:latest # base docker image

# Install python and pip 
RUN apk add --update py-pip
>>>>>>> 8f5a9cd6c086628c661582ed8a10e4cf5f4e6cf0

# upgrade pip
# RUN pip install --upgrade pip 

COPY test.txt . 

EXPOSE 5000 

<<<<<<< HEAD
CMD ["/bin/bash"]
=======
CMD ["cat", "test.txt"]
>>>>>>> 8f5a9cd6c086628c661582ed8a10e4cf5f4e6cf0
