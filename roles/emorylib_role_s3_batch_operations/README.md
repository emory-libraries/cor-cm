emorylib_role_s3_batch_operations
=========

This role will submit and optionally track a S3 Batch Operation Job. There is also optional slack notifications for the results of the job. This role assumes the batch operations inventory reports are in .csv format.

Requirements
------------

An understanding of how S3 Batch Operations function. Current Batch Operations supported by AWS are listed [here](https://docs.aws.amazon.com/AmazonS3/latest/dev/batch-ops-operations.html).
This README will focus on the Tagging and Copy operations, but others should also work.

Role Variables
--------------

```yaml
s3_batch_op: # Primary input which contains the batch operation

default_s3_batch_op_region: # Define a default operation region instead of defining it in each indiviual operation. Can be overriden.
default_s3_batch_op_account_id: # Define a default operation account id instead of defining it in each indiviual operation. Can be overriden.
default_s3_batch_op_role_arn: # Define a default operation role arn instead of defining it in each indiviual operation. Can be overriden.

s3_batch_op_submit_only: #Set to false to enable advanced features like tracking and reporting, default is true
## The following report variables require that the batch operation have a report section
s3_batch_op_generate_direct_download: # Set to true to generate a direct link to the csv report, direct links are better for public buckets, default is false
s3_batch_op_generate_console_download: # Set to true to generate a console link to the csv report, console links are better for private buckets, default is false
########################################################################################
# Slack Options
s3_batch_op_report_to_slack: #Set to true to generate a slack message, requires slack options be set/
```

Additional Slack options can be found in the [slack_variables.yml](./defaults/main/slack_variables.yml) file.

Example Playbook
----------------

```yaml
      ## bucket B is in us-west-2, bucket A us-east-1
    - hosts: localhost
      roles:
         - emorylib_role_s3_batch_operations
      vars:
        default_s3_batch_op_region: us-east-1
        default_s3_batch_op_account_id: 12345678910
        default_s3_batch_op_role_arn: arn:aws:iam::12345678910:role/batch-op-role
        s3_batch_op: '{{ copy_from_bucket_A_to_bucket_B }}'
        copy_from_bucket_A_to_bucket_B:
          ## This example will use the standard batch operation manifest as the input

          #role_arn: Use default, no need to define it
          #account_id: Use default, no need to define it here
          operation:
            S3PutObjectCopy:
              TargetResource: arn:aws:s3:::bucket_B
          manifest:
            Spec:
              Format: S3InventoryReport_CSV_20161130
            Location:
              ObjectArn: arn:aws:s3:::bucket_A/manifest_key_path/2020-03-22T00-00Z/manifest.json 
              ETag: 23dfb69aa65bfcc7378ced950f031b0d
          report:
            Bucket: arn:aws:s3:::report_location_bucket
            Prefix: report_location_key/report
            Format: Report_CSV_20180820
            Enabled: true
            ReportScope: AllTasks
          priority: 1
          region: us-west-2 #override default region
          description: Optional description goes here

        copy_from_bucket_B_to_bucket_A:
          ##This example will use the role's logic to grab a Manfiest.Location.ObjectArn/Etag with just a date and the key prefix

          role_arn: arn:aws:iam::10987654321:role/batch-op-role-2 # override default role arn
          account_id: 10987654321 # override default account id
          #region: # Use default region
           operation:
            S3PutObjectCopy:
              TargetResource: arn:aws:s3:::bucket_A
          manifest:
            Spec:
              Format: S3InventoryReport_CSV_20161130
          ## Don't specify Location.Manifest when using location
          report:
            Bucket: arn:aws:s3:::report_location_bucket
            Prefix: report_location_key/report
            Format: Report_CSV_20180820
            Enabled: true
            ReportScope: AllTasks
          location:
            KeyPrefix: bucket_B/manifest_key_path/manifest/
            Date: '2020-03-17' #Date must be in this format and it must be in quotes or the role will error out
            #Date: latest      #Using "latest" will look for the latest manifest in the key prefix
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
