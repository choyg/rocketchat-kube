# rocketchat-kube

Barebones Kubernetes install for [Rocket.Chat](https://rocket.chat/).

This deploys a highly unavailabe installment of mongodb with a single replica. Use at your own despair.

The current storage class assumes GCP as the [provisioner](https://github.com/choyg/rocketchat-kube/blob/192fb8b79b2eaa993cecff94cbb3748139f2a1d0/infra/cluster/storage.yaml#L5). This value can be easiy swapped out from [this list](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner)

## Usage
1. Update the [configmap](https://github.com/choyg/rocketchat-kube/blob/192fb8b79b2eaa993cecff94cbb3748139f2a1d0/infra/rocket/base/rocket-config.yaml#L3) with your base url
1. `kubectl apply -k ./infra/cluster` Create namespace and storage provisioner.
1. `kubectl apply -k ./infra/mongo/base` Deploy a single replicated mongo.
1. Exec into the mongo container and launch `mongo`. Ensure the replica set actually worked with `rs.status()`. If the error says something about not initiated, run `rs.initiate()`
1. `kubectl apply -k ./infra/rocket/base` Deploy the actual rocket chat stuff.

## FAQ
> This doesn't create a way to connect to the rocket chat server

`kubectl expose deployment rocketchat --type=LoadBalancer --name=oooooooo`

> This is a sacriligeous use of Kustomize

True

> Why does the mongodb install require a manual step

https://stackoverflow.com/questions/25585452/how-to-initialize-mongodb-replication-set-without-calling-rs-initiate
