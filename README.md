# Tutorial 11 Adpro: Deployment on Kubernetes

Name: Carleano Ravelza Wongso

NPM: 2306213022

## Reflection on Hello Minikube

>  Compare the application logs before and after you exposed it as a Service.

![image](https://github.com/user-attachments/assets/a6ea6101-3de7-44ee-81fd-e670bc1d3bf1)

Before exposing the Deployment as a Service, the application logs display the server startup messages but no incoming request logs. After the Deployment is exposed as a service, we can actually hit the service and see the request log.

> What is the purpose of the `-n` option and why did the output not list the pods/services that you explicitly created?

The `-n` (or `--namespace`) flag in kubectl is used to list or manage resources within a specific Kubernetes namespace. If no namespace is specified, it defaults to the default namespace, where resources like our hellonode Deployment and Service are located. So, when we run `kubectl get pods,services -n kube-system`, it queries the `kube-system` namespace instead, meaning it won’t display the hellonode resources, since they exist in the `default` namespace, not `kube-system`.
