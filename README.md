# Monitor Applications and Server Resources

October 1, 2023

By:  Annie V Lam - Kura Labs

# Purpose

Monitor application and server resources using AWS Cloudwatch.

Previously, the CI/CD pipeline was automated using Jenkins and GitHub webhook.  For this deployment, the Virtual Private Cloud, availability zone, and subnets were configured. Cloudwatch was integrated with the EC2 where it can monitor the EC2 resources via Cloudwatch agents. Instead of using Elastic Beanstalk CLI to deploy the application, GUnicorn was used and deployed to the Nginx web server

## Step #1 Map Out the Deployment

![Deployment Flowchart](Images/Deployment_Pipeline.png)

## Step #2 GitHub/Git

GitHub is the repository where Jenkins retrieves the files to build, test, and deploy the URL Shortener application.  For this deployment, after the initial commit, the GitHub was cloned into the local report, where it was branched and the Jenkinsfile was edited and the changes were commited.  The second branch was then merged to the main branched, then pushed to the GitHub repository.

## Step #3 Setup EC2 Instance and Install Jenkins

**Jenkins**
Jenkins is used to automate the Build, Test, and Deploy the URL Shortener Application.  To use Jenkins in a new EC2, all the proper installs to use Jenkins and to read the programming language that the application is written in need to be installed. In this case, they are Jenkins, Java, Python, and Jenkins additional plugin "Pipeline Keep Running Step".

**Instructions to Setup a New EC2 Instance**

[Create EC2 Instance](https://github.com/LamAnnieV/Create_EC2_Instance/blob/main/Create_EC2_Instance.md)

**Shell Scripts for Install(s) in the Instance**

[Install "python3.10-venv", "python3-pip" and "nginx"](https://github.com/LamAnnieV/Instance_Installs/blob/main/02_other_installs.sh)

**Instructions for Jenkins Install, Install Plugin(s), and Create Build**

[Install Jenkins](https://github.com/LamAnnieV/Instance_Installs/blob/main/01_jenkins_installs.sh)

[Install "Pipeline Keep Running Step" Plugin](https://github.com/LamAnnieV/Jenkins/blob/main/Install_Pipeline_Keep_Running_Step.md)

[Create Jenkins Multibranch Pipeline Build](https://github.com/LamAnnieV/Jenkins/blob/main/Jenkins_Multibranch_Pipeline_Build.md)

[Install/Configure CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance-fleet.html)

[How to create a CloudWatch alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ConsoleAlarms.html)

### Jenkins Build #1:  In Jenkins create a build "Deployment_03" for the URL Shortener application from GitHub Repository https://github.com/LamAnnieV/Deployment_03 and run the build.  This build consists of two stages:  The build Stage and the Test Stage

**Result:  Build and Test was successful, see run #1**

![Jenkins Successful Build: See Run #1](Images/Jenkins_Success.png)

## Step #4 Install AWS ELastic Beanstalk CLI

In Deployment #2, the URL Shortener was manually deployed via AWS Elastic Beanstalk.  In this deployment, AWS CLI and AWS EB CLI were installed to automate the deployment of the URL Shortener.

**AWS EB CLI Install**

[Generate AWS CLI Credentials](https://github.com/LamAnnieV/Setup_AWS/blob/main/Generate_AWS_CLI_Credentials.md)

[Script to Install CLI](https://github.com/LamAnnieV/Instance_Installs/blob/ec378d89c22c95a909cb1283516e633ab6c9b153/03_CLI_installs.sh)

[Script to Install AWS EB CLI Part I](https://github.com/LamAnnieV/Instance_Installs/blob/main/04A_AWS_EB_CLI_install.sh)

[Script to Install AWS EB CLI Part II](https://github.com/LamAnnieV/Instance_Installs/blob/main/04B_AWS_EB_CLI_install.sh)

[Script to Install AWS EB CLI Part II](https://github.com/LamAnnieV/Instance_Installs/blob/main/04C_AWS_EB_CLI_install.sh)

**After installing the AWS EB CLI the application URL was displayed:**

![Application URL](Images/URL_Website.png)

### Jenkins Build #2:  The Jenkins file was edited to include a "Deploy" stage, which deploys the URL shortener.

**Result:  Build and Test was successful, see run #3**

![Jenkins Successful Build: See Run #1](Images/Jenkins_Success.png)

**Check EBS Health**

![Check EBS Health](Images/EBS_Health.png)

**Launch URL Shortener Website**

![URL Shortener](Images/URL_Shortener.png)

## Step #4 Configure GitHub Webhook

When there is a commit in GitHub, the "Run Build" still needs to be manually ran.  To automate this process, a GitHub Webhook was configured.  When there is a commit in the GitHub Repository, The webhook pushes the files to Jenkins and automatically runs the Build.

**To configure GitHub Webhook**

[Configure GitHub Webhook](https://github.com/LamAnnieV/GitHub/blob/main/Configure_GitHub_Webhook.md)

**Webhook Validated**

![Webhook Validated](Images/webhook_response.png)

### Jenkins Build #3: Webhook was configured in GitHub and the base.html file was edited to test the Webhook, which should auto "Run Build" in Jenkins

In Jenkins Build #1 and #2, when there is a commit in GitHub, the "Run Build" still needs to be manually ran.  To automate this process, a GitHub Webhook was configured.  When there is a commit in the GitHub Repository, the webhook pushes the files to Jenkins and automatically runs the Build.

To test the webhook, the file https://github.com/LamAnnieV/Deployment_03/blob/main/templates/base.html was edited to change "URL Shortener" to "URL Shrinker"

### Result:  Build and Test was successful, see run #4

![Jenkins Successful Build](Images/Jenkins_Webhook.png)

### Launch URL Shortener Website

![URL Shortener](Images/Tested_Webhook.png)

## Issue(s): 

- There was an issue installing the ADW EB CLI.  In this case in order to successfully install ADW EB CLI, Python3-pip needed to be installed beforehand.
            
## Area(s) for Optimization:

- Further automate the installs to minimize manual input/entries
- Install and configure Monitoring for Systems and Application Files
  
