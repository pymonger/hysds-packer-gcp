# hysds-packer-gco
Packer adapation for HySDS component images for Google Cloud Platform (GCP).  

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
