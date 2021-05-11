---
title: k8s
toc: true
category: computer
lang: zh-cn
date: 2021-05-11 13:11:30
tags:
---

Kubernetes 的使用(我使用k0s)

<!-- more -->

To view the nodes in the cluster:
```console
$ kubectl get nodes
```

Deploy our first app(include the full repository url for images hosted outside Docker hub):
```console
$ kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```

To list your deployments:
```console
$ kubectl get deployments
```

To show the application deployed in the nodes
```console
$ kubectl get pods
```

Next, to view what containers are inside that Pod and what images are used to build those containers we run the `describe pods` command:
```console
$ kubectl describe pods
```

Pods that are running inside Kubernetes are running on a private, isolated network.
By default they are visible from other pods and services within the same kubernetes cluster.
The `kubectl` command can create a proxy that will forward communication into the cluster-wide:
```console
$ kubectl proxy
```
You can see all those APIs hosted through the proxy endpoint.
For example, we can query the version directly through the API using the `curl` command:
```console
curl http://localhost:8001/version
```
First we need to get the Pod name, and we'll store in the environment variable POD_NAME:
```console
$ export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
$ echo Name of the Pod: $POD_NAME
```

To see the output of our application, run a `curl` request:
```console
$ curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
```

Anything that the application would normally send to `STDOUT` becomes logs for the container within the Pod. We can retrieve these logs using the `kubectl logs` command:
```console
$ kubectl logs $POD_NAME
```
The name of the container itself can be omitted since we only have a single container in this Pod.

We can execute commands directly on the container once the Pod is up and running.
```console
$ kubectl exec $POD_NAME -- env
```
Again, worth mentioning that the name of the container itself can be omitted since we only have a single container in this Pod.

Next let's start a bash session in the Pod's container:
```console
$ kubectl exec -ti $POD_NAME -- bash
```
We have now an open console on the container where we run our NodeJS application. The source code of the app is in the server.js file:
```console
root@kubernetes-bootcamp-xxxxxxxxx-xxxxx:/# cat server.js
```

To create a new service and expose it to external traffic we'll use the expose command with NodePort as parameter:
```console
$ kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
```
> Services can be exposed in different ways by specifying a `type` in the ServiceSpec:
> * *ClusterIP* (default) - Exposes the Service on an internal IP in the cluster. This type makes the Service only reachable from within the cluster.
> * *NodePort* - Exposes the Service on the same port of each selected Node in the cluster using NAT. Makes a Service accessible from outside the cluster using `<NodeIP>:<NodePort>`. Superset of ClusterIP.
> * *LoadBalancer* - Creates an external load balancer in the current cloud (if supported) and assigns a fixed, external IP to the Service. Superset of NodePort.
> * *ExternalName* - Maps the Service to the contents of the `externalName` field (e.g. `foo.bar.example.com`), by returning a `CNAME` record with its value. No proxying of any kind is set up. This type requires v1.7 or higher of `kube-dns`, or CoreDNS version 0.0.8 or higher.

let's list the current Services from our cluster:
```console
$ kubectl get services
```

To find out what port was opened externally (by the NodePort option)
we'll run the `describe service` command:
```console
$ kubectl describe services/kubernetes-bootcamp
```

Create an environment variable called NODE_PORT that has the value of the Node port assigned:
```console
export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
```

Now we can test that the app is exposed outside of the cluster using `curl`:
```console
$ curl 127.0.0.1:$NODE_PORT
```
2. Using labels
   With `describe deployment` command you can see the name of the label.
   Let's use this label to query out list of Pods or Services:
   ```console
   $ kubectl get pods -l app=kubernetes-bootcamp
   $ kubectl get services -l app=kubernetes-bootcamp
   ```

   Get the name of the Pod and store it in the POD_NAME environment variable:
   ```console
   $ export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
   $ echo Name of the Pod: $POD_NAME
   ```

   To apply a new label we use the label command followed by the object type. object name and the new label:
   ```console
   $ kubectl label pod $POD_NAME version=v1
   ```
   We pinned the application version to the Pod, and we can check it with the describe pod command:
   ```console
   $ kubectl describe pods $POD_NAME
   ```
   We see here that the label is attached now to our Pod.
   And we can query now the list of pods using the new label:
   ```console
   $ kubectl get pods -l version=v1
   ```
3. Deleting a service
   To delete Services you can use the `delete service` command. Lables can be used also here:
   ```console
   $ kubectl delete service -l app=kubernetes-bootcamp
   ```
   Tips: You can confirm that the app is still running with a curl inside the pod:
   ```console
   $ kubectl exec -ti $POD_NAME -- curl localhost:8080
   ```

## Scale up your app

1. Scaling a deployment
   To list your deployments:
   ```console
   $ kubectl get deployments
   ```
   The output should be similar to:
   ```
   NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
   kubernetes-bootcamp   1/1     1            1           11m
   ```
   This shows:
   * *NAME* lists the names of the Deployments in the cluster.
   * *READY* shows the ratio of CURRENT/DESIRED replicas
   * *UP-TO-DATE* displays the number of replicas that have been updated to achieve the desired state.
   * *AVAILABLE* displays how many replicas of the application are available to your users.
   * *AGE* displays the amount of time that the application has been running.
   
   To see the ReplicaSet created by the Deployment, run
   ```console
   kubectl get rs
   ```
   Two important columns of this command are:
   * *DESIRED* displays the desired number of replicas of the application, which you define when you create the Deployment. This is the desired state.
   * *CURRENT* displays how many replicas are currently running.

   Next, let's scale the Deployment to 4 replicas:
   ```console
   $ kubectl scale deployments/kubernetes-bootcamp --replicas=4
   ```
   To list your Deployments once again:
   ```
   $ kubectl get deployments
   ```
2. Load Balancing
   Let's check that the Service is load-balancing the traffic. To find out the exposed IP and Port we can use the descrie service:
   ```console
   $ kubectl describe services/kubernetes-bootcamp
   ```

   Create an environment variable called NODE_PORT that has a value as the Node port:
   ```console
   $ export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
   ```

   Next, we’ll do a curl to the exposed IP and port. Execute the command multiple times:
   ```console
   $ curl 127.0.0.1:$NODE_PORT
   ```
   We hit a different Pod with every request. This demonstrates that the load-balancing is working.
3. Scale Down
   To scale down the Service to 2 replicas, run again the `scale` command:
   ```console
   $ kubectl scale deployments/kubernetes-bootcamp --replicas=2
   ```

## Update your app

1. Update the version of the app
   To update the image(for example version 2):
   ```
   $ kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
   ```
2. Verify an update
   To view the current iamge version of the app, run a describe command against the Pods:
   ```console
   $ kubectl describe pods
   ```
   The update can be confirmed also by running a rollout status command:
   ```console
   $ kubectl rollout status deployments/kubernetes-bootcamp
   ```
3. Rollback an update
   Let's perform another update, and deploy image tagged as `v10`
   ```console
   $ kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=gcr.io/google-samples/kubernetes-bootcamp:v10
   ```
   Use `get deployments` to see the status of the deployment:
   ```console
   $ kubectl get deployments
   ```
   And something is wrong... We do not have the desired number of Pods available. List the Pods again:
   ```console
   $ kubectl get pods
   ```
   A `describe` command on the Pods should give more insights:
   ```console
   $ kubectl describe pods
   ```
   There is no image called `v10` in the repository. Let's roll back to our previously working version. We'll use the `rollout` undo command:
   ```console
   $ kubectl rollout undo deployments/kubernetes-bootcamp
   ```
   List again the Pods:
   ```console
   $ kubectl get pods
   $ kubectl describe pods
   ```
   We see that the deployment is using a stable version of the app (v2). The Rollback was successful.

The most common operations can be done with the following kubectl commans:
* **kubectl get** - list resources
  **kubectl get pods -o wide** to get more informations
* **kubectl describe** - show detailed information about a resource
* **kubectl logs** - print the logs from a container in a pod
* **kubectl exec** - execute a command on a container in a pod