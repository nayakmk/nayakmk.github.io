---
layout: post
title: 'Helm: Getting Started'
date: '2022-11-13 21:59:16 +0530'
---
## Helm Installation

Ensure you have chocholatey installed already.

> choco install kubernetes-helm

### Add Repo for Helm packages

helm repo add bitnami https://charts.bitnami.com/bitnami

### Searching for a Package

helm search repo postgres

```
NAME: postgres
LAST DEPLOYED: Sun Nov 13 22:05:16 2022
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: postgresql
CHART VERSION: 12.1.2
APP VERSION: 15.1.0

** Please be patient while the chart is being deployed **

PostgreSQL can be accessed via port 5432 on the following DNS names from within your cluster:

    postgres-postgresql.default.svc.cluster.local - Read/Write connection

To get the password for "postgres" run:

    export POSTGRES_PASSWORD=$(kubectl get secret --namespace default postgres-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)

To connect to your database run the following command:

    kubectl run postgres-postgresql-client --rm --tty -i --restart='Never' --namespace default --image docker.io/bitnami/postgresql:15.1.0-debian-11-r0 --env="PGPASSWORD=$POSTGRES_PASSWORD" \
      --command -- psql --host postgres-postgresql -U postgres -d postgres -p 5432

    > NOTE: If you access the container using bash, make sure that you execute "/opt/bitnami/scripts/postgresql/entrypoint.sh /bin/bash" in order to avoid the error "psql: local user with ID 1001} does not exist"

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace default svc/postgres-postgresql 5432:5432 PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432
PS C:\Users\nayakmk> set POSTGRES_PASSWORD=$(kubectl get secret --namespace default postgres-postgresql -o jsonpath="{.data.postgres-password}" | base64 -d)
```

### Connect to The Service

> kubectl port-forward --namespace default svc/postgres-postgresql 5432:5432