---
Title: AWS Integration
Date: 2016-05-12T16:39:45+09:00
URL: https://mackerel.io/docs/entry/integrations/aws
EditURL: https://blog.hatena.ne.jp/mackerelio/mackerelio-docs.hatenablog.mackerel.io/atom/entry/6653812171395777794
---

Using AWS integration, you can manage AWS cloud products as a host of Mackerel and monitor its metrics. This function is only offered in the Trial plan and Standard plan.

Each AWS cloud product will be registered as one host in Mackerel and therefore be counted as a billable host.
The types of hosts consist of standard hosts for EC2 and micro hosts for other products.
Additionally, the API of AWS will be called every 5 minutes for each targeted metric to be obtained. Please take note, for this reason, an [Amazon CloudWatch API usage fee](https://aws.amazon.com/jp/cloudwatch/pricing/) may occur.

Currently, the following AWS cloud products are supported. For information on obtaining metrics, please refer to each individual document.

[EC2](https://mackerel.io/docs/entry/integrations/aws/ec2)・[ELB (CLB)](https://mackerel.io/docs/entry/integrations/aws/elb)・[ALB](https://mackerel.io/docs/entry/integrations/aws/alb)・[NLB](https://mackerel.io/docs/entry/integrations/aws/nlb)・[RDS](https://mackerel.io/docs/entry/integrations/aws/rds)・[ElastiCache](https://mackerel.io/docs/entry/integrations/aws/elasticache)・[Redshift](https://mackerel.io/docs/entry/integrations/aws/redshift)・[Lambda](https://mackerel.io/docs/entry/integrations/aws/lambda)・[SQS](https://mackerel.io/docs/entry/integrations/aws/sqs)・[DynamoDB](https://mackerel.io/docs/entry/integrations/aws/dynamodb)・[CloudFront](https://mackerel.io/docs/entry/integrations/aws/cloudfront)・[API Gateway](https://mackerel.io/docs/entry/integrations/aws/apigateway)・[Kinesis](https://mackerel.io/docs/entry/integrations/aws/kinesis)・[S3](https://mackerel.io/docs/entry/integrations/aws/s3)・[ES](https://mackerel.io/docs/entry/integrations/aws/es)・[ECS](https://mackerel.io/docs/entry/integrations/aws/ecs)・[SES](https://mackerel.io/ja/docs/entry/integrations/aws/ses)・[Step Functions](https://mackerel.io/docs/entry/integrations/aws/states)・[EFS](https://mackerel.io/docs/entry/integrations/aws/efs)・[Kinesis Data Firehose](https://mackerel.io/docs/entry/integrations/aws/firehose)・[Batch](https://mackerel.io/docs/entry/integrations/aws/batch)


<h2 id="setting">Integration method</h2>
There are two ways to integrate AWS Integration.

- Configure the IAM role to allow access only from the AWS account of the Mackerel system and authenticate with AssumeRole
- Configure the Access Key ID and Secret Access Key

From a security standpoint, we strongly recommend configuring with the IAM role.

<h3>How to configure an IAM role</h3>
<h4>1. Creating a role with the IAM Management Console</h4>
Create a new role with the <a href="https://console.aws.amazon.com/iam" target="_blank">IAM Management Console</a>.
Select `Another AWS account` from the role types.

![](https://cdn-ak.f.st-hatena.com/images/fotolife/m/mackerelio/20190528/20190528124827.png)

Click the 'Create' button in the [Mackerel AWS Integration Settings page](https://mackerel.io/my?tab=awsIntegration) to get an External ID. Enter `217452466226` as the authorized Account ID. Select the `Require external ID` option and specify the External ID obtained from Mackerel's configuration page. The Mackerel system uses the account to access the user’s role. With this configuration, only the Mackerel account can access the created role. Create the role without checking `Require MFA`.

Grant the policies listed below for the role. Be careful not to grant FullAccess permission. Also, the maximum number of policies that can be attached to one IAM role is limited to 10. This is a specification of AWS. If necessary, please apply to AWS for an upper limit extension.

- `AmazonEC2ReadOnlyAccess`
- `AmazonElastiCacheReadOnlyAccess`
- `AmazonRDSReadOnlyAccess`
- `AmazonRedshiftReadOnlyAccess`
- `AWSLambdaReadOnlyAccess`
- `AmazonSQSReadOnlyAccess`
- `AmazonDynamoDBReadOnlyAccess`
- `CloudFrontReadOnlyAccess`
- `apigateway:GET / apigateway:OPTIONS`
    - Specify resource policy like `arn:aws:apigateway:ap-northeast-1::/*` . You can not limit integration target by resource policy.
- `AmazonKinesisReadOnlyAccess`
- `AmazonS3ReadOnlyAccess`
- `AmazonESReadOnlyAccess`
- `ecs:Describe* / ecs:List*`
- `AmazonSESReadOnlyAccess / ses:Describe*`
- `AWSStepFunctionsReadOnlyAccess`
- `AmazonElasticFileSystemReadOnlyAccess`
- `AmazonKinesisFirehoseReadOnlyAccess`
- `batch:Describe* / batch:List*
- `CloudWatchReadOnlyAccess`（When only configuring CloudFront, API Gateway, Kinesis, S3, ES, ECS, SES, Step Functions, EFS, Firehose or Batch）

Additionally, in AWS Integration you can filter using tags as is mentioned further down, but if you filter using tags with ElastiCache or SQS, additional policies need to be added.
For more details, refer to <a href="#tag">Filter by tag</a>.

![](https://cdn-ak.f.st-hatena.com/images/fotolife/a/andyyk/20171025/20171025153435.png)

Fillin the name and create a role.
We recommend assigning an easy-to-understand name like `MackerelAWSIntegrationRole`  for use in Mackerel’s AWS integration.

<h4>2. Register an ARN role in Mackerel</h4>
Register an ARN role from the same Mackerel screen where we just acquired the External ID.

<h4>3. Confirm the host</h4>
After a short while, your AWS cloud product will be registered as a host in Mackerel and begin posting metrics. By creating monitoring rules, you can also be notified of alerts. For more information, see [Setting up monitoring and alerts](https://mackerel.io/docs/entry/howto/alerts).

<h3>Configure the Access Key ID and Secret Access Key</h3>
From a security standpoint, the following method is not recommended.
<h4>1. Creating a user with the IAM Management Console</h4>
Create a new user with the <a href="https://console.aws.amazon.com/iam" target="_blank">IAM Management Console</a>. We recommend assigning an easy-to-understand name like `MackerelAWSIntegrationUser` for use in Mackerel’s AWS integration.

![](https://cdn-ak.f.st-hatena.com/images/fotolife/a/andyyk/20160512/20160512162316.png)

<h4>2. Registering the Access Key in Mackerel</h4>
Register the Access Key ID and Secret Access Key (displayed on the screen when creating the account) [in Mackerel](https://mackerel.io/my?tab=awsIntegration). Be careful not to mistake the organization to be registered.

![](https://cdn-ak.f.st-hatena.com/images/fotolife/a/andyyk/20160512/20160512162642.png)

<h4>3. Granting policies</h4>
Grant the policies listed below for the newly created user. Be careful not to grant FullAccess permission. Also, the maximum number of policies that can be attached to one IAM user is limited to 10. This is a specification of AWS. If necessary, please apply to AWS for an upper limit extension.

- `AmazonEC2ReadOnlyAccess`
- `AmazonElastiCacheReadOnlyAccess`
- `AmazonRDSReadOnlyAccess`
- `AmazonRedshiftReadOnlyAccess`
- `AWSLambdaReadOnlyAccess`
- `AmazonSQSReadOnlyAccess`
- `AmazonDynamoDBReadOnlyAccess`
- `CloudFrontReadOnlyAccess`
- `apigateway:GET / apigateway:OPTIONS`
    - Specify resource policy like `arn:aws:apigateway:ap-northeast-1::/*` . You can not limit integration target by resource policy.
- `AmazonKinesisReadOnlyAccess`
- `AmazonS3ReadOnlyAccess`
- `AmazonESReadOnlyAccess`
- `ecs:Describe* / ecs:List*`
- `AmazonSESReadOnlyAccess / ses:Describe*`
- `AWSStepFunctionsReadOnlyAccess`
- `AmazonElasticFileSystemReadOnlyAccess`
- `AmazonKinesisFirehoseReadOnlyAccess`
- `batch:Describe* / batch:List*
- `CloudWatchReadOnlyAccess`（When only configuring CloudFront, API Gateway, Kinesis, S3, ES, ECS, SES, Step Functions, EFS, Firehose or Batch）

Additionally, in AWS Integration you can filter using tags as is mentioned further down, but if you filter using tags with ElastiCache or SQS, additional policies need to be added.

For more details, refer to <a href="#tag">Filter by tag</a>.

![](https://cdn-ak.f.st-hatena.com/images/fotolife/a/andyyk/20171025/20171025153435.png)

<h4>4. Verify the host</h4>
After a short while, your AWS cloud product will be registered as a host in Mackerel and begin posting metrics. By creating monitoring rules, you can also be notified of alerts.
For more information, see [Setting up monitoring and alerts](https://mackerel.io/docs/entry/howto/alerts).

<h2 id="tag">Filter by tag</h2>
AWS cloud products to be registered as hosts and retrieve metrics can be filtered based on the tags appended by AWS.

<h3>1. Assign permissions for tag retrieval</h3>
In order to filter using an AWS tag, authority for the following actions is required in addition to the policies that were assigned for configuring AWS integration.

- `elasticache:ListTagsForResource`
- `sqs:ListQueueTags`
- `states:ListTagsForResource`

Additionally, authority for the following action is needed to configure using the Access Key and Secret Access Key. It is not needed for configuration via the IAM role.

- `iam:GetUser` for linked users

To add policies, use the Inline Policies process.

![](https://cdn-ak.f.st-hatena.com/images/fotolife/m/mackerelio/20160616/20160616150058.png)
![](https://cdn-ak.f.st-hatena.com/images/fotolife/m/mackerelio/20160616/20160616150059.png)

<h3>2. Tag filtering settings</h3>
You can specify tags on the Mackerel settings screen. Confirm the number of linked hosts and save your changes.

By specifying the tag as `service:foo, service:bar`, instances tagged with a key of `service` and value of `foo` or a key of `service` and value of `bar` will be targeted. If the key or value contains characters such as a colon `:` or comma `,`, enclose it with quotation marks (`"` or `'`). For example, if the key is `service:role` and the value is `foo,bar`, specify it as `"service:role":"foo,bar"`.

![](https://cdn-ak.f.st-hatena.com/images/fotolife/m/mackerelio/20160622/20160622115520.png)

<h2 id="faq">FAQ</h2>

### Regarding the access key’s authority check and CreateInternetGateway

In order to check whether or not the access key registered by the user has an unnecessarily strong authority, AWS Integration periodically calls the CreateInternetGateway API in dry-run. Metrics will not be obtained or posted if the access key has an authority more than necessary so be careful. The reason why checks still periodically occur after registration is because there is a possibility that policies will get added to the access key, resulting in a key with stronger authority.

### Regarding retiring hosts linked with AWS Integration

By configuring the integration above, AWS cloud products complying with the target service and tag conditions are automatically integrated with Mackerel and registered as a host. On the other hand, hosts in Mackerel will not be deleted (retired) simply by deleting instances etc. in AWS. In order to remove hosts integrated with AWS from Mackerel's managed targets, you need to retire them separately.

[https://mackerel.io/docs/entry/howto/host-retirement:embed:cite]

Even if a host is not retired, host information will remain and hosts without metric posts will not be subject to billing.

<h3 id="plugin-custom-identifier">Regarding the aggregation of monitoring content with plugins in integrated hosts</h3>

`custom_identifier` can be specified in the settings of mackerel-agent's custom metrics and check monitoring plugins. `custom_identifier` is a mechanism that allows the user to assign a distinct identifier to a host. Using this, it is possible to aggregate metrics and check monitors that have been posted from mackerel-agent installed on another machine as a part of the AWS integration host. `custom_identifier` is specified in the configuration of compatible plugins.

For example, the endpoint in the case of Amazon RDS, and the DNS Name in the case of ELB, will be the `custom_identifier`string.

#### Example uses
Here are two example uses. In either case, the agent needs to be restarted after adding to the mackerel-agent configuration file.

The first example is of MySQL monitoring using the [mackerel-plugin-mysql](https://github.com/mackerelio/mackerel-agent-plugins/tree/master/mackerel-plugin-mysql) plugin for Amazon RDS. Metrics retrieved by the plugin can be aggregated as custom metrics for the RDS host by adding the plugin configuration which includes the `custom_identifier` as shown below to the mackerel-agent.conf.

```
[plugin.metrics.mysql]
command = ["mackerel-plugin-mysql", "-host", "<RDS endpoint>", "-username", "user", "-password", "pass"]
custom_identifier = "<RDS endpoint>"
```

The second example is of Elasticsearch monitoring using the Amazon Elasticsearch Service and the [check-elasticsearch](https://github.com/mackerelio/go-check-plugins/tree/master/check-elasticsearch) plugin. Elasticsearch Service Cluster Health Checks can be aggregated as check monitoring for ElasticsearchService hosts by adding the plugin configuration which includes the `custom_identifier` as shown below to the mackerel-agent.conf.

```
[plugin.checks.elasticsearch]
command = ["check-elasticsearch", "-s", "https", "-H", "<Elasticsearch Service endpoint>", "-p", "443"]
custom_identifier = "<Elasticsearch Service domain ARN>"
```
