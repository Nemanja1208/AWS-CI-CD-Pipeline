# Setting up CI/CD Pipeline on AWS

In this tutorial, you use CodePipeline to deploy code maintained in a CodeCommit repository to a single Amazon EC2 instance.

Your pipeline is triggered when you push a change to the CodeCommit repository.

The pipeline deploys your changes to an Amazon EC2 instance using CodeDeploy as the deployment service.

The pipeline has two stages:

A source stage (Source) for your CodeCommit source action.

A deployment stage (Deploy) for your CodeDeploy deployment action.

# Prerequisites

1. Create an AWS account and administrative user

2. Apply a managed policy `AWSCodePipeline_FullAccess` for administrative access to `CodePipeline`

3. Install the AWS CLI if you don't already have on and configure it with your Access Key ID, secret, default region name and default output format

# Create a CodeCommit Repository

1. Open the CodeCommit console at `https://console.aws.amazon.com/codecommit/`

2. In the Region selector, choose the AWS Region where you want to create the repository and pipeline.

3. On the Repositories page, and choose Create repository. Name the repo and create it.

4. You can clone it and set up local repository but it is not necessary...

# We add sample code to our CodeCommit Repository

1. Download a sample and save it into a folder or directory on your local computer.

2. You can download sample app here -> `https://docs.aws.amazon.com/codepipeline/latest/userguide/samples/SampleApp_Linux.zip` and unzip the files (place them inside your local repo is you set it up)

3. To upload files to your repository, use one of the following methods.

   3a. Use the CodeCommit console to upload your files

   3b. Use git commands to upload your files

# Create an Amazon EC2 Linux instance and install the CodeDeploy agent

In this step, you create the Amazon EC2 instance where you deploy a sample application.

As part of this process, create an instance role that allows install and management of the CodeDeploy agent on the instance.

The CodeDeploy agent is a software package that enables an instance to be used in CodeDeploy deployments.

You also attach policies that allow the instance to fetch files that the CodeDeploy agent uses to deploy your application and to allow the instance to be managed by SSM.

1. Creating a instance role with `AmazonEC2RoleforAWSCodeDeploy` and `AmazonSSMManagedInstanceCore` policies.

2. Launch a instance of EC2 in the console `https://console.aws.amazon.com/ec2/`

   - Choose Amazon Linux AMI and t3.micro and for the purpose of this you can continue without key pair...

   - Under Network settings In Auto-assign Public IP, make sure the status is Enable.

   - Next to Assign a security group, choose Create a new security group.

   - In the row for SSH, under Source type, choose My IP.

   - Choose Add security group, choose HTTP, and then under Source type, choose My IP.

   - Expand Advanced details. In IAM instance profile, choose the IAM role you created in the previous procedure (for example, EC2InstanceRole).

   - Under Summary, under Number of instances, enter 1..

   - Choose Launch instance. After a couple of second you can see your instance created...

# Create an application in CodeDeploy

1. Create a CodeDeploy service role `AWSCodeDeployRole` in the IAM console

2. Create an application in CodeDeploy `https://console.aws.amazon.com/codedeploy` and choose EC2/On-premises as platform

3. Create a deployment group in CodeDeploy

   - A deployment group is a resource that defines deployment-related settings like which instances to deploy to and how fast to deploy them.

   3a. Choose name and add service role we create earlier,

   3b. Deployment type `in place`

   3c. Choose Amazon EC2 instances and choose yours

   3d. Under Agent configuration with AWS Systems Manager, choose Now and schedule updates

   3e. Under Deployment configuration, choose CodeDeployDefault.OneAtaTime

   3f. Under Load Balancer, make sure Enable load balancing is not selected. You do not need to set up a load balancer or choose a target group for this example.

   3g. Create a deployment group

# Create your first pipeline in CodePipeline

1. Go to console and create a pipeline `https://console.aws.amazon.com/codepipeline`

2. Add name and choose `New Service Role` to allow CodePipeline to create a service role in IAM. Leave the settings under Advanced settings at their defaults, and then choose Next.

3. In Source provider, choose `CodeCommit`. In Repository name, choose the name of the CodeCommit repository you created in `Step 1`, choose main or master as branch and click `Next`

4. Skip build stage

5. Add deploy stage - choose CodeDeploy. Choose you app and deployment group and then choose Next step, review and create pipeline.

   - The pipeline starts running after it is created. It downloads the code from your CodeCommit repository and creates a CodeDeploy deployment to your EC2 instance.

   - You can view progress and success and failure messages as the CodePipeline sample deploys the webpage to the Amazon EC2 instance in the CodeDeploy deployment.

   - To verify that your pipeline ran successfully

   - View the initial progress of the pipeline. The status of each stage changes from No executions yet to In Progress, and then to either Succeeded or Failed. The pipeline should complete the first run within a few minutes.

   - After Succeeded is displayed for the pipeline status, in the status area for the Deploy stage, choose CodeDeploy. This opens the CodeDeploy console. If Succeeded is not displayed see Troubleshooting CodePipeline.

   - On the Deployments tab, choose the deployment ID. On the page for the deployment, under Deployment lifecycle events, choose the instance ID. This opens the EC2 console.

   - On the Description tab, in Public DNS, copy the address (for example, ec2-192-0-2-1.us-west-2.compute.amazonaws.com), and then paste it into the address bar of your web browser.

   - The web page displays for the sample application you downloaded and pushed to your CodeCommit repository.
