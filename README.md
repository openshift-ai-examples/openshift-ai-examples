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

4. Wait for all operators to successfully install
```bash
oc get csv -w -n default

NAME                            DISPLAY                                          VERSION    REPLACES                             PHASE
elasticsearch-operator.v5.8.3   OpenShift Elasticsearch Operator                 5.8.3      elasticsearch-operator.v5.8.2        Succeeded
jaeger-operator.v1.51.0-1       Red Hat OpenShift distributed tracing platform   1.51.0-1   jaeger-operator.v1.47.1-5            Succeeded
kiali-operator.v1.65.11         Kiali Operator                                   1.65.11    kiali-operator.v1.65.10              Succeeded
rhods-operator.2.6.0            Red Hat OpenShift AI                             2.6.0      rhods-operator.2.5.0                 Succeeded
serverless-operator.v1.31.1     Red Hat OpenShift Serverless                     1.31.1     serverless-operator.v1.31.0          Succeeded
servicemeshoperator.v2.4.5      Red Hat OpenShift Service Mesh                   2.4.5-0    servicemeshoperator.v2.4.4           Succeeded
```

5. Create the DataScienceCluster object:
```bash
oc apply -f manifests/4-datasciencecluster.yaml
```

6. Create the data science project:
```bash
oc apply -f manifests/5-datascienceproject.yaml
```

7. Create the Minio s3 storage and data connections
```bash
oc apply -n rhoai-demo -f https://github.com/rh-aiservices-bu/fraud-detection/raw/main/setup/setup-s3.yaml
```
