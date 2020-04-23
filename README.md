# opa-bundle-demo
OPA (OPA-Istio) that allows you to enforce OPA policies at the Istio Proxy layer. This is a example of how to inject policy from external sources like s3 so that we will able to change policy at dynamic time.

Table of contents

#####Getting_started
#####Configuring
#####Running
#####Example
###Getting Started
Requirements - To run this example you will need minikube, kubectl, Docker, istio.

###Configuring
To get case class objects from Kafka topic will need to make deserilaizer

###Running
Steps to run this templete-

1> Start minikube

minikube start --vm-driver=none

2> Install istio on minikube cluster

istioctl manifest apply --set profile=demo

3> Ensure all pods are running

kubectl get pods -n istio-system

4> Now apply quick_start.yaml

kubectl apply -f quick_start.yaml

5> Enable the opa and istio injection for service pods

**kubectl label namespace default opa-istio-injection="enabled"

kubectl label namespace default istio-injection="enabled"**

6> Now apply bookinfo.yaml

kubectl apply -f bookinfo.yaml

7> Create a gateway for your service mesh by applying bookinfo-gateway.yaml

kubectl apply -f bookinfo-gateway.yaml

8> Node Port

export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')

export INGRESS_HOST=$(minikube ip)

**export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT **

echo $GATEWAY_URL

Now check the authorization by following curl commands

###Example
curl --user alice:password -i http://$GATEWAY_URL/productpage

curl --user alice:password -i http://$GATEWAY_URL/api/v1/products

curl --user bob:password -i http://$GATEWAY_URL/productpage

curl --user bob:password -i http://$GATEWAY_URL/api/v1/products

All this examples will run when you upload same policy from https://github.com/LokeshAggarwal1997/opa-demo/blob/master/quickstart.yaml#L237 here into s3
