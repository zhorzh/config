#############################
# install HELM
#############################

kubectl create serviceaccount -n kube-system tiller
# serviceaccount "tiller" created

kubectl create clusterrolebinding tiller-binding \
    --clusterrole=cluster-admin \
    --serviceaccount kube-system:tiller
# clusterrolebinding "tiller-binding" created

helm init --service-account tiller
# HELM_HOME has been configured at /Users/zhorzh/.helm.

helm repo update
# Update Complete. ⎈ Happy Helming!⎈

#############################
# install cert-manager
#############################

helm install --name cert-manager --version v0.3.2 \
    --namespace kube-system stable/cert-manager
# cert-manager has been deployed successfully!

#############################
# Set up Let‘s Encrypt
#############################

export EMAIL=zhorzh.alexandr@gmail.com

curl -sSL https://rawgit.com/ahmetb/gke-letsencrypt/master/yaml/letsencrypt-issuer.yaml | \
    sed -e "s/email: ''/email: $EMAIL/g" | \
    kubectl apply -f-
# clusterissuer "letsencrypt-staging" created
# clusterissuer "letsencrypt-prod" created

#############################
# deploy gke ingress
#############################

#############################
# get certificate
#############################

kubectl apply -f certificate.yaml
# certificate "api-tls" created

kubectl get secrets
# wait until api-tls secret appear here

kubectl describe -f certificate.yaml
# check status of the certificate
