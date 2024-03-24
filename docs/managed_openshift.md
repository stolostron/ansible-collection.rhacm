# MANAGED_OPENSHIFT

## Description
The `managed_openshift` role deploys or destroys Managed Openshift clusters.

## Support matrix
The role supports the following platforms:
* ARO (Microsoft Azure Red Hat OpenShift)
* ROSA (Red Hat OpenShift Service on AWS)

## Role default variables
#### State of the cluster
Create or destroy the cluster.  
The state could be 'present' or 'absent'.
```
state: present
```

#### OpenShift pull secret
The pull secret is used to authenticate with the services that are provided by the included authorities, `Quay.io` and `registry.redhat.io`, which serve the container images for OpenShift Container Platform components.  
Use the following link to obtain the pull-secret - https://console.redhat.com/openshift/install/pull-secret  
The variable is mandatory.
```
pull_secret:
```

#### Clusters deployment details
Provide the details of the clusters that needs to be deployed.
```
managed_openshift_clusters:
  # ARO cluster
  - name: cluster-aro
    platform: aro
    base_domain: example.com
    credentials: aro-creds
    region: centralus
    network:
      cluster: 10.128.0.0/14
      service: 172.30.0.0/16
    resource_group_name: <resource_group_name>
    machines:
      master:
        type: Standard_D8s_v3
      worker:
        type: Standard_D4s_v3
        disk_size: 128
        count: 3
    openshift_version: "4.11.26"  # To check for available ARO versions, execute "az aro get-versions --location <location>" command.

  # ROSA cluster
  - name: cluster-rosa
    platform: rosa
    credentials: rosa-creds
    region: us-east-2
    multi_az: true  # Optional. Deploy to multiple datacenters. By default - false
    fips: true  # Optional. Deploy FIPS. By default - false
    network:
      cluster: 10.128.0.0/14
      machine: 10.0.0.0/16
      service: 172.30.0.0/16
    machines:
      worker:
        type: m5.xlarge
        count: 3
    openshift_version: "4.13.6"  # Optional. If not specified, will use the latest.
    skip_tls_verify: true  # Optional. Set the flag in the kubeconfig file.
```

#### Clusters credentials
Credentials that should be used during managed openshift cluster deployment.
```
managed_openshift_clusters_credentials:
  # ARO credentials
  - name: aro-creds
    platform: aro
    client_id: <client_id>
    client_secret: <client_secret>
    tenant_id: <tenant_id>
    subscription_id: <subscription_id>

  # ROSA credentials
  - name: rosa-creds
    aws_access_key_id: <your_key>
    aws_secret_access_key: <your_secret_key>
    rosa_token: <your_rosa_token>
```

#### Managed OpenShift clusters directory
Specifies the directory where clusters credentials and kubeconfig files will be stored.  
A dedicated directory creates for each cluster.
```
managed_openshift_dir: logs/managed_openshift
```
