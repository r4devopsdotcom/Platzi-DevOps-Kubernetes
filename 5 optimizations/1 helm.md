# Helm
Is a package manager for kubernetes, to easy install and deploy components.

# Add/Update a repository
- helm repo add <repository_name> <repository_url>
- helm repo update

# List repo charts
- helm search repo <repository_name>

# Get info of a chart
- helm show chart bitnami/mysql
- helm show all bitnami/mysql

# Install a chart
- helm repo install <chart_name> --generate-name
- helm install bitnami/mysql --generate-name

# see what has been released
- helm list

# Uninstall a Release
- helm uninstall <release_name>

# Create a helm chart of an application
- helm create <chart_name>
- mv templates old_templates                    # replace templates folder

**Get the yaml file for 1 deploy and then for all deployments with a bash script**
- kubectl get -o yaml --export deployment worker
- while read kind name; do

**Bash Script to get the yamk file of all deployments**
while read kind name; do
kubectl get -o yaml $kind $name > $name-$kind.yaml
done <<EOF
deployment worker
deployment hasher
daemonset rng
deployment webui
deployment redis
service hasher
service rng
service webui
service redis
EOF

# install the created chart
helm install <created_chart>