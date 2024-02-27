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

* Click the "Open" link for the workbench to access the Jupyter notebook.

![](https://github.com/openshift-ai-examples/openshift-ai-examples/blob/main/openshift-ai-deploy-llm/assets/open_notebook.png)

4. Clone the git repository within the notebook:
* Click the Git Clone icon:

![](https://github.com/openshift-ai-examples/openshift-ai-examples/blob/main/openshift-ai-deploy-llm/assets/git_clone_button.png)

* Copy the git repository URL:
```bash
https://github.com/openshift-ai-examples/openshift-ai-examples.git
```

* Paste and click "Clone":

![](https://github.com/openshift-ai-examples/openshift-ai-examples/blob/main/openshift-ai-deploy-llm/assets/clone.png)

5. Open get_model.ipynb and run each cell (openshift-ai-examples/openshift-ai-deploy-llm/notebooks/get_model.ipynb)

6. Open upload_model.ipynb and run each cell (openshift-ai-examples/openshift-ai-deploy-llm/notebooks/upload_model.ipynb)

7. Serve the model with Caikit TGIS:
```bash
oc apply -n rhoai-demo-llm -f manifests/4-serve-model.yaml
```