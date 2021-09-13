# Tips for AWS Certified DevOps Engineer – Professional

* Using the Amazon Kinesis Adapter is the recommended way to consume streams from Amazon DynamoDB.
* You can avoid pushes to the master branch in CodeCommit using an additional policy to include a Deny rule for the ***codecommit:GitPush*** in an IAM policy.
* In ElasticBeanstalk, you can choose from a variety of deployment methods:
  * ***All at once*** – Deploy the new version to all instances simultaneously. All instances in your environment are out of service for a short time while the deployment occurs. This is the method that provides the least amount of time for deployment.
  * ***Rolling*** – Deploy the new version in batches. Each batch is taken out of service during the deployment phase, reducing your environment's capacity by the number of instances in a batch.
  *  ***Rolling with additional batch*** – Deploy the new version in batches, but first launch a new batch of instances to ensure full capacity during the deployment process.
  *  ***Immutable*** – Deploy the new version to a fresh group of instances by performing an immutable update.
  *  ***Blue/Green*** - Deploy the new version to a separate environment, and then swap CNAMEs of the two environments to redirect traffic to the new version instantly.
