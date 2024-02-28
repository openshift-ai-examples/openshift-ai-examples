# openshift-ai-deploy-llm

This is an example of how to quickly deploy a small llm ([google/flan-t5-small](https://huggingface.co/google/flan-t5-small)) on a Red Hat OpenShift AI data science cluster.

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

5. Download and convert the model to Caikit format:

* Open get_model.ipynb and run each cell (openshift-ai-examples/openshift-ai-deploy-llm/notebooks/get_model.ipynb)

6. Upload the converted model to the MinIO S3 bucket:

* Open upload_model.ipynb and run each cell (openshift-ai-examples/openshift-ai-deploy-llm/notebooks/upload_model.ipynb)

7. Serve the model with Caikit TGIS (This will take a little while):
```bash
oc apply -n rhoai-demo-llm -f manifests/4-serve-model.yaml
```

8. Test the llm
```bash
curl -kL -H 'Content-Type: application/json' -d '{"model_id": "flan-t5-small-caikit", "inputs": "Is this working?"}' https://$(oc get route flan-t5-rhoai-demo-llm -n istio-system -o jsonpath='{.spec.host}')/api/v1/task/text-generation
```

Output
```
{"generated_text": "yes", "generated_tokens": 2, "finish_reason": "EOS_TOKEN", "producer_id": {"name": "Text Generation", "version": "0.1.0"}, "input_token_count": 6, "seed": null}% 
``` 
