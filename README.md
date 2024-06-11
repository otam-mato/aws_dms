# Setting up AWS Database Migration Service (DMS) to transfer data from an on-premises Microsoft SQL Server (MS SQL) to an Amazon S3 bucket



### Step 1: Prepare the Environment

1. **Create an Amazon S3 Bucket:**
   - Sign in to the AWS Management Console.
   - Open the Amazon S3 console.
   - Create a new bucket where the migrated data will be stored.

2. **Set Up IAM Roles and Policies:**
   - **Create an IAM Role for DMS:**
     - Navigate to the IAM console.
     - Create a new role with the following trust relationship:
       ```json
       {
           "Version": "2012-10-17",
           "Statement": [
               {
                   "Effect": "Allow",
                   "Principal": {
                       "Service": [
                           "dms.amazonaws.com",
                           "s3.amazonaws.com"
                       ]
                   },
                   "Action": "sts:AssumeRole"
               }
           ]
       }
       ```
       ![image](https://hackmd.io/_uploads/r13KlqHBC.png)

     - Attach the following policies to the role:
       - `AmazonS3FullAccess`
       - Any custom policies needed to access your on-prem SQL Server (if applicable).

   

### Step 2: Configure the On-Premises Environment

1. **Set Up Network Connectivity:**
   - Ensure your on-premises MS SQL Server can communicate with AWS DMS.
   - Set up a VPN or AWS Direct Connect if necessary.

2. **Create a SQL Server Account for DMS:**
   - Create a user account in your SQL Server with the necessary permissions to read the data.

### Step 3: Create and Configure AWS DMS Components

1. **Create a Replication Instance:**
   - Open the AWS DMS console.
   - Create a new replication instance. Ensure it has sufficient resources and is in the correct VPC and subnet group.
   ![image](https://hackmd.io/_uploads/SJG0RKBBA.png)


2. **Create Source and Target Endpoints:**
   - **Source Endpoint (MS SQL Server):**
     - Specify the connection details for your on-premises SQL Server.
     - Test the connection to ensure DMS can access the database.
   - **Target Endpoint (Amazon S3):**
     - Specify the S3 bucket details.
     - Provide the ARN of the IAM role created earlier.
     - Test the connection to ensure DMS can access the S3 bucket.

3. **Create a Migration Task:**
   - Define a new task to migrate data from the source endpoint (MS SQL Server) to the target endpoint (Amazon S3).
   - Configure the task settings, such as table mappings, transformation rules, and any other relevant options.
   - Start the task and monitor its progress.

### Step 4: Monitor and Validate

1. **Monitor the Migration:**
   - Use the AWS DMS console to monitor the status of the replication instance, endpoints, and migration task.
   - Check CloudWatch logs for any errors or issues.

2. **Validate the Data:**
   - Once the migration task completes, validate the data in the S3 bucket to ensure it matches the source data.
   - Use tools like AWS Glue or Amazon Athena to query and analyze the data in S3 if needed.





