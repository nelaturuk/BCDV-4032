FROM ubuntu:latest

# Install python and pip 
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install -y curl

# upgrade pip
# RUN pip install --upgrade pip 

COPY test.txt . 

EXPOSE 5000 

CMD ["/bin/bash"]