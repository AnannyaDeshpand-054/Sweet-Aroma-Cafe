🚀 CI/CD Pipeline with AWS CodePipeline & CodeDeploy
📖 Project Overview

This project demonstrates how to implement an end-to-end CI/CD pipeline using AWS CodePipeline and AWS CodeDeploy. The pipeline automates the process of deploying a web application from a GitHub repository to an EC2 Linux instance, ensuring faster and more reliable software delivery.

🏗️ Architecture

GitHub Repository → Stores the source code of the web application

AWS CodePipeline → Orchestrates the CI/CD workflow

AWS CodeDeploy → Automates deployment of code to the EC2 instance

EC2 Linux Instance → Target server running the deployed application

IAM Roles & Policies → Provide secure access to AWS services

⚙️ Implementation Steps
**1. Setup GitHub Repository:**
-Create a repository on GitHub with your web application code.
-Add an AppSpec file (appspec.yml) to define deployment instructions (e.g., copy files, start services).
-Create a  scripts folder and add file called restsrt_server.sh 


**2. Configure EC2 Instance:**
-Launch an EC2 Linux instance.(cicd_demo)
-Install and configure the CodeDeploy Agent:
```bash
sudo yum update -y
sudo yum install ruby -y
sudo yum install wget -y
cd /home/ec2-user
wget https://aws-codedeploy-<region>.s3.<region>.amazonaws.com/latest/install
chmod +x ./install
sudo ./install auto
sudo service codedeploy-agent start
```
<img width="1618" height="279" alt="image" src="https://github.com/user-attachments/assets/43e80520-dce5-471f-82d0-0caba2c28c03" />

<img width="1894" height="681" alt="image" src="https://github.com/user-attachments/assets/647a2ff4-b7b2-44bc-9e41-43bf704ae368" />

**3. Create IAM role for EC2 instance:**
-Create a IAM role.(ec2role_cicd_demo)
-Trusted entity type-->aws services.
-Use case-->EC2.
-Attach AmazonEC2RoleforAWSCodeDeploy (old name — still available in many accounts).
-Create a role.

<img width="1561" height="455" alt="image" src="https://github.com/user-attachments/assets/6fbd1e36-86c6-429f-bca4-b18523c32088" />
-Attach that role ec2 created.
<img width="1892" height="434" alt="image" src="https://github.com/user-attachments/assets/951498af-f100-44be-a082-af07d5433b99" />

**4. create a Codedeploy Application:**
-In AWS Console, create a CodeDeploy Application.(cicd_demo_app)
-Compute platform-->EC2.
-Define a Deployment Group linked to your EC2 instance (via tags or Auto Scaling Group).
-Create Deployment Group(cicd_demo_group):
-Select your EC2 instances by tag or Auto Scaling group.
-Assign the CodeDeploy service role (CodeDeployServiceRole) to the deployment group.(codedeployrole_cicd_demo)
-Configure deployment settings (e.g., all-at-once, rolling, etc.).
-Attach the required IAM Role to allow CodeDeploy access.

<img width="1561" height="491" alt="image" src="https://github.com/user-attachments/assets/d9032238-8b5c-4b5e-83b1-787d97b4fe70" />

<img width="1067" height="825" alt="image" src="https://github.com/user-attachments/assets/b30cffc5-19cc-40f8-90ae-5c3788669c5f" />

<img width="992" height="855" alt="image" src="https://github.com/user-attachments/assets/7a850f7a-ef5d-4f56-ada7-342a0ef81db7" />

<img width="1036" height="790" alt="image" src="https://github.com/user-attachments/assets/e3bca7b8-8633-4940-82bd-b52835c048e8" />

-Now the deployment group is created.
<img width="1433" height="238" alt="image" src="https://github.com/user-attachments/assets/82c19e33-1550-4b03-9a3f-2a05d35edc99" />


**5. Create AWS CodePipeline:**
-Choose creation option -Build custom pipeline
-provide a name to pipeline --> cicd_demo_pipeline
-Source stage → GitHub / CodeCommit:
-GitHub: generate and add a personal access token to CodePipeline.
-Deploy stage → CodeDeploy:
-Link to the correct application and deployment group.
-Ensure artifact location in S3 matches what CodeDeploy expects.
-Save and start the pipeline.

<img width="1011" height="787" alt="image" src="https://github.com/user-attachments/assets/65337de4-8d8e-4d1d-826e-2cda6b6bd0b0" />

<img width="998" height="794" alt="image" src="https://github.com/user-attachments/assets/3e7df0a7-ee26-4b86-8890-7dd57fad930d" />

<img width="1004" height="798" alt="image" src="https://github.com/user-attachments/assets/424dca24-c3cc-4146-8cc7-6cf190dc8113" />

**6. Test the Pipeline*
-Push a code change to GitHub.
-CodePipeline detects the change → triggers CodeDeploy → deploys updated code to EC2.
-Verify deployment by accessing the web application via the EC2 public IP or domain.


✅ Benefits

End-to-End Automation → No manual deployments

Reliability → Consistent deployment process with rollback support

Scalability → Works with Auto Scaling Groups for larger deployments

Speed → Faster release cycles with minimal downtime

🔮 Future Enhancements

Integrate Unit Testing in the pipeline before deployment

Add Approval Stages for manual verification in production

Enable Blue/Green Deployments for zero-downtime releases

Integrate with CloudWatch for deployment monitoring and alerts
