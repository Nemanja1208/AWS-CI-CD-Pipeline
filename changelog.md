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

1.
