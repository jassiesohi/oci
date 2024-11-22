

1) Prepare the format of your VM: https://blogs.oracle.com/cloud-infrastructure/post/import-a-vmware-virtual-machine-to-oracle-cloud-infrastructure

2) Bring your own image: https://docs.oracle.com/en-us/iaas/Content/Compute/References/bringyourownimage.htm

3) Create a bucket in the object storage: https://docs.oracle.com/en-us/iaas/Content/Object/Tasks/managingbuckets_topic-To_create_a_bucket.htm

4) Install and configure the CLI to load the VMDK file: https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/cliinstall.htm#InstallingCLI__windows

5) After successfully completing task 4, upload the file (Namefile.vmdk) from CLI to the bucket following this code:

oci os object put -bn <bucket-name> --file <file-path>

Replace "<bucket-name>" with the name of the bucket you want to upload the file to, "<file-path>" with the path to the file on your computer.

6) Import the custom image from bucket.

7) You have imported the custom image, you can create the instance from it.

8) You can start the instance.

