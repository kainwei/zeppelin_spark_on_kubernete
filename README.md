# Zeppelin and Spark Run on Kubernete Example

Following this example, you will create a [Apache Zeppelin](https://zeppelin.apache.org/) using functional [Apache
Spark](http://spark.apache.org/) cluster as an interpreter running on [Kubernete](https://kubernetes.io/).


### Why do this
This is a good example to learn k8s and also how spark and zeppelin work.

Second one is although [Apache Zeppelin](https://zeppelin.apache.org/) have already provided solution how to run itself on kubernetes also it can start k8s pod for spark interpreter, unfortunately the solution are not very friendly(basically it's automatic starting interpreter function is not working well, also when it try to start spark pod, you can not pull the spark image from it's repository(not exist or need authentication)), so I built this for my self.

### Sources

The Spark and Zeppelin Docker images are heavily based on:
https://github.com/kubernetes/application-images/tree/master/spark

The kubernete part from https://kubernetes.io/blog/2016/03/using-spark-and-zeppelin-to-process-big-data-on-kubernetes/ 

The yaml files from early version of https://github.com/kubernetes/kubernetes.git (kubernetes/examples/spark is not there any more, I got them from others VCS who forked these four years ago) 


## Step Zero: Prerequisites

This example assumes

- You have a Kubernetes cluster installed and running.
- That you have installed the ```kubectl``` command line tool installed in your path and configured to talk to your Kubernetes cluster
- That your Kubernetes cluster is running [kube-dns](https://github.com/kubernetes/dns) or an equivalent integration.

Optionally, your Kubernetes cluster should be configured with a Loadbalancer integration (automatically configured via kube-up or GKE)

## Step One: Build Service And Pods By Yaml Files

```sh
$ git clone https://github.com/kainwei/zeppelin_spark_on_kubernete.git
$ kubectl create -f zeppelin_spark_on_kubernete/kubernete_yaml
```
```‘zeppelin_spark_on_kubernete/kubernete_yaml’```is a directory, so this command tells kubectl to create all of the Kubernetes objects defined in all of the YAML files in that directory. You don’t have to clone the entire repository, but it makes the steps of this demo just a little easier.


The pods (especially Apache Zeppelin) are somewhat large, so may take some time for Docker to pull the images. Once everything is running, you should see something similar to the following:

```sh
$ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
spark-master-controller-mtsvl     1/1     Running   0          4s
spark-ui-proxy-controller-nxw22   1/1     Running   0          4s
spark-worker-controller-98w92     1/1     Running   0          4s
spark-worker-controller-tkxhz     1/1     Running   0          4s
zeppelin-controller-slfx6         1/1     Running   0          4s
```

You can see that Kubernetes is running one instance of Zeppelin, one Spark master and three Spark workers.

## Step Two: Set up the Secure Proxy to Zeppelin 
Next you’ll set up a secure proxy from your local machine to Zeppelin, so you can access the Zeppelin instance running in the cluster from your machine. (Note: You’ll need to change this command to the actual Zeppelin pod that was created on your cluster.)
```sh
$ kubectl port-forward zeppelin-controller-slfx6 80:8080 --address 0.0.0.0
```
This establishes a secure link to the Kubernetes cluster and pod ```(zeppelin-controller-slfx6)``` and then forwards the port in question (8080) to local port 80, which will allow you to use Zeppelin safely.




### Try Zeppelin With Spark
Now that the secure proxy is up, visit http://```<your ip>```. You should see an intro page like this:
![alt text](https://github.com/kainwei/zeppelin_spark_on_kubernete/blob/master/screenshot/zeppelin_welcom.png?raw=true)


