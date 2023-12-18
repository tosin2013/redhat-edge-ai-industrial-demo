# Use an official Python runtime as the base image
FROM quay.io/hgeaydem/ubi9-jetson-pytorch 

# Set the working directory in the container
WORKDIR /app

# Install dependencies
RUN  dnf update -y && \
     dnf install -y libpng gcc-c++ make python3-devel git wget tar && \
     wget https://www.open-mpi.org/software/ompi/v4.1/downloads/openmpi-4.1.1.tar.gz && \
     tar zxf openmpi-4.1.1.tar.gz && \
     cd openmpi-4.1.1 && \
     ./configure --prefix=/usr/local && \
     make all install && \
     ldconfig

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
ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

USER 1001

# Run the script when the container launches
CMD ["python3", "redhat.py"]
