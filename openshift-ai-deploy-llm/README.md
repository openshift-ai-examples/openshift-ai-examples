# openshift-ai-deploy-llm

This is an example of how to quickly deploy a small llm on a Red Hat OpenShift AI data science cluster.

Procedure:

1. Create the data science project:
```bash
oc apply -f manifests/1-datascienceproject.yaml
```

2. Create the MinIO s3 storage and data connections
```bash
oc apply -n rhoai-demo-llm -f manifests/2-setup-s3.yaml
```

3. Create the workbench (notebook)
```bash
oc process -f manifests/3-notebook-template.yaml -n rhoai-demo-llm \
-p INGRESS_DOMAIN=$(oc get ingresscontroller default -n openshift-ingress-operator -o jsonpath='{.status.domain}') \
-p NOTEBOOK_IMAGE=$(oc get is s2i-minimal-notebook -n redhat-ods-applications -o jsonpath='{.status.dockerImageRepository}{":"}{.spec.tags[-1].name}') \
| oc create -f -
```
3. Access the workbench from the data science project:

Click the "Open" link for the workbench to access the Jupyter notebook.