# Kubernetes Workshop

Welcome!
In this workshop we will learn the basics about kubernetes.

We will:
1. Deploy a Pod and a service from console.
2. Deploy a Pod and a service from manifests.
3. Learn about namespaces.
4. Deploy a deployment and see what are the advantages of this approach.

## Prerequisites

1. A kubernetes cluster (this can be installed locally with [Minikube](https://minikube.sigs.k8s.io/docs/))
2. kubectl cli client installed and connected to the cluster

You can check connection to the cluster by running the following command and it would show the control plane endpoint: 
~~~ bash 
kubectl cluster-info
~~~

Once connected run the command kubectl to see how many nodes do we have in our data plane  
~~~ bash 
kubectl get nodes
~~~

## Create a Pod and a Service

For now we will deploy a Pod and Service using the kubectl command line

1. Run pod using this command:

~~~ bash
kubectl run my-nginx --image nginx
~~~

This will create a pod called `my-nginx` using the docker image `nginx`. Check which Pods are running with the command

~~~ bash
kubectl get pods
~~~

2. Right now the application is running, but we can't access to it. To expose the application, create a new service running this command:

~~~ bash
kubectl expose pods my-nginx --port=80 --target-port=80 --type='NodePort' --name=my-nginx-service
~~~

3. To list all the services, run the command `kubectl get services`. You'll get an output like this:

~~~ bash 
NAME                            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes                      ClusterIP   10.96.0.1       <none>        443/TCP        28m
my-nginx-service      NodePort    10.103.65.167   <none>        80:31934/TCP   3s
~~~

You'll see the Service we just created right there. See the PORT(S) columns. Take note of the second port, in this case, `31934`.

4. Navigate to one of the nodes urls and the port. For example:

~~~ bash 
<host_ip>:31934
~~~

The application should be running in your browser.

5. Delete the resources created in the step. (Google it ;-) )

## Create a Namespace, a Pod and a Service using a YAML file

We deployed the application using the command line. Now let's deploy it using a yaml file.

1. Open the file called [1.pod.yaml](k8s_manifests/1.pod.yaml) and copy it's content.
2. Create a new yaml file inside the master node of the cluster and paste its content.
3. Complete the missing information in this file.
4. run the command `kubectl apply -f name_of_file.yaml`.
5. You should check the all the resources were created:

~~~ bash
kubectl get namespaces
kubectl get pods -n <your_namespace>
kubectl get services -n <your_namespace>
~~~

## Change the port where the application is running (Optional)

The application by default is running in the port 80 inside the pod. This can be changed updating the environment variable called `PORT`.

1. Update the Pod manifest to add this environment variable to the Pod.
2. Update the Service to reflect this change.

