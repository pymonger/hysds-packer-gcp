# hysds-packer-templates
Packer templates for HySDS.

## Requisites
- Install packer: https://packer.io/

## Base CentOS 7 Image

### OpenStack
1. Download your OpenStack RC file and source it
2. Using the OpenStack dashboard, import the official CentOS 7 cloud image
  - http://cloud.centos.org/centos/7/images/CentOS-7-x86_64-GenericCloud.qcow2
  - take note of the image ID; this will be your *source_image* value
3. Validate:

    ```
    packer validate -var 'image_name=centos-7-x86_64' -var 'source_image=<source_image>' \
      -only=jpl-openstack centos7.json 
    ```
4. If no errors validating, build:

    ```
    packer build -var 'image_name=centos-7-x86_64' -var 'source_image=<source_image>' \
      -only=jpl-openstack centos7.json 
    ```

## HySDS Components

### OpenStack
1. Determine which HySDS component you want to create an image for
  - mozart, metrics, grq, factotum, verdi, autoscale (verdi with autoscale services)
  - this will be your *hysds_component* value
  - this will also be your *hysds_component_name* value except in the case of
    the autoscale *hysds_component* which should have the *hysds_component_name*
    verdi-autoscale or just verdi
2. Download your OpenStack RC file and source it
3. Using the OpenStack dashboard, import the official CentOS 7 cloud image
  - http://cloud.centos.org/centos/7/images/CentOS-7-x86_64-GenericCloud.qcow2
  - take note of the image ID; this will be your *source_image* value
4. Get or create a [personal access token](https://github.jpl.nasa.gov/settings/tokens) for JPL github access to the hysds-org code repository
  - this token will be your *git_oauth_token* value
5. Validate:

    ```
    packer validate -var 'image_name=centos-7-x86_64-<hysds_component_name>' \
      -var 'source_image=<source_image>' \
      -var 'git_oauth_token=<git_oauth_token>' \
      -var 'hysds_component=<hysds_component>' \
      hysds_openstack-<jpl|mimosa|bellini>.json 
    ```
6. If no errors validating, build:

    ```
    packer build -var 'image_name=centos-7-x86_64-<hysds_component_name>' \
      -var 'source_image=<source_image>' \
      -var 'git_oauth_token=<git_oauth_token>' \
      -var 'hysds_component=<hysds_component>' \
      hysds_openstack-<jpl|mimosa|bellini>.json 
    ```

### AWS
1. Make sure you have your AWS access and secret keys.
2. Determine which HySDS component you want to create an image for
  - mozart, metrics, grq, factotum, verdi, autoscale (verdi with autoscale services)
  - this will be your *hysds_component* value
  - this will also be your *hysds_component_name* value except in the case of
    the autoscale *hysds_component* which should have the *hysds_component_name*
    verdi-autoscale or just verdi
2. Determine how much space you need on the root volume of the instance
  - this will be your *volume_size* value in GB (default is 200GB)
3. Using the AWS dashboard, create a security group that will allow SSH access from the machine that you will run packer from
  - this will be your *security_group* value
4. Using the AWS dashboard, determine the region you want to work in and the AMI ID of the official CentOS 7 cloud image
  - to get AMI ID for CentOS 7 images, click on the "Manual Launch" tab [here](https://aws.amazon.com/marketplace/ordering?productId=b7ee8a69-ee97-4a49-9e68-afaee216db2e&ref_=dtl_psb_continue)
  - take note of the region; this will be your *region* value
  - take note of the AMI ID; this will be your *source_ami* value
5. Get or create a [personal access token](https://github.jpl.nasa.gov/settings/tokens) for JPL github access to the hysds-org code repository
  - this token will be your *git_oauth_token* value
6. Validate:

    ```
    packer validate -var 'access_key=<access_key>' \
      -var 'secret_key=<secret_key>' \
      -var 'region=<region>' \
      -var 'ami_name=centos-7-x86_64-<hysds_component_name>' \
      -var 'source_ami=<source_ami>' \
      -var 'volume_size=<volume_size>' \
      -var 'security_group=<security_group>' \
      -var 'git_oauth_token=<git_oauth_token>' \
      -var 'hysds_component=<hysds_component>' \
      hysds_aws.json
    ```
7. If no errors validating, build:

    ```
    packer build -var 'access_key=<access_key>' \
      -var 'secret_key=<secret_key>' \
      -var 'region=<region>' \
      -var 'ami_name=centos-7-x86_64-<hysds_component_name>' \
      -var 'source_ami=<source_ami>' \
      -var 'volume_size=<volume_size>' \
      -var 'security_group=<security_group>' \
      -var 'git_oauth_token=<git_oauth_token>' \
      -var 'hysds_component=<hysds_component>' \
      hysds_aws.json
    ```

### Azure
1. Download your azure connection credentials.
2. Determine which HySDS component you want to create an image for
  - mozart, metrics, grq, factotum, verdi, autoscale (verdi with autoscale services)
  - this will be your *hysds_component* value
  - this will also be your *hysds_component_name* value except in the case of
    the autoscale *hysds_component* which should have the *hysds_component_name*
    verdi-autoscale
3. Setup a base CentOS-7 image (with SELinux fixes, tty-less sudo), capture it, and note that image's description/label.  This will be the image name.

4. Validate:

    ```
    packer validate -var 'image_name=hysds-azure-'<hysds_component_name> \
      -var 'git_oauth_token=<git_oauth_token>' \
      -var 'hysds_component='<hysds_component> \
      \
      -var 'source_image=<Azure images' lable/description>' \
      -var 'azure_subscription_name=<azure subscription name>' \
      -var 'azure_storage_account=<azure storage account>' \
      -var 'azure_publish_settings_path=<path/to/azure/publish/settings>' hysds_azure.json
    ```

5. Given no validation errors, build:

    ```
    packer build -var 'image_name=hysds-azure-'<hysds_component_name> \
      -var 'git_oauth_token=<git_oauth_token>' \
      -var 'hysds_component='<hysds_component> \
      \
      -var 'source_image=<Azure images' lable/description>' \
      -var 'azure_subscription_name=<azure subscription name>' \
      -var 'azure_storage_account=<azure storage account>' \
      -var 'azure_publish_settings_path=<path/to/azure/publish/settings>' hysds_azure.json
    ```

### Google Cloud Platform (Google Compute Engine)
1. Make sure you have your account file (account.json).
2. Determine which HySDS component you want to create an image for
  - mozart, metrics, grq, factotum, verdi, autoscale (verdi with autoscale services)
  - this will be your *hysds_component* value
  - this will also be your *hysds_component_name* value except in the case of
    the autoscale *hysds_component* which should have the *hysds_component_name*
    verdi-autoscale
3. Determine how much space you need on the root volume of the instance
  - this will be your *volume_size* value in GB (default is 200GB)
4. Get or create a [personal access token](https://github.jpl.nasa.gov/settings/tokens) for JPL github access to the hysds-org code repository
  - this token will be your *git_oauth_token* value
5. Validate:

    ```
    packer validate -var 'account_file=<GCP account file>' \
      -var 'project_id=<GCP project id>' \
      -var 'source_image=<source_image>' \
      -var 'disk_size=<disk_size>' \
      -var 'image_name=hysds-centos7-<hysds_component_name>' \
      -var 'git_oauth_token=<git_oauth_token>' \
      -var 'hysds_component=<hysds_component>' \
      hysds_gce.json
    ```
7. If no errors validating, build:

    ```
    packer build -var 'account_file=<GCP account file>' \
      -var 'project_id=<GCP project id>' \
      -var 'source_image=<source_image>' \
      -var 'disk_size=<disk_size>' \
      -var 'image_name=hysds-centos7-<hysds_component_name>' \
      -var 'git_oauth_token=<git_oauth_token>' \
      -var 'hysds_component=<hysds_component>' \
      hysds_gce.json
    ```
