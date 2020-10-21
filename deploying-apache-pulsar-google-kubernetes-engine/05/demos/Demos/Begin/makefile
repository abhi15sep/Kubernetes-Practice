PROJECT_ID = pluralsight-pulsar-gke
CLUSTER_NAME = pulsar-cluster-1

# gcloud version -> 306.0.0
# kubectl version -> 1.18.8
# helm version -> 3.3.0

# 1
gcloud-auth:
	gcloud auth login

# 2
gcloud-config:
	gcloud config set project $(PROJECT_ID)

# 3
create-cluster:
	gcloud beta container clusters create $(CLUSTER_NAME) --cluster-version=1.16.13-gke.1 \
	--identity-provider=$(PROJECT_ID).svc.id.goog \
	--tags=pulsar-server \
	--scopes=gke-default \
	--num-nodes=3 \
	--no-enable-autoupgrade \
	--machine-type=n1-standard-2 \
	--enable-ip-alias

# 4
set-default-cluster:
	gcloud config set compute/zone us-central1-b
	gcloud config set container/cluster $(CLUSTER_NAME)
	gcloud container clusters get-credentials $(CLUSTER_NAME)

# 5
helm-add-charts:
	helm repo add jetstack https://charts.jetstack.io
	helm repo add apache https://pulsar.apache.org/charts
	helm repo update

# 6
cert-manager-install:
	kubectl create namespace cert-manager
	helm install \
  	cert-manager jetstack/cert-manager \
  	--namespace cert-manager \
  	--version v0.16.1 \
  	--set installCRDs=true

# 7
pulsar-install:
	kubectl create namespace pulsar
	helm install pulsar apache/pulsar --timeout 10m

# 8
pulsar-upgrade:
	helm upgrade --install pulsar apache/pulsar --timeout 10m -f values.yaml

# ----- EXPLORING PULSAR -----

# Checkout all of the namespaces on the cluster
# kubectl get namespaces(ns)

# Get all of the pods in the pulsar namespace
# kubectl get pods(po) -n pulsar

# Get all of the services in the pulsar namespace
# kubectl get services(svc) -n pulsar

# Get all of the configmaps in the pulsar namespace
# kubectl get configmaps(cm) -n pulsar

# Get all of the secrets in the pulsar namespace
# kubectl get secrets -n pulsar

# Get the TLS certificates in the pulsar namespace
# kubectl get certificates -n pulsar