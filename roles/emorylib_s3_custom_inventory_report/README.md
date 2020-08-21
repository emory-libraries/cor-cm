emorylib_s3_custom_inventory_report
=========

This role leverages S3 Inventory reports to create a custom inventory report. *emorylib_s3_custom_inventory_report* is intended to supplement existing AWS s3 bucket replication.
Normally to replicate the contents of one s3 bucket to another you would enable S3 Replication, but that requires versioning to be turned on for the source bucket. At Emory Libraries we encountered a bug with our Fedora Commons repository which required us to recreate our s3 bucket with versioning disabled. Since we still needed a way to replicate our repository bucket to a bucket in another region, this role was developed.

The role will take the inventory manifest of two S3 buckets, one designated as the source and the other the destination, and compare their inventories and build a new manifest that contains objects that are in the source bucket but not the destination bucket. This new manifest will be uploaded via awscli to a report bucket and key of the user's choosing. The manifest will contain links to csv.gz files that contain the objects and keys, mimicking the inventory report that AWS provides. There is one additional file generated which contains the number of objects found in the manifest. If no objects are found, then a manifest is generated but no .csv.gz files are uploaded.

Requirements
------------

Please familiarize yourself with the AWS S3 Batch Operations documentation found [here](https://docs.aws.amazon.com/AmazonS3/latest/dev/batch-ops.html) before attempting to run this role.

The source and destination buckets must have s3 inventory manifest generation enabled. We suggest daily inventory reports instead of weekly. The host running this role needs s3 access to all buckets in the role, source, dest, and report. The source and dest inventory reports must have matching inventory fields.

The host running must have awscli installed. We recommend running this on a host inside the aws account of interest.

Role Variables
--------------

```yaml
# Main Input Variable
s3_custom_report:
  source:               # The source is the bucket that will be backed up
    bucket:             # Name of the bucket
    key_prefix:         # Optional, Path to the directory where the S3 inventory reports are uploaded to
    date: ''            # Accepts either an exact data timestamp (MUST BE IN QUOTES) or the word 'latest', latest will get the latest available timestamp
  dest:                 # The dest is the 'backup' bucket. Any object found in source that is not in dest will be inside the custom report.
    bucket:
    key_prefix:
    date: ''
  report:               # The report key is the bucket and key_prefix the custom report will be uploaded to
    bucket:
    key_prefix:         # Optional

# Other variables
s3_custom_report_download_location: /tmp    # Working directory where the .csv files are downloaded and manipulated
s3_custom_report_max_csv_size: 45_000        # Controls how many rows the .csv files are allowed to have before being split into another file
#s3_custom_report_source_bucket:            # Optional variable, use this to manually set the source bucket incase the logic is having trouble,
                                            # This should generally be left commented out
```

Dependencies
------------

There are not any dependencies, but this role is intended to be ran before [__emorylib_role_s3_batch_operations__](https://github.com/emory-libraries/emorylib_role_s3_batch_operations)
If done correctly this will ensure a nightly backup of the source bucket to dest, without occurring unnecessary data transfer costs.

Example Playbook
----------------

```yaml
# Example with an exact date
s3_custom_report:
  source:
    bucket: inventory-reports
    key_prefix: og-bucket/inventory-report
    date: '2020-02-26T00-00Z'  # Exact dates must be in quotes!
  dest:
    bucket: inventory-reports
    key_prefix: backup-bucket/inventory-report
    date: latest
  report:
    bucket: inventory-reports
    key_prefix: og-bucket-to-backup-bucket/inventory-report   # 'inventory-report' is the inventory name we gave S3 for this example, the key could be different or absent entirely.
s3_custom_report_max_csv_size: 100_000                  # For larger csv files
```

License
-------

BSD

Author Information
------------------

Solomon Hilliard for Emory Libraries
