# Master node troubleshooting

Now that you are here - you check the console of the master nodes - similar to the bootstrap node console.

## Master node initialization

Typically if you have your bootstrap on the waiting for ETCD server - your master node has been initialized. If you found that the master is not initialized and you see messages like <br>![master ign](images/03-masterign.png)
It means that it is waiting for the bootstrap to initialize the machine config server. If the bootstrap is checked using the command `curl -k https://<bootstrap>:22623/config`   <br>![machine config](images/02-machineconfig.png) <br> But the load balancer `curl -k https://api-int.<cluster>.<base-domain>:22623/config` is failed then you should check your master load balancer for port 22623.

## Master is Running

Once the master nodes are running and the bootstrap node has been removed from the master load balancer, most of the checking is performed using `oc` command.

```
export KUBECONFIG=auth/kubeconfig
oc get node
```

This should show the unhealthy master nodes initially and then they become Ready. <br>![get nodes](images/03-getnodes.png)

After the master nodes are Ready, check the cluster operators. Run `oc get clusteroperators`. You can also run `watch -n5 oc get clusteroperators` that will have the display refreshed every 5 seconds. <br>![getco](images/03-getco.png)

Eventually the only ones that are not Ready should be authentication and console. Those 2 clusteroperators requires worker nodes to be running before they becomes Available.

## Problems with masters

The following are some of the potential problems that can arises when installing masters:

- Masters does not have enough computing power (ie CPU, memory, storage) - you can get the bootstrap manifest insertion failed such as: <br> ![manifest failed](images/03-manifest-failed.jpg)

- Masters passed bootstrap but does not have enough computing power - multiple cluster operators fail. <br> ![co not there](images/03-minico.png)

- Workers are not provisioned from the cluster (Public cloud platform) = check MachineSet and Machine operator - see [worker trouble shooting](worker.md).

## Problems with pods running in Master node

You can login to the master node as the `core` user and check the running pods and containers there using the `crictl` command.

- The following example is the initial etcd pod that is created when the master node is starting. <br>![CRICTL](images/03-crictl.png)

    You can list the pods, and then the containers, each pod can have one or more containers. Log can be retrieved by using the container id (or part of it). In this way, you can find a cause for an incomplete start of a master node.

- Further down the intialization, when the bootkube is almost done, more pods appear in the master node <br>![more pods](images/03-crictl-run.png)
