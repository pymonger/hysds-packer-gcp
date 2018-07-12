# hysds-packer-gcp
Packer adapation for creating HySDS component images in Google Cloud Platform (GCP).  

## Requisites
- Install packer: https://packer.io/

## HySDS Components

### Google Cloud Platform (Google Compute Engine)
1. Make sure you have your account file (account.json).
  - If not, [generate your service account credential](https://cloud.google.com/storage/docs/authentication#service_accounts). 
2. Determine the source image (default is centos-7-v20180611).
3. Determine which HySDS component you want to create an image for
  - mozart, metrics, grq, factotum, ci, and autoscale
  - this will be your *hysds_component* value
  - this will also be your *hysds_component_name* value except in the case of
    the autoscale *hysds_component* which should have the *hysds_component_name*
    verdi-autoscale
4. Determine how much space you need on the root volume of the instance
  - this will be your *volume_size* value in GB (default is 200GB)
5. Validate:

    ```
    packer validate -var 'account_file=<GCP account file>' \
      -var 'project_id=<GCP project id>' \
      -var 'source_image=<source_image>' \
      -var 'disk_size=<disk_size>' \
      -var 'image_name=hysds-centos7-<hysds_component_name>' \
      -var 'hysds_component=<hysds_component>' \
      hysds_gce.json
    ```
6. If no errors validating, build:

    ```
    packer build -var 'account_file=<GCP account file>' \
      -var 'project_id=<GCP project id>' \
      -var 'source_image=<source_image>' \
      -var 'disk_size=<disk_size>' \
      -var 'image_name=hysds-centos7-<hysds_component_name>' \
      -var 'hysds_component=<hysds_component>' \
      hysds_gce.json
    ```
