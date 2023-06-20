![Nut Demo](images/screenshot.png)

# Edge Computer vision demo of identifying defects on Nuts

The project involves developing a computer vision-based program that can identify black marks on nuts representing defects or impurities. 
The program is designed to take input images of nuts as input and process them using YOLO V5 model to identify and localize black marks on the nuts. The orchestration  aspect using Microshift ensures that the program can be easily deployed and run on different platforms and environments without worrying about compatibility issues. By leveraging computer vision and Microshift, the project aims to provide an efficient and scalable solution for identifying black marks on nuts, which can contribute to quality control processes in industries such as food production or manufacturing.

To further enhance the project's automation capabilities, an Ansible automation platform is utilized. Ansible allows for the automation of various tasks, including the creation of the edge image and deployment and configuration of the computer vision program on OpenShift. It streamlines the setup process, making it more efficient and reducing manual effort.



## Model creation with Openshift AI

The first step of the project involves training a model on Red Hat AI, an artificial intelligence platform provided by Red Hat. Red Hat AI offers a range of tools and frameworks for developing and training machine learning models.

To train the model, the first task is to collect and label a dataset of images containing nuts with and without black marks. These images serve as the training data for the model. The dataset would include images of different types of nuts, captured from various angles and lighting conditions. For achieving that the best option is to film the nuts using a camera with the same resolution as the final camera that will be used. This is required as the model is trained with predefined resolution and images are resized by the training algorith. Feeding a model with images's resolution that differs from the training dataset will reduce the model's accuracy.

We will train Using Red Hat OpenShift Data Science that leverage popular deep learning frameworks like TensorFlow or PyTorch to build and train a neural network model. The model architecture is designed to learn the visual patterns and features associated with the presence of black marks on nuts. The training process involves feeding the labeled images into the model, allowing it to learn and adjust its internal parameters to make accurate predictions.

Once the model is trained on Red Hat OpenShift Data Science, it can be saved and exported for further deployment and inference on the target system, such as in a container that will run on a the micro kubernetes cluster Microshift. The trained model becomes a valuable asset for the subsequent steps of the project, where it will be utilized for identifying black marks on nuts in real-world images.



## Download the Jupyter Lab


[Train_Nut](Train_Nut.ipynb)



# Content of the Jupyter Lab

## Clone the Repository

This cell clones the repository from the URL "https://github.com/bdherouville/redhat-edge-ai-industrial-demo.git" using the `git clone` command. The repository contains the code and data for the Red Hat Edge AI Industrial Demo.

```
!git clone https://github.com/bdherouville/redhat-edge-ai-industrial-demo.git
```


## Clone the YOLOv5 Repository

This cell clones the YOLOv5 repository from the URL "https://github.com/ultralytics/yolov5" using the `git clone` command. YOLOv5 is a popular object detection algorithm used for training and inference on image and video data.

```
!git clone https://github.com/ultralytics/yolov5 
```

## Modify the YOLOv5 Requirements

This cell modifies the YOLOv5 requirements by replacing the package `opencv-python` with `opencv-python-headless` in the `requirements.txt` file. This modification is done to make Jupyter able to run the python opencv code.

```
!cd yolov5 && sed -i s/'opencv-python>'/'opencv-python-headless>'/g requirements.txt
```

## Uninstall OpenCV Packages

This cell uninstalls the OpenCV packages `opencv-python` and `opencv-python-headless` using the `pip uninstall` command with the `-y` flag for automatic confirmation. This step ensures that any existing installations of OpenCV packages are removed.

```
!pip uninstall -y opencv-python opencv-python-headless
```

## Install YOLOv5 Requirements

This cell installs the required packages for YOLOv5 by running the `pip install` command with the `-r` flag and specifying the `requirements.txt` file. This step ensures that all the necessary dependencies for YOLOv5 are installed.


```
!cd yolov5 && pip install -r requirements.txt  # install
```

## Train the Model

This cell navigates to the `yolov5` directory and runs the command `python train.py` to start training the YOLOv5 model. The training process is performed with specific settings such as image size set to 1280, number of epochs to 10, batch size 10, dataset location, and pre-trained weights.


```
!cd /opt/app-root/src/yolov5 && python train.py --img 1280 --epochs 10 --batch-size 10 --data /opt/app-root/src/redhat-edge-ai-industrial-demo/dataset/images_annotated/YOLODataset/dataset.yaml --weights yolov5s.pt
```


## Move the Best Model

This cell finds the file named `best.pt` in the current directory and moves it to the directory `/opt/app-root/src/redhat-edge-ai-industrial-demo/model/nut.pt`. This step is performed using the `find` command combined with the `mv` command. The `best.pt` file represents the trained model with the best performance during training and is renamed as `nut.pt` in the target directory for further usage.


# Workload container


## Create the container


The command podman create is used to create a container, and -t specifies the name for the container as 'nut'. The dot (.) at the end represents the build context, indicating that the container should be created using the current directory as the build context.

```
podman create -t nut .
```

## Tag the container to the registry's tag

The following podman command podman tag is used to assign a new tag to the container and amke us able to push the container to the registry.
```
podman tag -t localhost:nut your-registry-name/container:tag
```

## Push the container to the registry.

This podman command push is used to upload the container to the specified registry. The container is identified by its name and tag, which are provided as your-registry-name/container:tag.

```
podman push your-registry-name/container:tag
```

# Microshift Deployment


deployment.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nut-detection
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nut-detection
  template:
    metadata:
      labels:
        app: nut-detection
    spec:
      containers:
        - name: nut-detection
          image: quay.io/bertrand_dherouville/nut:latest
          runtime: nvidia
          ports:
            - containerPort: 5000
          securityContext:
            allowPrivilegeEscalation: false
            capabilities:
              drop: ["ALL"]
            seccompProfile:
              type: RuntimeDefault
            runAsNonRoot: true
      hostAliases:
        - ip: "192.168.0.198"
          hostnames:
          - "rpi4.domain.local"
      args:
        - "/etc/hosts"
```

svc.yaml
```
apiVersion: v1
kind: Service
metadata:
  name: nut-detection-service
spec:
  selector:
    app: nut-detection
  ports:
    - protocol: TCP
      port: 5000
      targetPort: 5000
      nodePort: 30000   # Specify the desired NodePort value here
  type: NodePort       # Use NodePort type for the service
```