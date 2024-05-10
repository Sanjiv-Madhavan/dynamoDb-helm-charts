# DynamoDB Local - Kubernetes Setup Steps

Assumed to have setup your own cluster. If needed to create one, refer to [this](https://medium.com/@AlexanderObregon/setting-up-a-kubernetes-development-environment-locally-with-minikube-7462ed071b39) medium article and you shhould have it in no time :)

## Clone Helm Charts

```
$ cd ~/Workspace
$ git clone github.com/Sanjiv-Madhavan/dynamoDb-helm-charts
```

## Install Helm Chart

```
$ helm install aws ./dynamodb-local-k8s-depl
$ kubectl port-forward -n default svc/aws-dynamodb-local 8000:8000

```

## Check Pod logs 
- Suggested app to check pod configs - [Lens](https://k8slens.dev/)
- Pod logs should look like below
    ```
    Initializing DynamoDB Local with the following configuration:
    Port:	8000
    InMemory:	false
    Version:	2.3.0
    DbPath:	null
    SharedDb:	true
    shouldDelayTransientStatuses:	false
    CorsParams:	null
    2024-03-20T06:54:27.489342024Z
    ```

## Download & Install NoSQL Workbench

```
$ cd tmp
$ wget -o WorkbenchDDBLocal-mac.zip https://s3.amazonaws.com/nosql-workbench/WorkbenchDDBLocal-mac.zip # Only for Mac
```
Refer [Installation Steps](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.settingup.install.html)

## Add Localhost Connection

- Go to Operational Builder
- Add Connection for Localhost
-- Host: 127.0.0.1
-- Port: 8000
- Import Sample DynamoDB Tables

## Helm install command

```
$ helm install aws ./dynamodb-local-k8s-depl
```

## Using AWS CLI to create tables - paste in dev terminal and not pod terminal

```
$ # Create Table
$ aws dynamodb create-table --table-name Orders --attribute-definitions AttributeName=orderId,AttributeType=S --key-schema AttributeName=orderId,KeyType=HASH --billing-mode PAY_PER_REQUEST --endpoint-url http://localhost:8000 --region us-east-1
```

```
$ # List Tables
$ aws dynamodb list-tables --endpoint-url http://localhost:8000 --region us-east-1
```
