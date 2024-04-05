Kubernetes Deployment with Nginx Load Balancer and Canary Deployment
====================================================================

This README provides step-by-step instructions on how to deploy a Kubernetes cluster with an Nginx load balancer and perform a canary deployment using Minikube.

Prerequisites
-------------

*   Minikube installed on your local machine
    
*   kubectl command-line tool installed
    
*   Docker installed (if using Minikube with Docker driver)
    

Steps
-----

1.  minikube start
    
2.  minikube addons enable ingress
    
3.  kubectl apply -f app-1-dep.yaml 
   
4.  kubectl apply -f app-1-svc.yaml
    
5.  kubectl apply -f app-2-dep.yaml 
   
6.  kubectl apply -f app-2-svc.yaml
    
7.  kubectl apply -f nginx-configmap.yaml
    
8.  kubectl apply -f nginx-dep.yaml
    
9.  kubectl apply -f nginx-svc.yaml
    
10. kubectl apply -f app-1-ingress.yaml
    
11. kubectl apply -f app-2-ingress.yaml
    
### Check if deployments, services and ingress were correctly configured

12. kubectl get deployments
    
13. kubectl get services
    
14. kubectl get ingress

### Test Application

15.  curl http://$(minikube ip)/

**You should see responses:**

$ curl http://$(minikube ip)/
Hello World from [app-1-dep-86f67f4f87-2d28z]!
$ curl http://$(minikube ip)/
Hello World from [app-2-dep-7f686c4d8d-lr95c]!

from both app-1 and app-2 services based on the specified weights in the Ingress resources.
    

Canary Deployment
-----------------

The Ingress resources (app-1-ingress.yaml and app-2-ingress.yaml) are configured to perform a canary deployment. The traffic is split between app-1 and app-2 services based on the specified weights.

*   app-1 is the stable version and receives 70% of the traffic.
    
*   app-2 is the canary version and receives 30% of the traffic.
    

Weights in the Ingress resources can be adjusted to control the traffic distribution between the stable and canary versions.

Cleanup
-------

To clean up the resources created in this deployment, run the following commands:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   kubectl delete -f app-1-ingress.yaml  kubectl delete -f app-2-ingress.yaml  kubectl delete -f nginx-svc.yaml  kubectl delete -f nginx-dep.yaml  kubectl delete -f nginx-configmap.yaml  kubectl delete -f app-2-svc.yaml  kubectl delete -f app-2-dep.yaml  kubectl delete -f app-1-svc.yaml  kubectl delete -f app-1-dep.yaml   `

This will delete all the created resources, including deployments, services, configmaps, and ingresses.

Finally, you can stop Minikube:

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   minikube stop   `