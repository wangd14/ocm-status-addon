# Status-AddOn

Agent and Controller built with the Open Cluster Management Add-On Framework
to provide deep status for selected resources.

# TL;DR

TODO


## Prereqs

- go version 1.20 and up
- make
- kubectl
- docker (or compatible docker engine that works with kind)
- kind
- helm

## Setup

1. Clone this repo.

2. Setup KSLight as described in [setup](https://github.ibm.com/dettori/kslight#setup), then install
   status add-on with helm:
    ```shell
    helm --kube-context imbs1 upgrade --install status-addon -n open-cluster-management oci://quay.io/pdettori/status-addon-chart --version 0.1.0
    ```
3. Check status of agent deployments
    ```shell
    kubectl --context imbs1 -n cluster1 get managedclusteraddons addon-status
    kubectl --context imbs1 -n cluster2 get managedclusteraddons addon-status
    ```
    After agents start and are running, `AVAILABLE`` should be `True` on both namespaces

## Using the Add-On

1. Deploy a workload with KSLight, for example following [Scenario 1](https://github.ibm.com/dettori/kslight#scenario-1---multi-cluster-workload-deployment-with-kubectl)

2. Verify that `WorkStatus` objects are created in `imbs1` managed clusters namespaces 
    ```shell
    kubectl --context imbs1 get workstatuses -n cluster1 
    kubectl --context imbs1 get workstatuses -n cluster2
    ```

3. Inspect one of the statuses (change `WS_NAME` based on your scenario). You should be able to see a full status.
    ```shell
    WS_NAME=appsv1-deployment-nginx-nginx-deployment
    kubectl --context imbs1 get workstatuses -n cluster1 ${WS_NAME} -o yaml
    ```

## Uninstalling the add-on

To uninstall the status add-on, use the following helm command:

```shell
helm --kube-context imbs1 -n open-cluster-management delete status-addon
```