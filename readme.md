# Kubectl templates for combining Grafana, InfluxDB v2, and Telegraf 

Template Kubectl yaml files to read MQTT messages from an MQTT broker and show a chart in Grafana from InfluxDB v2.

The Kubectl yaml files are part of this repo.

Note that Telegraf needs an InfluxDB API token.

## Grafana

Steps to roll out Grafana as Kubernetes pod:

```
sudo kubectl delete namespace nsgrafana

sudo kubectl create namespace nsgrafana

touch grafana.yaml

sudo nano grafana.yaml (see grafana.yaml file)

sudo kubectl apply -f grafana.yaml --namespace=nsgrafana

sudo kubectl get all --namespace=nsgrafana
```

Once deployed, the Grafana website is available at http://192.168.2.196:3000.

* Give the admin user a proper password

## InfluxDB v2

Steps to roll out InfluxDB v2 as Kubernetes pod:

```
sudo kubectl delete namespace nsinfluxdb

sudo kubectl create namespace nsinfluxdb

sudo touch influx.yaml

sudo nano influx.yaml (see influx.yaml file)

sudo kubectl apply -f influx.yaml --namespace=nsinfluxdb

sudo kubectl get all --namespace=nsinfluxdb
``` 

Once deployed, the influxdb website is available at http://192.168.2.196:8086.

* add a user with a password
* fill in an organization (I used 'iotedger')
* fill in a bucket (I used 'telemetry')
* WARNING: Remember the admin token shown here on the page. It will not be shown again! 
* Chose 'configure later' so we have a clean influxdb
* create an API token for the Telegraf agent so it can connect to the InfluxDB

## Telegraf

Steps to roll out Telegraf as Kubernetes pod:

```
sudo kubectl delete namespace nstelegraf

sudo kubectl create namespace nstelegraf

touch telegraf.yaml

sudo nano telegraf.yaml (see telegraf.yaml file)

sudo kubectl apply -f telegraf.yaml --namespace=nstelegraf

sudo kubectl describe pods --namespace=nstelegraf
```

The telegraf agent has no website UI in this case.

* Alter the telegraf.conf so it matches your MQTT broker and topic
* Check the API token of your InfluxDB
* Drop the telegraf.conf in the right folder

## TODO

* turn public ip address into local ip address
* tokens on disk instead of hardcoded
* alternative timestamp value from body via inputs.mqtt_consumer.json_v2 (use correct indentation)

```
  json_time_format = "2006-01-02T15:04:05.999999999Z"
  json_timezone = "UTC"
```

