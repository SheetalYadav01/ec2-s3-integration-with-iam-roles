# ec2-s3-integration-with-iam-roles
Secure Apache hosting on AWS EC2 with SSL/TLS and IAM-based S3 access. Configured a self-signed HTTPS server and used IAM roles with Python (boto3) to list S3 objects securely. Demonstrates cloud security fundamentals and AWS SDK usage for DevSecOps practices.

Part A: Configure Apache Web Server on Amazon Linux to Use SSL/TLS

•	Launched an EC2 instance with Amazon Linux 2 AMI

<img width="509" alt="image" src="https://github.com/user-attachments/assets/aa3ab129-30b6-4abc-9f2a-f1dc4c26f9b2" />

<img width="540" alt="image" src="https://github.com/user-attachments/assets/59e74755-67e9-43df-be1d-8908f8402ba8" />

•	Allowed inbound ports: 22 (SSH), 80 (HTTP), 443 (HTTPS)

<img width="457" alt="image" src="https://github.com/user-attachments/assets/6bea79f2-f327-46b3-a2aa-b0577d0b80e7" />

•	Verified public IP and instance status: Successful SSH connection

 <img width="457" alt="image" src="https://github.com/user-attachments/assets/7738db21-e3e3-4bbd-827d-1553e37a91d2" />

Step 2: Install Apache and SSL Module
•	Executed the following:
sudo yum update -y
sudo yum install -y httpd mod_ssl

 <img width="540" alt="image" src="https://github.com/user-attachments/assets/b3ec6dac-5fca-4272-aa4b-dcffee7bc700" />

Step 3: Start Apache and Enable on Boot
•	Executed the following:
sudo systemctl start httpd
sudo systemctl enable httpd

<img width="540" alt="image" src="https://github.com/user-attachments/assets/cc32ab38-0448-44eb-9f51-17e45472039d" />

Step 4: Generate a Self-Signed SSL Certificate
•	Executed the following:

   sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/pki/tls/private/selfsigned.key \
  -out /etc/pki/tls/certs/selfsigned.crt
  
<img width="540" alt="image" src="https://github.com/user-attachments/assets/56623e08-eca2-436a-96f1-50fd8964711e" />


Step 5: Configure Apache SSL Settings
Executed the following:
•	Edited the file:
sudo vi /etc/httpd/conf.d/ssl.conf

•	Updated paths:

SSLCertificateFile /etc/pki/tls/certs/selfsigned.crt
SSLCertificateKeyFile /etc/pki/tls/private/selfsigned.key

 <img width="551" alt="image" src="https://github.com/user-attachments/assets/f256c26e-651a-4870-824f-775930cba367" />


Step 6: Restarted Apache and Verified HTTPS Access
•	Executed:
sudo systemctl restart httpd

<img width="473" alt="image" src="https://github.com/user-attachments/assets/ccf68391-f67b-4097-937a-20154887c172" />

•	Browser showing HTTPS connection (with SSL warning)

<img width="545" alt="image" src="https://github.com/user-attachments/assets/1fc76286-d93f-423f-b34f-a81fee7cb50b" />

<img width="527" alt="image" src="https://github.com/user-attachments/assets/d5714351-9944-418f-b73a-821b159d5d4d" />

<img width="372" alt="image" src="https://github.com/user-attachments/assets/7c36aa39-6119-4ea6-b960-4b9035d9b68d" />

<img width="540" alt="image" src="https://github.com/user-attachments/assets/2b8b996e-5c2c-4d95-92b1-a3c44d64923b" />


Step 7: Attempted SSL Labs Analysis
Visited: https://www.ssllabs.com/ssltest/analyze.html

<img width="540" alt="image" src="https://github.com/user-attachments/assets/a7342e4c-18ee-4bc4-9887-44bba8aefba9" />


Part B: Use IAM Role to Grant Access to S3 from EC2

Step 1: Create IAM Role with S3 Read-Only Access
•	Created role: EC2S3ReadOnlyRole
•	Attached policy: AmazonS3ReadOnlyAccess

 <img width="540" alt="image" src="https://github.com/user-attachments/assets/639d50e5-721f-4618-a07d-7500b9477831" />

Step 2: Attach IAM Role to EC2 Instance
•	Attached EC2S3ReadOnlyRole to EC2 instance using EC2 Console

<img width="540" alt="image" src="https://github.com/user-attachments/assets/48d27cd0-8a93-43cf-b68b-879dfeb1ee35" />

Step 3: Upload File to S3 Bucket
•	Created bucket: my-s3-accessec2-test-bucket
•	Uploaded file: test.txt

<img width="540" alt="image" src="https://github.com/user-attachments/assets/780bfbc3-58f2-48a6-89f9-9dbeee332eb6" />

Step 4: Install AWS CLI and SDK

<img width="540" alt="image" src="https://github.com/user-attachments/assets/acfeb55f-7711-485a-b41e-2e8c9b2e08fd" />



Step 5: Create and Run Sample Program on EC2
•	Used the following Python script:
 
<img width="540" alt="image" src="https://github.com/user-attachments/assets/47f7727d-94f5-46f6-9149-6afc2d051870" />

•	Executed the following:
python3 s3_list.py
•	Successful output listing test.txt

<img width="540" alt="image" src="https://github.com/user-attachments/assets/2b2be8f3-821f-42af-aa79-6f5eb44552a8" />

