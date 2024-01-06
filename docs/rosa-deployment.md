# Rosa Deployment Instructions 


![20240106111312](https://i.imgur.com/AGBg7uY.jpg)

## Prerequisites
* Ensure cluster is updated to 4.13


## SSH into RHEL jumpbox
```
ssh  cloud-user@<jumpbox_ip>
```

## Optioanl run the script below to install dependencies
```
curl -OL https://raw.githubusercontent.com/tosin2013/redhat-edge-ai-industrial-demo-infra/main/dev-box.sh
chmod +x dev-box.sh
./dev-box.sh
```

## Configure ArgoCD 
```
cd $HOME/redhat-edge-ai-industrial-demo-infra
oc create -k clusters/overlays/rosa
```


## Run Tekton Pipeline
* [Run tekton pipeline](run-tekton-pipeline.md)

![20240106111607](https://i.imgur.com/SH87x22.png)

![20240106134547](https://i.imgur.com/ssgQacx.png)