# Use the official Ubuntu base image
FROM ubuntu:latest

# Set environment variables to non-interactive to prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Update the package list and install dependencies
RUN apt-get update && \
    apt-get install -y python3 python3-pip python3-venv && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory in the container
WORKDIR /app

# Copy the application code into the container
COPY . /app

# Confirm the requirements.txt file is copied
RUN ls -l /app/requirements.txt

# Create a virtual environment and activate it
RUN python3 -m venv /app/venv

# Install Python dependencies in the virtual environment
RUN /app/venv/bin/pip install --upgrade pip && \
    /app/venv/bin/pip install -r /app/requirements.txt

# Expose the port the app runs on
EXPOSE 8000

# Set the entry point to python3 in the virtual environment and command to run the application
ENTRYPOINT ["/app/venv/bin/python"]
CMD ["manage.py", "runserver", "0.0.0.0:8000"]

