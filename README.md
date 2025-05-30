# Tutorial 11 Adpro: Deployment on Kubernetes

Name: Carleano Ravelza Wongso

NPM: 2306213022

## Reflection on Hello Minikube

>  Compare the application logs before and after you exposed it as a Service.

![image](https://github.com/user-attachments/assets/a6ea6101-3de7-44ee-81fd-e670bc1d3bf1)

Before exposing the Deployment as a Service, the application logs display the server startup messages but no incoming request logs. After the Deployment is exposed as a service, we can actually hit the service and see the request log.

> What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

The `-n` (or `--namespace`) flag in kubectl is used to list or manage resources within a specific Kubernetes namespace. If no namespace is specified, it defaults to the default namespace, where resources like our hellonode Deployment and Service are located. So, when we run `kubectl get pods,services -n kube-system`, it queries the `kube-system` namespace instead, meaning it won’t display the hellonode resources, since they exist in the `default` namespace, not `kube-system`.

## Reflection on Rolling Update & Kubernetes Manifest Files

> Difference between Rolling Update and Recreate deployment strategy

Rolling Update is the default deployment strategy in Kubernetes, designed to ensure zero downtime by gradually replacing old pods with new ones. This process maintains application availability by controlling how many pods are taken down or added at a time using parameters like maxUnavailable and maxSurge. It suits most production workloads where continuous service availability is essential.

In contrast, the Recreate strategy stops all existing pods before deploying new ones. While this leads to a temporary service outage, it guarantees version consistency across all pods. This method is useful for applications that can't tolerate running multiple versions concurrently, such as those with tightly coupled state or schema dependencies.

> Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.

Here's the steps that I took:

1. Create `spring-petclinic-rest` deployment and optionally expose it as a service

```shell
kubectl create deployment spring-petclinic-rest --image=docker.io/springcommunity/spring-petclinic-rest:3.0.2
kubectl expose deployment spring-petclinic-rest --type="LoadBalancer" --port 9966
```

2. Change the deployment strategy

We can use the shell command:

```shell
kubectl patch deployment spring-petclinic-rest \
  --type='json' \
  -p='[
    {"op": "remove", "path": "/spec/strategy/rollingUpdate"},
    {"op": "replace", "path": "/spec/strategy/type", "value": "Recreate"}
  ]'
```

or we can just use: `kubectl edit deployment` and manually update `spec.strategy.type`.

> Benefits of using Kubernetes manifest files

Using Kubernetes manifest files brings structure, consistency, and automation to application deployment. Unlike manual deployments that rely on running multiple imperative commands, manifest files define the desired state of resources declaratively. Kubernetes continuously reconciles this state, reducing drift and human error. By storing manifests in version control, changes become auditable and reproducible, enabling rollbacks and team collaboration through Infrastructure as Code principles.

Manifests also simplify automation and CI/CD integration, allowing deployments to be tested, reviewed, and promoted across environments with minimal effort. In practice, deploying with kubectl apply -f proved significantly more reliable and efficient compared to manually crafting each resource. It transforms deployment from a manual, error-prone task into a predictable, traceable workflow.
