# ledger-k8s

A project which deploy ledger application to k8s(Azure AKS, Aliyun, etc.). Ledger application has 3 tier:
frontend, backend and db. So the k8s deployment infrastructure is as below:
    
                        load balancer
                            |
                        ingress service
                            |
                        web service
                            |
                        web pods
                            |
                        app service
                            |
                        app pods
                            |
                        db service
                            |
                        db pods   

### Steps to run:

1. Deploy web container;
    kubectl apply -f web-deployment.yaml

2. Deploy service which access web container;
    kubectl apply -f web-service.yaml

3. Deploy db container(should before deploy api container, because api container connect to db when init);
    kubectl apply -f db-deployment.yaml

4. Deploy db service which access db container;
    kubectl apply -f db-service.yaml

5. Deploy api container;
    kubectl apply -f api-deployment.yaml

6. Deploy service which access api container;
    kubectl apply -f api-service.yaml

7. Deploy nginx-ingress-controller(https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/mandatory.yaml)
    kubectl apply -f mandatory.yaml

   *Note: api version of kind=deployment in official nginx-ingress-controller should be changed to "apps/v1"*
   *if k8s version > 1.9* 

   ```
    apiVersion: apps/v1
    kind: Deployment
   ```

8. Deploy Ingress service;
    kubectl apply -f ledger-ingress.yaml

9. Deploy Load Balance service:
    kubectl apply -f ingress-nginx.yaml