# Run tekton pipeline
![20240106134547](https://i.imgur.com/ssgQacx.png)

## Prerequisites
* Configure Secret for quay regisgtry -> use this link as reference [configure-pipeline-secret.sh](https://raw.githubusercontent.com/tosin2013/redhat-edge-ai-industrial-demo-infra/main/hack/configure-pipeline-secret.sh)


## Run pipeline against quay.io
```
curl -OL https://raw.githubusercontent.com/tosin2013/redhat-edge-ai-industrial-demo-infra/main/hack/run_pipeline.sh
chmod +x run_pipeline.sh

USERNAME=takinosh
./run_pipeline.sh quay.io/${USERNAME}/redhat-edge-ai-industrial-demo
```

## Run pipeline against self hosted quay on openshift
```
curl -OL https://raw.githubusercontent.com/tosin2013/redhat-edge-ai-industrial-demo-infra/main/hack/run_pipeline.sh
chmod +x run_pipeline.sh

./run_pipeline.sh $(oc get route -n quay | grep registry-quay | awk '{print $2}' | head -1)/admin/redhat-edge-ai-industrial-demo
```
./run_pipeline.sh quay.io/admin/redhat-edge-ai-industrial-demo


## Source code
**Tekton Pipelines**
* Tekton Pipeline for [redhat-edge-ai-industrial-demo](https://github.com/tosin2013/redhat-edge-ai-industrial-demo-infra/tree/main/components/applications/redhat-edge-ai-industrial-demo/overlays/rhde-dev-env)

**Via URL**  

*make sure openshift pipelines is installed before running*

```
oc apply -k https://github.com/tosin2013/redhat-edge-ai-industrial-demo-infra/components/applications/redhat-edge-ai-industrial-demo/overlays/rhde-dev-env
```