🔧 Installation Steps
Start your Kubernetes clusterFor Minikube:
minikube start --driver=docker

Install Helm (if not installed)
brew install helm

Add and update Helm repository
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

Create monitoring namespace and install the stack
kubectl create namespace monitoring
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring

This installs:
Prometheus Operator
Prometheus
Grafana
Alertmanager
kube-state-metrics
Node Exporter
ServiceMonitors for core Kubernetes components 
📊 Access Grafana Dashboard
Port-forward Grafana service
kubectl port-forward svc/prometheus-grafana -n monitoring 3000:80

Open in browser Visit: http://localhost:3000
Login credentials
Username: admin
Password: prom-operator
💡 To retrieve the password securely:

kubectl get secret -n monitoring prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 --decode

🧹 Optional: Customize or Upgrade
Override default values using a values.yaml file:

helm upgrade prometheus prometheus-community/kube-prometheus-stack -n monitoring -f values.yaml

Check official chart values for customization options. 

🧹 Uninstall
helm uninstall prometheus -n monitoring
kubectl delete namespace monitoring