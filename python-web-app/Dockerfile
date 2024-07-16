# Use the official Ubuntu base image
FROM ubuntu:latest

# Set environment variables to non-interactive to avoid user prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Update package list and install Python3 and pip
RUN apt-get update && \
    apt-get install -y python3 python3-pip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip3 install --no-cache-dir -r requirements.txt

# Make port 8000 available to the world outside this container
EXPOSE 8000

# Run manage.py with runserver command when the container launches
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]

