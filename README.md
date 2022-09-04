# Lab EFK on Kind

Logging management lab with Kind, Elasticsearch, Fluent Bit and Kibana.

## Requirements

- [Docker](https://docs.docker.com/get-docker/)
- [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Intro

[Docker](https://docs.docker.com/get-docker/) helps developers bring their ideas to life by conquering the complexity of app development. We simplify and accelerate development workflows with an integrated dev pipeline and through the consolidation of application components. Actively used by millions of developers around the world, Docker Desktop and Docker Hub provide unmatched simplicity, agility and choice.

[Kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation) is a tool for running local Kubernetes clusters using Docker container “nodes”.
kind was primarily designed for testing Kubernetes itself, but may be used for local development or CI.

The Kubernetes command-line tool, [kubectl](https://kubernetes.io/docs/tasks/tools/), allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs.

Running a Kubernetes-based infrastructure is challenging and complex. Administrators often lament how complicated performance optimization and monitoring are, which can lead to problems in production.

When Kubernetes starts behaving strangely, searching the logs can help you uncover breadcrumbs. These contextual tips can help you find possible solutions. In this lab we will explore the EFK Stack (Elasticsearch, Fluent Bit and Kibana).

The EFK Stack is a collection of three open source products — Elasticsearch, Fluent Bit, and Kibana. The EFK stack provides centralized logging to identify application logs. It also allows you to search all logs in one place.

## Logging Architecture Lab

![Logging Architecture](/images/arch.png "Logging Architecture")


## Usage

Create cluster
```bash
kind create cluster --config kind.yaml
```
![kind create cluster](/images/create-cluster.png "kind create cluster")

Deploy [random logger](https://github.com/chentex/random-logger)
```bash
kubectl create deployment random-logger --image=chentex/random-logger --replicas=3
```
![deploy random logger](/images/deploy-random-logger.png "deploy random logger")

Create cluster
```bash
kubectl create namespace logging
```

Deploy Elasticsearch
```bash
kubectl apply -f manifests/elasticsearch.yaml
```
![deploy elasticsearch](/images/deploy-elasticsearch.png "deploy elasticsearch")

Deploy Kibana
```bash
kubectl apply -f manifests/kibana.yaml
```
![deploy kibana](/images/deploy-kibana.png "deploy kibana")

Deploy Fluent Bit
```bash
kubectl apply -f manifests/fluentbit.yaml
```
![deploy fluentbit](/images/deploy-fluentbit.png "deploy fluentbit")

Port forward Elasticsearch
```bash
kubectl -n logging port-forward service/elasticsearch 9200:9200
```
![curl elasticsearch](/images/curl-elasticsearch.png "curl elasticsearch")

Port forward  Kibana
```bash
kubectl -n logging port-forward service/kibana 5601:5601
```

go to http://localhost:5601

![kibana](/images/kibana.png "kibana")

![discover](/images/kibana-discover.png "discover")

![discover](/images/kibana-discover2.png "discover")

![discover](/images/kibana-discover3.png "discover")

![discover](/images/kibana-discover4.png "discover")

## Clear lab

kind delete clusters kind

## References

- [Fluent Bit Official Manual](https://docs.fluentbit.io/manual)
- [A Fluent Bit Tutorial: Shipping to Elasticsearch](https://logz.io/blog/fluent-bit-tutorial/)
- [Managing application logs with EFK stack](http://www.inanzzz.com/index.php/post/4f7a/managing-application-logs-with-efk-stack-elasticsearch-fluent-bit-kibana-in-kubernetes)
- [Fluent Bit kubernetes logging](https://github.com/fluent/fluent-bit-kubernetes-logging)
- [Exporting Kubernetes Logs to Elasticsearch Using Fluent Bit](https://medium.com/kubernetes-tutorials/exporting-kubernetes-logs-to-elasticsearch-using-fluent-bit-758e8de606af)
- [EFK Stack Setup (Elasticsearch, Fluent-bit and Kibana) for Kubernetes Log Management](https://www.studytonight.com/post/efk-stack-setup-elasticsearch-fluentbit-and-kibana-for-kubernetes-log-management)

## License

The Lab EFK on Kind is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).



