# Tips for AWS Certified DevOps Engineer – Professional

* Using the Amazon Kinesis Adapter is the recommended way to consume streams from Amazon DynamoDB.
* You can avoid pushes to the master branch in CodeCommit using an additional policy to include a Deny rule for the ***codecommit:GitPush*** in an IAM policy.
* In ElasticBeanstalk, you can choose from a variety of deployment methods:
  * ***All at once*** – Deploy the new version to all instances simultaneously. All instances in your environment are out of service for a short time while the deployment occurs. This is the method that provides the least amount of time for deployment.
  * ***Rolling*** – Deploy the new version in batches. Each batch is taken out of service during the deployment phase, reducing your environment's capacity by the number of instances in a batch.
  *  ***Rolling with additional batch*** – Deploy the new version in batches, but first launch a new batch of instances to ensure full capacity during the deployment process.
  *  ***Immutable*** – Deploy the new version to a fresh group of instances by performing an immutable update.
  *  ***Blue/Green*** - Deploy the new version to a separate environment, and then swap CNAMEs of the two environments to redirect traffic to the new version instantly.
* With ***AWSCodeCommitPowerUser*** policy a member can have read/write access on an repository but can not delete CodeCommit repositories.
* You can define Lambda functions in the ***AfterAllowTestTraffic*** lifecycle hook of the ***AppSpec.yaml*** file. Configure Lambda functions to validate the deployment using the test traffic and rollback if the tests fail.
* Host the application on an S3 bucket configured for website hosting. Set up a CloudFront web distribution and set the S3 bucket as the origin with the origin response event set to trigger a Lambda@Edge function. You can add the required security headers, such as ***X-Content-Type-Options***, ***X-Frame-Options*** and ***X-XSS-Protection***, in the HTTP response using the Lambda function.
* You can update the service on ECS Fargate and select “Force new deployment” so that the cluster will be re-deployed with a new image version.
* With Lambda and Lambda@Edge you can have a consistent and fast experience for all your users around the world.
* To implement an end-to-end HTTPS connection from the origin to the CloudFront viewers you can Generate an SSL certificate that is signed by a trusted third-party certificate authority in the ALB. Import the certificate into AWS Certificate Manager. Set the ***Viewer Protocol Policy*** to HTTPS Only in CloudFront and then use an SSL/TLS certificate from a 3rd party certificate authority which was imported to either AWS Certificate Manager or the IAM certificate store.
* Use the Amazon S3 Event Notification to automatically invoke a Lambda function when a file is uploaded/edited/deleted.
* To MINIMIZE downtime of the portal in the event that the primary database fails launch a read replica of the primary database to the second region. Set up Amazon RDS Event Notification to publish status updates to an SNS topic. Create a Lambda function subscribed to the topic to monitor database health. Configure the Lambda function to promote the read replica as the primary in the event of a failure.
* Use Amazon Macie to monitor and detect usage patterns on your S3 data logs.
* Deploy your app using Traffic shifting with AWS Lambda aliases to create canary deployments.
* You can create an IAM group for team members and another IAM group for the team leader, both with AWSCodeCommitPowerUser policy attached. Attach another IAM policy to the team members' group that denies Push, Delete, and Merge APIs of CodeCommit on the master branch.
* By default, CodeDeploy removes all files on the deployment location and the auto rollback will deploy the old revision files cleanly. You can choose “Retain the content” option for future deployments so that only the files included in the old app revision will be deployed and the existing contents will be retained.
* Automatically detect SSH brute force or malware attacks by enabling Amazon GuardDuty in every account.
* Produce a Docker image that runs the X-Ray daemon. Upload the image to a Docker image repository, and then deploy it to your Amazon ECS cluster. Configure the network mode settings and port mappings in your task definition file to allow traffic on UDP port 2000. With that you instrument the application to detect where high latencies are occurring and to determine the specific services and paths impacting application performance.
* Add ***--force-new-deployment*** option to the AWS CLI command so that ECS re-deploys your cluster pulling a new image.
* Set up a special CloudFront user called an origin access identity (OAI). Grant the origin access identity permission to read the objects in your bucket. For each user, revoke the existing permission to access Amazon S3 URLs to download the objects. this configuration prevents anyone from bypassing the CloudFront distribution and disable direct access from Amazon S3 URLs.
* Diagnose and troubleshoot problems on your Amazon EC2 Linux and Windows Server instances using the EC2Rescue tool. Automatically run the tool by using the Systems Manager Automation and the ***AWSSupport-ExecuteEC2Rescue*** document.
* CodeDeploy can reach timeout while waiting for a long-running script to finish. 
* Your CodeCommit repository and AWS SNS topic must be on the same region for the triggers to work.
* With Amazon GuardDuty you can detect compromised EC2 instances as well as detect malicious activity within your AWS environment.
* Create a CloudWatch Events rule that is scheduled to run at midnight. Set the target to directly call the EC2 CreateSnapshot API to create a snapshot of EBS volumes. You do not need to use Lambda.
* Use the EB CLI ***eb create --cfg savedconfig*** to apply a saved configuration in an environment.
* Access logging is an optional feature of Elastic Load Balancing that is disabled by default.
* To clone the repository properly using the HTTPS connection attach the IAM policy AWSCodeCommitPowerUser to the IAM user through the AWS IAM console and in the IAM console, generate HTTPS Git credentials for AWS CodeCommit and download credentials to a .CSV file.
* For the repository event of deleting branches, create a trigger in CodeCommit to an Amazon SNS topic to provide notifications to users. You do not need CloudWatch.
* Use a ***Container command*** within an Elastic Beanstalk configuration file to execute the script, ensuring that the ***leader only*** flag is set to true. With that you can assure that a script will be executed only once in a group of instances.
* Ensure that the Amazon EBS volumes restored from snapshots have been pre-warmed by reading all the blocks before a test.
* You can use an AWS CloudFormation stack policy to deny updates. 
* You need to create an instance profile and associate it with that specific role to use a IAM Role.
* Configure these CloudWatch alarms to treat missing data points as “ignore” so that “INSUFFICIENT_DATA” does not show up.
* You can configure the Auto Scaling group to send a notification to an SNS topic whenever instances fail to terminate.
* You can modify the EC2 instance type. Select the relevant launch configuration and choose “Actions -> Copy launch configuration”. Modify the instance type accordingly in the new configuration. Select the new launch configuration for the Auto Scaling Group.
* If your Auto Scaling Group scales up too much and stays scaled up even when traffic decreases calculate the bottleneck or constraint on the compute layer, select that as the new metric, and set the CloudWatch metric thresholds to the bounding values that begin to affect response latency.
* Use a placement group for your instances, so the instances are physically near each other in the same Availability Zone.
* Use AWS Directory Service AD Connector. With that you will integrate with your existing identity management system running on Microsoft Active Directory.
* To change instance type in a ASG create a new launch configuration with the new instance type, attach it to the existing autoscaling group and increase the number of desired instances of ASG to launch new instances. When new instances are in healthy status, decrease the number of desired instances to delete instances with the old instance type.
* It takes time for the ELB to register the instances. Hence there is a small time frame before your instances can start receiving traffic.
* While creating Deployment Group In CodeDeploy, users can select the targets via Tags and lists. The targets can be any combination of Amazon EC2 Auto Scaling groups, Amazon EC2 instances or On-premises instances.
* The data in both DynamoDB table (at rest) and DynamoDB Streams is fully encrypted.
* The priorities in Elastic Beanstalk are “configurations in AWS console” > “configurations in .ebextensions” > “default value”.
* The settings in the .ebextensions folder cannot override that in the EB CLI command. 
* To have the least amount of downtime, the ideal way is to create a blue-green deployment environment and then use the Swap URL feature to swap environments for the new deployment and then do the swap back, in case the deployment fails. The AWS Documentation mentions the following on the SWAP URL feature of Elastic Beanstalk.
* In .ebextensions files, within each package manager, package installation order isn't guaranteed.
* Use the Deletion policy of the CloudFormation template to ensure a snapshot is created of the relational database.
* You can use intrinsic functions only in specific parts of a template. Currently, you can use intrinsic functions in resource properties, outputs, metadata attributes, and update policy attributes.
* You can have one single dashboard to report metrics to CloudWatch from different regions.
* You can have one Opswork stack and multiple layers.
* With ***list-stacks*** command in CloudFormation you will see all the cloudformation stacks that have been deleted earlier.
* An AWS Lambda hook is one Lambda function specified with a string on a new line after the name of the lifecycle event. Each hook is executed once per deployment. Here are descriptions of the hooks available for use in your AppSpec file:
  * ***BeforeAllowTraffic*** – Use to run tasks before traffic is shifted to the deployed Lambda function version.
  * ***AfterAllowTraffic*** – Use to run tasks after all traffic is shifted to the deployed Lambda function version.
* If you enable the IAM Capability on the CodePipeline configuration for the Deploy CloudFormation stage action you can solve the ***InsufficientCapabilitiesException*** problem.
* If we encrypt S3 using KMS, we may get throttled at 10000 objects per second.
* Create a file named ***.ebextensions/alb.config*** in your code repository and add an *option_settings*** block for which you will specify the Rules for the key ***aws:elbv2:listener:default***. Push your code and let the CodePipeline run. With that you can redirect HTTP to HTTPS.
* Change the ***runOrder*** of your actions in CodePipeline so that they have the same value and you wll reduce build time.
* AWS Lambda has a max concurrent execution of 1000, while API gateway has a max concurrent execution of 10000. By integrating API Gateway and Step Functions together, you bypass any limit Lambda would have imposed on you.
* The variable ***CODEBUILD_SOURCE_VERSION*** is exposed at runtime directly within CodeBuild and represents the branch name of the code being tested for CodeCommit. 
* Create ECS task definitions that include the ***awslogs*** driver. Set an IAM instance role on the EC2 instance with the necessary permissions to write to CloudWatch log. With that you will ensure all application logs can be stored in CloudWatch logs.
* When creating a new task definition for your ECS service, ensure to add the sha256 hash in the full image name so that ECS pulls the correct image every time.
* Pull the new AMI IDs using an AWS Lambda-backed custom resource in the CloudFormation template. Reference the AMI ID that the custom resource fetched in the launch configuration resource block. This will avoid manually update CloudFormation templates for every new available AMI of an application.
* You can use the console, AWS CLI, or AWS CloudFormation to add cross-region actions in pipelines. If you use the console to create a pipeline or cross-region actions, default artifact buckets are configured by CodePipeline in the Regions where you have actions. 
* Restart the ECS agent with you noticed that the new tasks are occasionally running the old image of the application.