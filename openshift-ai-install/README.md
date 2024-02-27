# openshift-ai-install

This is an example of how to quickly install a Red Hat OpenShift AI data science cluster.

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
```

```bash
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

6. Log in to OpenShift AI

Refer to the existing Red Hat OpenShift AI documentation for instructions:
- [Logging in to OpenShift AI](https://access.redhat.com/documentation/en-us/red_hat_openshift_ai_self-managed/2.6/html/getting_started_with_red_hat_openshift_ai_self-managed/logging-in_get-started)