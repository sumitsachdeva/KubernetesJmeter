[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)
# kubernetes-jmeter

Jmeter test workload inside kubernetes. [Jmeter](charts/jmeter) chart bootstraps an Jmeter stack on a Kubernetes cluster using the Helm package manager.

Currently [jmeter](charts/jmeter) helm chart deploy:
*   Jmeter master
*   Jmeter slaves
*   InfluxDB instance with graphite interface as a jmeter backend
*   Grafana instance


## Installation
```
git clone git@github.com:kaarolch/kubernetes-jmeter.git
cd kubernetes-jmeter/charts/jmeter
helm install --namespace YOUR_NAMESPACE install -n test ./
```
If you would like to provide custom values.yaml you can add `-f` flag.

```
helm install --namespace YOUR_NAMESPACE install -n test ./ -f my_values.yaml
```

The command deploys Jmeter on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

If you change deployment name (`-n test`) please update grafana datasource influx `url` inside your custom values.yaml files.

## Run sample test

### Manual run
Copy example test

```
kubectl -n YOUR_NAMESPACE cp examples/simple_test.jmx $(kubectl -n YOUR_NAMESPACE get pod -l "app=jmeter-master" -o jsonpath='{.items[0].metadata.name}'):/test/

```
Run tests

```
kubectl -n YOUR_NAMESPACE exec  -it $(kubectl -n YOUR_NAMESPACE get pod -l "app=jmeter-master" -o jsonpath='{.items[0].metadata.name}') -- sh -c 'ONE_SHOT=true; /run-test.sh'
```

### Run test via configmap

TBD

## Remove stack

```
helm delete YOUR_RELEASE_NAME --purge
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The default configuration values for this chart are listed in [values.yaml](charts/jmeter/values.yaml).

## Project status

Currently kubernetes-jmeter project is able to run some test on distributed slaves but there still is a lot to do. In few days there should be some documentation added to this repo.

## To Do
Everything ;)
1.  Visualization stack (Grafana + influxdb)
*   Add default dashboard after deployment
2.  Helm charts - 80% of base chart
*   Auto update influxdb datasource base on release name currently there is fixed test-influx host added.
*   Resource limitation
3.  Jmeter test get from maven (0%)
4.  Jmeter test get from git (20%) - still not push to master
5.  Default grafana dashboard
7.  SSL between Jmeter nodes
8.  Documentation (30%)
9.  Release of a helm charts and helm repo update process via travis
