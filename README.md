# Kubernetes Kustomize HAProxy

[kustomize](https://kustomize.io/) kubernetes native configuration management tool. A template-free way to customize application configuration that simplifies the use of off-the-shelf applications

## Repository Overview

This repository contains kustomize kubernetes manifest using which [HAProxy](https://www.haproxy.com/) [daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) can be deployed on EKS or any kubernetes platform. 

## Files Overview

* ha-proxy-deployment.yaml : base deployment manifest for HAProxy. 

* haproxy.cfg : HAProxy native configuration, configmap will be dynamically generated by kustomize on the fly. 

* haproxy-service.yaml : kubernetes service manifest to expose service daemonset.

* kustomization.yaml: kustomize configuration file. Values to be overwriiten are mentioned here.

* nginx-deployment.yaml : sample nginx deployment to validate HAProxy deployment.  Traffic request to HAProxy service on port 80 will forward the request to this nginx deployment.

* nginx-service.yaml : clusterIP kubernetest service for nginx deployment. 

## Architecture

![Architecture](HAProxy.png?raw=true "Title")

After deployment HAProxy will be running on each and every worker node. HAProxy service will access traffic on three ports, 80, 5000 & 9000.

* 80 - Incoming request which will be forwarded to a backend service
* 5000 - Flask application which will fetch and return ec2-metadata when running on EKS.
* 9000 - HA proxy health & traffic stats 

# How to Deploy

Step 1: kubectl version  1.14.X or higher

`kubectl version`


Step 2: Clone this repository

`git clone https://github.com/dipinthomas/haproxy-kustomize.git `

Step 3: Deployment

`kubectl apply -k HAProxy`



