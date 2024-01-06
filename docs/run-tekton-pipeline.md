# Run tekton pipeline

## Prerequisites
* Configure Secret for quay regisgtry -> use this link as reference [configure-pipeline-secret.sh](https://raw.githubusercontent.com/tosin2013/redhat-edge-ai-industrial-demo-infra/main/hack/configure-pipeline-secret.sh)


## Run pipeline against quay.io
```
curl -OL https://raw.githubusercontent.com/tosin2013/redhat-edge-ai-industrial-demo-infra/main/hack/run_pipeline.sh
chmod +x run_pipeline.sh

USERNBAMRE=admin
./run_pipeline.sh quay.io/admin/redhat-edge-ai-industrial-demo
```

## Run pipeline against self hosted quay on openshift
```
curl -OL https://raw.githubusercontent.com/tosin2013/redhat-edge-ai-industrial-demo-infra/main/hack/run_pipeline.sh
chmod +x run_pipeline.sh

./run_pipeline.sh $(oc get route -n quay | grep registry-quay | awk '{print $2}' | head -1)/admin/redhat-edge-ai-industrial-demo
```
./run_pipeline.sh quay.io/admin/redhat-edge-ai-industrial-demo

