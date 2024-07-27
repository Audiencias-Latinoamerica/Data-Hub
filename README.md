<p align="center">
<image
  src="https://github.com/Audiencias-Latinoamerica/.github/assets/4085605/ff9828b4-a69f-4c68-9f2c-7ba2c7a354ab"
  height=200
  margin=0>
</p>


# Instructions for Uploading Data to S3


To facilitate data transfer, we have set up an S3 bucket where you can deposit your data into the appropriate folders. The security level of the data is high as the buckets are secure. To access the bucket, you will need an AWS access key and secret key. Below are the instructions and the data categories for uploading.

## Folder Structure

1. **Activity**
   - **Description:** Data related to the activity of set-top boxes or OTTs.
   - **Path:** `s3://<bucket-name>/activity/`
   - **Recommended File Formats:** Parquet, JSONL, CSV, TSV

2. **Users**
   - **Description:** User information with sensitive data anonymized using SHA256 and MD5.
   - **Path:** `s3://<bucket-name>/users/`
   - **Recommended File Formats:** Parquet, JSONL, CSV, TSV
   - **Note:** Ensure all sensitive information is properly anonymized.

3. **EPG**
   - **Description:** Electronic Program Guide (EPG) and Channels information.
   - **Path:** `s3://<bucket-name>/epg/`
   - **Recommended File Formats:** Parquet, JSONL, CSV, TSV

4. **Devices**
   - **Description:** Information about the devices used.
   - **Path:** `s3://<bucket-name>/devices/`
   - **Recommended File Formats:** Parquet, JSONL, CSV, TSV

## General Recommendations

- **File Formats:** We recommend using standard formats such as Parquet, JSONL, CSV, or TSV to facilitate integration and data analysis.
- **File Naming:** Use descriptive names and dates in the format YYYYMMDD to make identification easier. Example: `activity_20240709.parquet`
- **Uploading Files:**
  - Use the AWS CLI, or any tool compatible with S3 to upload files.
  - Ensure you have the necessary permissions to write to the respective folders.
  - Verify that files are uploaded correctly.

## Installing AWS CLI

To upload files using AWS CLI, you need to install it first:

1. **Windows:**
   - Download the installer from the [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html).
   - Run the installer and follow the instructions.

2. **macOS:**
   - Use Homebrew to install AWS CLI:
     ```sh
     brew install awscli
     ```

3. **Linux:**
   - Use the following commands to install AWS CLI:
     ```sh
     curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
     unzip awscliv2.zip
     sudo ./aws/install
     ```

After installation, configure AWS CLI with your credentials:
```sh
aws configure
```
Enter your AWS Access Key ID, Secret Access Key, region, and output format when prompted.

## Using Boto3 for Uploading Files

As an alternative, you can use the Boto3 library in Python to upload files to S3. Below is an example script:

1. **Install Boto3:**
   ```sh
   pip install boto3
   ```

2. **Example Python Script:**
   ```python
   import boto3
   from botocore.exceptions import NoCredentialsError

   def upload_to_aws(local_file, bucket, s3_file):
       s3 = boto3.client('s3')
       try:
           s3.upload_file(local_file, bucket, s3_file)
           print("Upload Successful")
           return True
       except FileNotFoundError:
           print("The file was not found")
           return False
       except NoCredentialsError:
           print("Credentials not available")
           return False

   uploaded = upload_to_aws('local_file.parquet', '<bucket-name>', 'Activity/local_file.parquet')
   ```

## AWS CLI Command Examples

For those who prefer using the command line, here are some examples of how to upload files:

- **Upload a file to Activity:**
  ```sh
  aws s3 cp activity_20240709.parquet s3://<bucket-name>/activity/
  ```

- **Upload a file to Users:**
  ```sh
  aws s3 cp users_20240709.jsonl s3://<bucket-name>/users/
  ```

- **Upload a file to EPG:**
  ```sh
  aws s3 cp epg_20240709.csv s3://<bucket-name>/epg/
  ```

- **Upload a file to Devices:**
  ```sh
  aws s3 cp devices_20240709.tsv s3://<bucket-name>/devices/
  ```

## Support

If you have any questions or need assistance, please do not hesitate to contact our support team at [support@bb-media.com](mailto:support@bb-media.com) or [jm@bb-media.com](mailto:jm@bb-media.com).
