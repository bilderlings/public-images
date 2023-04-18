FROM python:3-alpine

RUN apk add bash curl

WORKDIR /

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt
