# AWS Blank Environment Instructions  

## Prerequisites
* RHEL 8.x jumpbox that is registred with Red Hat
* Review and run the [openshift-ai-workload.sh](https://gist.githubusercontent.com/tosin2013/76e47de3f32de4486ab4699c21b2188e/raw/e2e8fedda029e699cc522a3e607b40c37ea01ee5/openshift-ai-workload.sh)
```
Export the following AWS variables before running this script:
export aws_access_key_id="YOUR_ACCESS_KEY_ID"
export aws_secret_access_key="YOUR_SECRET_ACCESS_KEY"
export aws_region="YOUR_AWS_REGION"

curl -OL https://gist.githubusercontent.com/tosin2013/76e47de3f32de4486ab4699c21b2188e/raw/e2e8fedda029e699cc522a3e607b40c37ea01ee5/openshift-ai-workload.sh
chmod +x openshift-ai-workload.sh
./openshift-ai-workload.sh
```

## Configure jumpbox for deployment
```
curl -OL https://raw.githubusercontent.com/tosin2013/redhat-edge-ai-industrial-demo-infra/main/dev-box.sh
chmod +x dev-box.sh
./dev-box.sh
```


## For deployments without GPU
```
cd $HOME/redhat-edge-ai-industrial-demo-infra
oc create -k clusters/overlays/aws
```

## For deployments with GPU
```
cd $HOME/redhat-edge-ai-industrial-demo-infra
oc create -k clusters/overlays/aws-gpu
```

