# ACM Hive Cluster

## Description
The `acm_hive_cluster` role prepares and deploys clusters on a Hub cluster.

## Support matrix
The roles supports the following platforms:
* AWS
* GCP
* Azure
* vSphere
* Openstack

## Role Variables
#### State
State of the hive clusters. Could be "present" or "absent".
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

#### SSH Public key
The ssh public key pushed to the cluster nodes during deploy and used to access the nodes.  
The variable is mandatory.
```
ssh_pub_key:
```

#### ACM Hive clusters
Defines hive clusters that should be deployed on the Hub.
**Note** - For more provides and override options, refer to `config-sample.yml` file.
```
acm_hive_clusters:
  # AWS cluster
  - name: cluster-aws
    credentials: aws-creds
    platform: aws
    region: us-east-2
    network:
      cluster: 10.128.0.0/14
      machine: 10.0.0.0/16
      service: 172.30.0.0/16
      type: OVNKubernetes
    hive_cluster_version: "4.12"  # Takes the latest available image
  # GCP cluster
  - name: cluster-gcp
    credentials: gcp-creds
    platform: gcp
    region: us-east1
    network:
      cluster: 10.128.0.0/14
      machine: 10.0.0.0/16
      service: 172.30.0.0/16
      type: OpenShiftSDN
    hive_cluster_version: "4.11"
    fips: true  # Optional variable to enable FIPS on cluster
  # Azure cluster
  - name: cluster-azure
    credentials: azure-creds
    platform: azr
    region: centralus
    network:
      cluster: 10.128.0.0/14
      machine: 10.0.0.0/16
      service: 172.30.0.0/16
      type: OVNKubernetes
    # Specific image could be specified instead of "hive_cluster_version"
    hive_cluster_image: quay.io/openshift-release-dev/ocp-release:4.12.7-multi
  # vSphere cluster
  - name: cluster-vsphere
    credentials: vmware-creds
    platform: vmw
    network:
      cluster: 10.128.0.0/14
      machine: <vsphere_vip cidr>
      service: 172.30.0.0/16
      type: OpenShiftSDN
    network_name: "<vsphere_net_name>"
    network_api_vip: "<vsphere_api_vip>"
    network_ingress_vip: "<vsphere_ingress_vip>"
    hive_cluster_version: "4.11"
  # Openstack cluster
  - name: cluster-openstack
    credentials: openstack-creds
    platform: openstack
    network:
      cluster: 10.128.0.0/14
      machine: 10.0.0.0/16
      service: 172.30.0.0/16
      type: OVNKubernetes
    external_network: <openstack_external_network>
    api_floating_ip: <cluster_floating_ip>
    ingress_floating_ip: <cluster_ingress_floating_ip>
    hive_cluster_version: "4.13"
```

#### Clusters credentials
Define credentials that will be used by the hive clusters during creation.
**Note** - For more provides and override options, refer to `config-sample.yml` file.
```
acm_hive_clusters_credentials:
  # AWS credentials
  - name: aws-creds
    platform: aws
    namespace: <namespace_to_keep_the_credentials>
    base_domain: <cluster_base_domain>
    aws_access_key_id: <aws_access_key_id>
    aws_secret_access_key: <aws_secret_access_key>
  # GCP credentials
  - name: gcp-creds
    platform: gcp
    namespace: <namespace_to_keep_the_credentials>
    base_domain: <cluster_base_domain>
    project_id: <project_id>
    os_service_account_json: |
      { "type": "service_account",
        "project_id": "...",
        "private_key_id": "...",
        "private_key": "...",
        "client_email": "...",
        "client_id": "...",
        "client_x509_cert_url": "...",
        "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
        "auth_uri": "https://accounts.google.com/o/oauth2/auth",
        "token_uri": "https://oauth2.googleapis.com/token",
        "universe_domain": "googleapis.com" }
  # Azure credentials
  - name: azure-creds
    platform: azr
    namespace: <namespace_to_keep_the_credentials>
    base_domain: <cluster_base_domain>
    base_domain_resource_group_name: <base_domain_resource_group_name>
    cloud_name: <azure_cloud_name>
    os_service_principal_json: |
      { "clientId": "...",
        "clientSecret": "...",
        "tenantId": "...",
        "subscriptionId": "..." }
  # vSphere credentials
  - name: vmware-creds
    platform: vmw
    namespace: <namespace_to_keep_the_credentials>
    base_domain: <cluster_base_domain>
    vcenter: <vcenter_url>
    username: <vcenter_username>
    password: <vcenter_password>
    cluster: "vcenter_cluster_name>"
    datacenter: "<vcenter_datacenter_name>"
    default_datastore: <vcenter_datastore_name>
    cacertificate: |
        <vcenter_ca_certificate>
  # Openstack credentials
  - name: openstack-creds
    platform: openstack
    namespace: <namespace_to_keep_the_credentials>
    base_domain: <cluster_base_domain>
    auth_url: <openstack_api_url>
    username: "<username>"
    password: "<password>"
    project_id: "<openstack_project_id>"
    project_name: "<openstack_porject_name>"
    domain_name: <openstack_domain_name>
```

#### OCP image query url
Performs query across the images to allocate the latest available image per requested OCP version.
```
ocp_image_query_url: https://quay.io/api/v1/repository/openshift-release-dev/ocp-release/tag/
```

#### OCP image base url
OCP image url to fetch the image from.
```
ocp_image_base_url: quay.io/openshift-release-dev/ocp-release
```

#### Cluster architecture
Define cluster architecture to be used.  
Allowed values: x86_64, s390x, ppc64le, aarch64
```
cluster_architecture: "x86_64"
```

#### Platform resources
Default resources definition per each platform.  
Cluster specific resources should be defined within the cluster definition under the `acm_hive_clusters` variable.
```
platform_resources:
  aws:
    master_resource:
      type: m5.xlarge
      rootVolume:
        iops: 4000
        size: 100
        type: io1
    worker_resource:
      type: m5.xlarge
      rootVolume:
        iops: 2000
        size: 100
        type: io1
  gcp:
    master_resource:
      type: n1-standard-4
    worker_resource:
      type: n1-standard-4
  azr:
    master_resource:
      type:  Standard_D4s_v3
      osDisk:
        diskSizeGB: 128
    worker_resource:
      type:  Standard_D2s_v3
      osDisk:
        diskSizeGB: 128
      zones:
      - "1"
      - "2"
      - "3"
  vmw:
    master_resource:
      cpus: 4
      coresPerSocket: 2
      memoryMB: 16384
      osDisk:
        diskSizeGB: 120
    worker_resource:
      cpus: 4
      coresPerSocket: 2
      memoryMB: 16384
      osDisk:
        diskSizeGB: 120
  openstack:
    master_resource:
      type: PnTAE.CPU_4_Memory_16384_Disk_50
    worker_resource:
      type: PnTAE.CPU_4_Memory_16384_Disk_50
```

***
The role is able to work in one of the following states:
* Newly deployed cluster.  
  The cluster will be deployed by the [ocp](ocp.md) role and generate `clusters_details_file`.  
  Clusters info will be fetched from this file to generate the token for the next tasks.
* Existing cluster.  
  If the cluster already exists and was not deployed by us, token could be generated by providing the following environment variables prior playbook execution.
  ```bash
  export OC_CLUSTER_API=<ocp_cluster_api>
  export OC_CLUSTER_USER=<ocp_cluster_user>
  export OC_CLUSTER_PASS=<ocp_cluster_pass>

  ansible-playbook playbook.yml -e @config.yml
  ```

***
The variables could be applied to the playbook run, by saving them into a separate yml file and include the file during the playbook execution.  
Note the '@' sign, which is used to apply the variables located within the provided file.

```
ansible-playbook playbooks/acm_hive_cluster.yml -e @/path/to/the/variable/file.yml
```
