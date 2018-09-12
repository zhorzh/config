# set kubectl context
gcloud container clusters get-credentials neural-pub

# check kubectl context
kubectl config get-contexts

# create cluster
gcloud container clusters create neural-pub \
  --zone europe-west1-d \
  --machine-type=n1-standard-1 \
  --enable-autorepair \
  --num-nodes=1

# setup ssl certificate
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ./ssl.key -out ./ssl.crt
openssl dhparam -out ./ssl.pem 2048
kubectl create secret tls tls-certificate --key ssl.key --cert ssl.crt
kubectl create secret generic tls-dhparam --from-file=ssl.pem
