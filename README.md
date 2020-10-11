# mediawiki
Contains docker and kubernetes files for setting up the Mediawiki v1.34 service on Kubernetes cluster
The YAML files are generated with some pre-configured values and needs to be modified for further generalization.

## Dockerfiles
* `/mediawiki-1.34/Dockerfile` - For installing and configuring Mediawiki v1.34 on a Centos7 container.
* `/mariadb-wiki/Dockerfile`  - For installing and configuring Mariadb for handling Mediawiki database.

## Docker-compose
* `docker-compose.yml` file has been provided to setup the Mediawiki and Mariadb services on a docker node.

## Kubernetes
The kubernetes objects are defined in following files: `mediawiki-deployment.yaml` `mediawiki-service.yaml` `db-deployment.yaml` `db-service.yaml` `env-configmap.yaml` `wikinetwork-networkpolicy.yaml` `secret.yaml`

* `secret.yaml` file is created for storing base64 converted password string to be used for variable MYSQL_ROOT_PASSWORD.

Replace the encrypted string by generating a new string using command: `echo -n "<new_password>" | base64`

### Deploying the Stack
On the Kubernetes workstation, run command:

```bash
kubectl apply -f mediawiki-deployment.yaml,mediawiki-service.yaml,db-deployment.yaml,db-service.yaml,env-configmap.yaml,secret.yaml
```

Once both the Mediawiki and db pods are up and running, the app UI can be accessed using the Mediawiki service hostname/ip and port.

Configure the Mediawiki by providing the values as below:
* Database Host : `db:33060`
* Database Name : `wikidatabase`
* Database Username : `root`
* Database Password : `Passw0rd`

After the DB connections are setup, provide details for setting up your new Wiki.

After final submission you it will generate a LocalSettings.php file which needs to be copied on the mediawiki container at location: `/opt/rh/httpd24/root/var/www/html/mediawiki`. The Mediawiki deployment needs to be modified to change this directory location as a mount point from localhost or a S3 bucket.

# Helm Chart

Install the Helm chart using command: 

```bash
helm install mediawiki ./mediawiki-chart
```
