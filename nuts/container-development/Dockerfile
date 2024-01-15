# Use Red Hat Universal Base Image (UBI) 9 as the base image
FROM registry.access.redhat.com/ubi9/ubi:latest

# Install system dependencies
RUN dnf install -y gcc git  gcc-c++ python3-devel libffi-devel && \
    dnf clean all

# Install pip using ensurepip
RUN python3.9 -m ensurepip && \
    python3.9 -m pip install --upgrade pip

# Optional: Enable EPEL Repository for additional packages
# RUN dnf install -y 'dnf-command(config-manager)' && \
#     dnf config-manager --set-enabled ol8_codeready_builder && \
#     dnf install -y libfdk-aac-devel libogg-devel libsndfile-devel python39-pip python39-devel

# Install PyTorch with additional support for NVIDIA GPU Cloud (NGC)
RUN pip3 install torch --extra-index-url https://pypi.ngc.nvidia.com
RUN pip3 install gevent
# Install additional Python dependencies
RUN pip3 install torchvision \
    opencv-python-headless Pillow numpy flask flask_socketio eventlet gevent gevent-websocket

# Environment variables for CUDA support (adjust if necessary)
ENV TORCH_CUDA_ARCH_LIST="8.6"
ENV FORCE_CUDA="1"
ENV VIDEO_URL="http://rpi4.dherouville.home:8081/"

# Set the working directory in the container
WORKDIR /app

# Copy the code into the container
#COPY . /app
COPY redhat.py  /app
COPY templates /app/templates
COPY redhat.py_websocket /app
COPY nut.pt /app

# Clone and install YOLOv5 with the necessary modifications
RUN git clone https://github.com/ultralytics/yolov5 && cd yolov5 && \
    sed -i s/'opencv-python>'/'opencv-python-headless>'/g requirements.txt && \
    pip3 install -r requirements.txt && pip3 uninstall -y opencv-python && \
    pip3 install --force-reinstall opencv-python-headless


# Expose the port for Flask app
EXPOSE 5000

# Set environment variables if needed
#ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LmIBRARY_PATH

USER 1001

# Run the script when the container launches
CMD ["python3", "redhat.py"]