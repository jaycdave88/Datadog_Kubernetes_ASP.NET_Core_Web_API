## Purpose

This project was meant to run locally on `minikub` with `kubectl`. Verify you have both of these dependencies installed locally before continuing. 

### How to use

Run the following commands: 

1. Update the `agent_daemon.yaml` with your Datadog API Key
2. `eval $(minikube docker-env)`
3. `kubectl create -f agent_daemon.yaml`
4. `kubectl create -f deployment.yaml`
5. `kubectl create -f services.yaml`

Next, navigate to http://192.168.99.100/api/values to produce load... then navigate to your Datadog instance to see traces.
