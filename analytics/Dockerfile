# Use an official Python runtime as a parent image
FROM python:3.8

# Set environment variables
ENV PYTHONUNBUFFERED 1
ENV POSTGRES_DB mydb
ENV POSTGRES_USER myuser
ENV POSTGRES_PASSWORD mypassword

# Create a directory for your application
RUN mkdir /app

# Set the working directory to /app
WORKDIR /app

# Copy the contents of the "analytics" folder (app.py, config.py, requirements.txt)
COPY analytics/ /app

# Install any necessary dependencies
RUN pip install -r requirements.txt

# Install PostgreSQL client
RUN apt-get update && apt-get install -y postgresql-client

# Create a directory for your SQL files
RUN mkdir /sql

# Copy the SQL files from the "db" folder into the container
COPY db/ /sql

# Expose any necessary ports
EXPOSE 8000

# Run your application
CMD ["python", "app.py"]