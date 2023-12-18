# Use an official Python runtime as the base image
FROM quay.io/hgeaydem/ubi9-jetson-pytorch 

# Set the working directory in the container
WORKDIR /app

# Install dependencies
RUN dnf install -y libpng gcc-c++ make python3-devel git

# Create a non-root user
RUN useradd -ms /bin/bash user

# Switch to the non-root user
USER user

# Copy the code into the container
COPY . /app

# Install gevent and gevent-websocket
RUN pip install gevent gevent-websocket

# Install Python dependencies
RUN pip install opencv-python-headless Pillow numpy flask flask_socketio eventlet gevent gevent-websocket

# Clone the YOLOv5 repository and install its requirements
RUN git clone https://github.com/ultralytics/yolov5 && \
    cd yolov5 && \
    sed -i s/'opencv-python>'/'opencv-python-headless>'/g requirements.txt && \
    pip install -r requirements.txt && \
    pip uninstall -y opencv-python && \
    pip install --force-reinstall opencv-python-headless 

# Expose the port if needed
EXPOSE 5000 

# Set environment variables if needed
ENV LD_LIBRARY_PATH=/usr/lib64:/usr/lib64/openmpi/lib/:$LD_LIBRARY_PATH

# Run the script when the container launches
CMD ["python3", "redhat.py"]
