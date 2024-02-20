# openshift-ai-deploy

This is an example of how to quickly deploy a Red Hat OpenShift AI data science cluster, a data science project, and Minio s3 storage.

Procedure:

1. Create namespaces:
```bash
oc apply -f manifests/1-namespaces.yaml
```

2. Create OperatorGroup objects for required operators:
```bash
oc apply -f manifests/2-operatorgroups.yaml
```

3. Create Subscription objects for required operators:
```bash
oc apply -f manifests/3-subscriptions.yaml
```

4. Create the DataScienceCluster object:
```bash
oc apply -f manifests/5-datasciencecluster.yaml
```

5. Create the data science project:
```bash
oc apply -f manifests/5-datascienceproject.yaml
```

6. Create the Minio s3 storage and data connections
```bash
oc apply -n rhoai-demo -f https://github.com/rh-aiservices-bu/fraud-detection/raw/main/setup/setup-s3.yaml
```