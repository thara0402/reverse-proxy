# Reverse Proxy with YARP

## Build and Push Docker Images
```shell-session
$ docker build -t thara0402/reverse-proxy:0.1.0 ./
$ docker run --rm -it -p 8000:80 --name reverse-proxy thara0402/reverse-proxy:0.1.0
$ docker push thara0402/reverse-proxy:0.1.0
```

## Create Azure Container Apps environment
```shell-session
$ az group create -n <ResourceGroup Name> -l canadacentral
$ az deployment group create -f ./deploy/main.bicep -g <ResourceGroup Name>
```

## Create Azure Container Apps
```shell-session
$ az containerapp create -n reverse-proxy -g <ResourceGroup Name> \
  -e <Environment Name> -i thara0402/reverse-proxy:0.1.0 \
  -v APPINSIGHTS_INSTRUMENTATIONKEY=<InstrumentationKey> \
  --ingress external --target-port 80 \
  --revisions-mode single --scale-rules ./deploy/httpscaler.json \
  --max-replicas 10 --min-replicas 1
```
