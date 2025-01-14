FROM alpine:latest # base docker image

# Install python and pip 
RUN apk add --update py-pip

# upgrade pip
# RUN pip install --upgrade pip 

COPY test.txt . 

EXPOSE 5000 

CMD ["cat", "test.txt"]