# Use an official Python runtime as the base image
FROM quay.io/hgeaydem/ubi9-jetson-pytorch 

# Set the working directory in the container
WORKDIR /app

# Install dependencies
RUN dnf upgrade -y && \
    dnf install -y gcc gcc-c++ gcc-gfortran make git wget tar \
                 python3-devel openssh-clients openssh-server libtool \
                 autoconf automake libpng numactl && \
    dnf clean all

# Install OpenMPI
RUN wget https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-4.1.4.tar.gz && \
    tar zxf openmpi-4.1.4.tar.gz && \
    cd openmpi-4.1.4 && \
    ./configure --prefix=/usr/local/openmpi --host=aarch64-redhat-linux CC=gcc CXX=g++ \
            --enable-mpi-cxx --enable-mpi-fortran --with-numa && \
    make -j$(nproc) all && \
    make install && \
    cd .. && \
    rm -rf openmpi-4.1.4*
    
ENV PATH=/usr/local/openmpi/bin:$PATH
ENV LD_LIBRARY_PATH=/usr/local/openmpi/lib:$LD_LIBRARY_PATH
ENV MANPATH=/usr/local/openmpi/share/man:$MANPATH

COPY . /app

# Install gevent and gevent-websocket
RUN pip install gevent gevent-websocket

# Clone the YOLOv5 repository and install its requirements
RUN git clone https://github.com/ultralytics/yolov5 && \
    cd yolov5 && \
    sed -i s/'opencv-python>'/'opencv-python-headless>'/g requirements.txt && \
    pip install -r requirements.txt && \
    pip uninstall -y opencv-python && \
    pip install --force-reinstall opencv-python-headless 

# Install Python dependencies
RUN pip install pip install  Pillow numpy flask flask_socketio eventlet gevent gevent-websocket

# Expose the port if needed
EXPOSE 5000 

# Set environment variables if needed
#ENV LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

USER 1001

# Run the script when the container launches
CMD ["python3", "redhat.py"]
