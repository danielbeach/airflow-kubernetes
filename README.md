# airflow-kubernetes
Running Airflow inside Kubernetes

Deploying all these YAML files to a kubernetes cluster will allow Airflow to run and be accessable via public HTTP/Internet.

Complete walkthrough blog. https://www.confessionsofadataguy.com/deploying-apache-airflow-inside-kubernetes/

1. Setup Kubernetes Cluster and connect kubectl. (try Linode)
2. pull official Apache Airflow Docker image `docker pull apache/airflow`
3. `kubectl apply -f postgres-airflow.yaml`
4. `kubectl apply -f postgres-service.yaml`
5. `kubectl get pods` get name of Postgres POD
6. SSH into Postgres POD `kubectl exec --stdin --tty postgres-airflow-5878785456-2knp7 -- /bin/bash`
7. Once into POD run `apt-get update`
8. `apt get install python3-pip`
9. Install Airflow.  `pip3 install \
                      apache-airflow[postgres]==1.10.10 \
                      --constraint \
                      https://raw.githubusercontent.com/apache/airflow/1.10.10/requirements/requirements-python3.7.txt`
10. Set environment variable. `export AIRFLOW__CORE__SQL_ALCHEMY_CONN=postgresql://airflow:airflow@localhost:5432/airflow`
11. run command `airflow initdb` , then exit SSH to machine.
12.`kubectl apply -f airflow-kubernetes.yaml`
13. `kubectl describe pods` You should see your Airflow PODs.
14. `kubectl apply -f airflow-service.yaml` Now your Airflow UI is available via HTTP over Internet.
15. run `kubectl get services airflow` Note External IP and PORT. You can get to your Airflow UI via http://{externalIP}:{port}
