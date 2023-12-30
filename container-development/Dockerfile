# Use Red Hat Universal Base Image (UBI) 9 as the base image
FROM registry.access.redhat.com/ubi9/ubi:latest

# Install system dependencies
RUN dnf install -y cmake gcc-c++ git \
    atlas-devel libfdk-aac-devel libogg-devel libsndfile-devel libpng-devel \
    python39 python39-pip python39-devel && \
    dnf clean all

# Install PyTorch with additional support for NVIDIA GPU Cloud (NGC)
RUN pip3 install torch --extra-index-url https://pypi.ngc.nvidia.com


# Install additional Python dependencies
RUN pip3 install torchvision \
    opencv-python-headless Pillow numpy flask flask_socketio eventlet gevent gevent-websocket

# Clone and install YOLOv5 with the necessary modifications
RUN git clone https://github.com/ultralytics/yolov5 && cd yolov5 && \
    sed -i s/'opencv-python>'/'opencv-python-headless>'/g requirements.txt && \
    pip3 install -r requirements.txt && pip3 uninstall -y opencv-python && \
    pip3 install --force-reinstall opencv-python-headless

# Environment variables for CUDA support (adjust if necessary)
ENV TORCH_CUDA_ARCH_LIST="8.6"  
ENV FORCE_CUDA="1"

# Set the working directory in the container
WORKDIR /app

# Copy the code into the container
COPY . /app

# Expose the port for Flask app
EXPOSE 5000 

# Run the script when the container launches
CMD ["python3", "redhat.py"]
