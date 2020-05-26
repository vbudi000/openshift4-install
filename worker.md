# Worker node

Once you try to work on the worker nodes, typically most of the infrastructure and connectivity problems are ironed out with the bootstrap and master initialization. Hence we assume that at this stage the worker nodes can be booted and the console shown the login page. Otherwise, follow the steps for master not initializing.

Worker nodes are needed to initialize the authentication cluster operator and console cluster operator.

## Pending CSR

 
## Cluster operator

Lets look at more detail on clusteroperator:

- You can get more detail on cluster operator using the command: `oc describe co <operator name>` <br>![desc co](images/04-descco)

- Find pod name from the operator namespace that is shown in the describe output above <br>![get operator pod](images/04-operpod)

- Look at the operator pod logs to see what is still needed for its completion `oc logs <podname> -n <nsname>` <br>![oper log](images/04-operlog.png)

## Ingress controller

Ingress controller structure is shown in the following diagram:

- Getting the ingress controller information

- Getting router information

- Getting DNS integration information

## Image registry

In OpenShift, Image Registry is an integral component for running applications. It is a stateful operator that must be backed up with a Physical storage. The storage is allocated by the image registry operator. If the initial size and storage class is not ideal, you should modify it as soon as possible to avoid data loss for stored application images.
