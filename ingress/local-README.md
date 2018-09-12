# Helm is a dependency manager for kubernetes
# There are two parts to Helm:
# The Helm client (helm) and the Helm server (Tiller).

# install helm
brew install kubernetes-helm
# install tiller
helm init

# install nginx-ingress controller
helm install stable/nginx-ingress --name my-nginx
kubectl apply -f mandatory.yaml
# kubernetes config for docker-for-mac
kubectl apply -f cloud-generic.yaml

# this should response with "default backend - 404"
curl localhost

# setup ssl certificate
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ./ssl.key -out ./ssl.crt
openssl dhparam -out ./ssl.pem 2048
kubectl create secret tls tls-certificate --key ssl.key --cert ssl.crt
kubectl create secret generic tls-dhparam --from-file=ssl.pem

# OPTIONAL dashboard
kubectl create -f dashboard.yaml
kubectl proxy  
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
