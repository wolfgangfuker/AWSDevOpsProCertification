# AWSDevOpsProCertification
Learning Summary for AWS  Certified DevOps Engineer Professional Certification

This isn't an extensive list but covers more topics where I needed to dive deep.
A full list of all services can be found in the exam guide: https://d1.awsstatic.com/training-and-certification/docs-devops-pro/AWS-Certified-DevOps-Engineer-Professional_Exam-Guide.pdf

- CodeBuild
	- Test Reports
		- Can be created inside
		- Need to add a report group name in the buildspec file of a build project.
		- Report expires 30 days after it was created.
		- Want to keep longer as 30 days -> export test results
- CodePipeline
	- Add a cross Region action to a pipeline: https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-create-cross-region.html#actions-create-cross-region-cfn
	- Lambda function as an action in CodePipeline.
		- Can verify and push new version of products into AWS Service Catalog.
- CodeDeploy
	- Service Catalog Deploy Action does NOT exist.
	- Codebuild does not natively integrate with Lambda.
	- In-place Deployment: Rolling update across EC2 instances.
		- Specify number of instances to be taken offline at a time for updates.
	- Blue/Green Deployment - latest application revision is installed on replacement instances. Traffic is rerouted to these instances when you choose either immediately or as soon as you are done testing. For both Codedeploy tracks application health according to rules you configure.
	- Stop and roll back
	- Centralized control
	- Easy to adopt
	- Concurrent deployments
	- Deployment types
		- In-place
			- App on each instance stopped
			- Latest application revision installed
			- New version started and validated
			- Load balance can deregister each instance during deployment and then restore service
			- Only available for EC2/On-Premise compute.
	- The CodeDeployDefault.HalfAtATime deployment configuration ensures that 50% of the instances are available during deployment. By setting Auto Scaling to ELB, you ensure that an instance will be replaced if the Application Load Balancing health check fails.
- ElasticBeanstalk
	- .ebextensions
		- https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions.html
		- YAML or JSON formatted documents
		- always need to have file extension .config
		- **Uniqueness** – Use each key(=sections) only once in each configuration file.
		- sections
			- option_settings
				- Configuration options - let you configure your Elastic Beanstalk environment
			- resources section
				- further customize / add additional AWS resources
			- packages
				- Use the packages key to download and install prepackaged applications and components
			- sources
				- use the sources key to download an archive file from a public URL and unpack it in a target directory on the EC2
			- files
				- use to create files on EC2 instances
				- can either be inline or from a URL
				- For S3 probide instance profile in Authorization
			- users
				- to create Linux/Unix users
			- groups
				- to create Linux/UNIX groups and assigning group IDs
			- commands
				- To execute commands
				- Non-container commands and other customization operations are performed prior to the application source code being extracted.
			- container_commands
				- To execute commands that affect your application source code.
				- Container commands run after the application and web server have been set up and the application version archive has been extracted, but before the application version is deployed.
				- Container commands are run from the staging directory, where your source code is extracted prior to being deployed to the application server. Any changes you make to your source code in the staging directory with a container command will be included when the source is deployed to its final location.
			- services
				- For services which should be started or stopped when instance is launched
	- CloudWatch
		- https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html
		- https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html
			- With CloudWatch Logs Insights, you can interactively search and analyze your log data in Amazon CloudWatch Logs.
			- CloudWatch Logs Insights queries cannot create metrics.
			- Use a CloudWatch Logs metric filter to automatically extract increment metrics based.
		- use a CloudWatch Logs metric filter to create a metric for logs that match a pattern or to detect specific events. Events can include root user logins. Then, you can create a CloudWatch alarm from that metric to invoke an alarm.
		- use subscription filters for real-time log streaming to destinations such as Lambda. CloudWatch Logs subscription filters cannot send events directly to Amazon SNS.
		- CloudWatch Logs Insights provides querying capabilities for CloudWatch Logs. You can create a dashboard that uses CloudWatch Logs Insights to create a custom view that shows root user activity across all accounts.
		- Real time processing of log data with subscriptions: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Subscriptions.html
		- https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-network-performance.html
	- CloudTrail
		-  You can use CloudTrail to log all API activity in an organization, including root user logins. You can configure an organization trail to ensure that root login activities are captured across all member accounts. Then, you can forward the logs to CloudWatch Logs for analysis.
	- Kinesis Data Stream
		- Enhanced Fan-Out
			- https://docs.aws.amazon.com/streams/latest/dev/building-enhanced-consumers-api.html
			- A data stream consumer with enhanced fan-out minimizes latency and maximizes read throughput when there are multiple consumers. In this scenario, enhanced fan-out will not reduce latency. The throughput will still be 2 MB per second because there is only one consumer.
		- Batch Size
			- Batch size is the maximum number of records in each batch that Lambda pulls from the stream or queue and sends to the function. Lambda passes all the records in the batch to the function in a single call. The payload quota for synchronous invocation is 6 MB. Increasing the batch size will increase the batch windows and then increase the latency.
		- Shards
			- Increasing the number of shards of a data stream increases the data capacity of the stream. With more shards, there are more batches being processed at once.
			- [How Lambda Processes Records from Amazon Kinesis Data Streams](https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis.html).
			- [Reshard a Stream](https://docs.aws.amazon.com/streams/latest/dev/kinesis-using-sdk-java-resharding.html).
	- Lambda
		- The ParallelizationFactor setting helps scale up the processing throughput when data volume is volatile and the IteratorAge is high.
		- [How Lambda Processes Records from Amazon Kinesis Data Streams](https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis.html).
	- Service Catalog
		- The use of template constraints at the AWS Service Catalog level minimizes overhead, provides constrained access to the templates for beginners, and provides unconstrained access for experts. Beginners will deploy by using the AWS Service Catalog products with template constraints. Experts will deploy directly by using CloudFormation with no constraints.
		- [AWS Service Catalog Template Constraints](https://docs.aws.amazon.com/servicecatalog/latest/adminguide/catalogs_constraints_template-constraints.html).
	- Trusted Advisor
		- You can use EventBridge to create a rule that has Trusted Advisor as the event source. The rule can target a Lambda function to delete the IAM access key and can target an SNS topic to provide the notification.
		- Trusted Advisor draws upon best practices learned from serving hundreds of thousands of AWS customers. Trusted Advisor inspects your AWS environment, and then makes recommendations when opportunities exist to save money, improve system availability and performance, or help close security gaps.
		- If you have a Basic or Developer Support plan, you can use the Trusted Advisor console to access all checks in the Service Limits category and [five checks](https://docs.aws.amazon.com/awssupport/latest/user/trusted-advisor-check-reference.html) in the Security category.
		- If you have a Business, Enterprise On-Ramp, or Enterprise Support plan, you can use the Trusted Advisor console and the [AWS Trusted Advisor API](https://docs.aws.amazon.com/awssupport/latest/user/get-started-with-aws-trusted-advisor-api.html) to access all Trusted Advisor checks. You also can use Amazon CloudWatch Events to monitor the status of Trusted Advisor checks.
	- Guard Duty
		- Amazon GuardDuty is a threat detection service that continuously monitors, analyzes, and processes AWS data sources and logs in your AWS environment. GuardDuty uses threat intelligence feeds, such as lists of malicious IP addresses and domains, file hashes, and machine learning (ML) models to identify suspicious and potentially malicious activity in your AWS environment. The following list provides an overview of potential threat scenarios that GuardDuty can help you detect:
		- Threat list in GuardDuty
	- WAF
		- https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-awswaf.html
		- AWS Config is requried for the use of Firewall Manager security policies
		- **Trusted IP list**
			- Trusted IP lists consist of IP addresses that you have trusted for secure communication with your AWS infrastructure and applications. GuardDuty does not generate VPC flow log or CloudTrail findings for IP addresses on trusted IP lists.
		- **Threat IP list**
			- Threat lists consist of known malicious IP addresses. This list can be supplied by third-party threat intelligence or created specifically for your organization.
		- You can use Firewall Manager to apply AWS WAF rule groups across multiple AWS accounts. Firewall Manager policies for AWS WAF can target entire organizations, specific OUs, or a list of AWS accounts. Firewall Manager policies for AWS WAF can also target ALBs.
	- CodeArtifact
		- https://docs.aws.amazon.com/codeartifact/latest/ug/domains.html
		- CodeArtifact _domains_ make it easier to manage multiple repositories across an organization. You can use a domain to apply permissions across many repositories owned by different AWS accounts. An asset is stored only once in a domain, even if it's available from multiple repositories.
		- Although you can have multiple domains, we recommend a single production domain that contains all published artifacts so that your development teams can find and share packages. You can use a second preproduction domain to test changes to the production domain configuration.
	- CloudFormation StackSets
		- Every account that you deploy a stack instance to requires a role that has a trust relationship with the administrator account. Otherwise, the deployment will fail.
		- https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-troubleshooting.html
	- Amazon Data Firehose
		- https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html
	- Amazon Athena
		- https://docs.aws.amazon.com/athena/latest/ug/creating-tables.html
	- ECR
		- ECR repository policies control access to repositories. You can define which IAM users or roles have access to a repository policy, but repository policies do not delete older versions of images.
		- ECR lifecycle policies help manage the lifecycle of images that are pushed to ECR repositories. Lifecycle policies can expire images by using rules that are based on age or count.
