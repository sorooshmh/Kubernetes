## Ingress Nginx


First we should clone repository and config nginx ingress and after that we can create deployment and expose them to external environment. In addition we deploy a resource to connect to deployments.

    git clone https://github.com/nginxinc/kubernetes-ingress/
    cd kubernetes-ingress/deployments


## Configure RBAC


    kubectl apply -f common/ns-and-sa.yaml
    kubectl apply -f rbac/rbac.yaml
    kubectl apply -f rbac/ap-rbac.yaml



## Create Common Resources


    kubectl apply -f common/default-server-secret.yaml
    kubectl apply -f common/nginx-config.yaml
    kubectl apply -f common/ingress-class.yaml




    kubectl apply -f common/crds/k8s.nginx.org_virtualservers.yaml
    kubectl apply -f common/crds/k8s.nginx.org_virtualserverroutes.yaml
    kubectl apply -f common/crds/k8s.nginx.org_transportservers.yaml
    kubectl apply -f common/crds/k8s.nginx.org_policies.yaml
    kubectl apply -f common/crds/k8s.nginx.org_globalconfigurations.yaml


## Deploy the Ingress Controller


    kubectl apply -f daemon-set/nginx-ingress.yaml



**Now we can deploy our pods:**


    cd /root/ingress
    kubectl apply -f nginx-deploy-green.yaml
    kubectl apply -f nginx-deploy-blue.yaml
    


    kubectl expose deploy nginx-deploy-main --port 80

    kubectl expose deploy nginx-deploy-blue --port 80

    kubectl expose deploy nginx-deploy-green --port 80

    kubectl apply -f ingress-resource-2.yaml


    kubectl describe ing ingress-resource-2





**Set Frontend and backend in HAPROXY**



    frontend http_front
    bind *:80
    stats uri /haproxy?stats
    default_backend http_back


    backend http_back
    balance roundrobin
    server worker-1 192.168.50.13:80
    server worker-2 192.168.50.14:80
    server worker-3 192.168.50.15:80





vim /etc/hosts
192.168.50.9 nginx.example.com
192.168.50.9 blue.ginx.example.com
192.168.50.9 green.ginx.example.com

Note:192.168.50.9 is our haproxy ip


