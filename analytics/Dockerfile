FROM python:3.9-slim-buster
COPY . /app
WORKDIR /app
# Update the local package index with the latest packages from the repositories
#RUN apt update -y
# Install a couple of packages to successfully install postgresql server locally
#RUN apt install build-essential libpq-dev -y
# Update python modules to successfully build the required modules
#RUN pip install --upgrade pip setuptools wheel
RUN pip install -r requirements.txt
#Run docker build --build-arg DB_USERNAME=*** --build-arg DB_PASSWORD=*** -t <imagename> .
ARG DB_USERNAME=$DB_USERNAME
ARG DB_PASSWORD=$DB_PASSWORD
ENV DB_USERNAME $DB_USERNAME
ENV DB_PASSWORD $DB_PASSWORD
CMD python app.py