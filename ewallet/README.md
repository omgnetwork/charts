# OmiseGO eWallet

[OmiseGO eWallet](https://github.com/omisego/ewallet) allows businesses and individuals to setup and run their own digital wallet services.

## Introduction

This chart bootstraps [OmiseGO eWallet](https://github.com/omisego/ewallet) deployment on a [Kubernetes](http://kubernetes.io/) cluster using the [Helm](https://helm.sh/) package manager. This chart also optionally includes [PostgreSQL](https://github.com/kubernetes/charts/tree/master/stable/postgresql) to be setup alongside with the application.

## Prerequisites

-   At least 1 GB of RAM available on your cluster.
-   Kubernetes 1.9+
-   Helm 2.3+
-   PV provisioner support in the underlying infrastructure
-   The ability to point a DNS entry to your eWallet install

## Installing

Since OmiseGO Charts is not included in the Helm repository, you must first clone this repository:

```shell
$ git clone https://github.com/omisego/charts.git omisego-charts
```

To install the chart with the release name `my-release`:

```shell
$ helm upgrade -i my-release \
    --set baseUrl=http://www.example.com \
    --set postgresql.enabled=true \
    --set ewalletSecretKey=$(openssl rand -base64 32) \
    --set localLedgerSecretKey=$(openssl rand -base64 32) \
    --wait omisego-charts/ewallet
```

> Note: Setup and upgrading may take a little bit long as setup hooks will perform database migration.

Alternatively, to install the chart with external database:

```shell
$ helm upgrade -i my-release \
    --set baseUrl=http://www.example.com \
    --set ewalletSecretKey=$(openssl rand -base64 32) \
    --set localLedgerSecretKey=$(openssl rand -base64 32) \
    --set ewalletDbUrl=postgres://user:pass@my-postgres-host:5432/ewallet_db \
    --set localLedgerDbUrl=postgres://user:pass@my-postgres-host:5432/local_ledger_db \
    --wait omisego-charts/ewallet
```

For external database, please make sure to create these databases before installing the chart.

> Tip: List all releases using `helm list`

## Uninstalling

To uninstall/delete `my-release` deployment:

```shell
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

> Tip: Use `helm delete --purge my-release` to also delete deploy history.

You will also need to delete jobs that are created as part of the setup process, which are not tracked by Helm:

```shell
$ kubectl delete jobs/my-release-ewallet-init-postgresql jobs/my-release-ewallet-init-db
```

## Configuration

Please see the annotated [values.yaml](values.yaml).
