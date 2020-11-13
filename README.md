## Build an Application with IPV6 Support
The purpose of this repository is to demo how to build a sample application with IPV6 support.

AWS CloudFormation template (main.yaml) will deploy a Virtual Private Cloud(VPC) with 2 Public and 2 Private subnets within the VPC. Later it will create a Public Facing Application Load Balancer attached to an Auto Scaling Group where it will serve a simple PHP application and echo real-client IP using REMOTE_ADDR header. You will be able to see IPV4 or IPV6 depending from where you access the ALB.

###  Launch the AWS CloudFormation Stack

Click on the **Launch Stack** button below to launch the CloudFormation Stack to build sample app with IPV6 support in the region of your preference, by default this demo will be deployed in us-west-2 (Oregon) region.

[![Launch CFN stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/quickcreate?templateUrl=https%3A%2F%2Faws-sample-app-with-ipv6-support.s3-us-west-2.amazonaws.com%2Fmain.yml&stackName=aws-sample-app-with-ipv6-support)

Provide a stack name eg **aws-sample-app-with-ipv6-support**.

You can launch the same stack using the AWS CLI. Here's an example:

```
aws cloudformation create-stack --stack-name aws-sample-app-with-ipv6-support \
   --template-body file://main.yaml \
   --capabilities CAPABILITY_NAMED_IAM \
   --region us-west-2
```

### How to Access Your Application using IPV4
Once stack creation is completed, it will output the Application Load Balancer DNS Name under "Outputs" tab of your stack. Another way of accessing via CLI:

```
aws cloudformation describe-stacks --stack-name aws-sample-app-with-ipv6-support \
   --query "Stacks[0].Outputs[0].OutputValue" \
   --region us-west-2
```

Open your browser and navigate to DNS Name. It should show something similar to "REMOTE_ADDR: 222.121.222.121". This means that your load balancer is serving IPV4 traffic.

### How to Access Your Application using IPV6
Now, you should see an Amazon EC2 instance with suffix "Test Server", choose the instance and click on Connect. Once the connection is established, write the following command:

```
curl DNS_NAME_OF_YOUR_ALB
```

The output should be something like "REMOTE_ADDR: 2600:1f14:690:4b00:c9c0:6dca:d21d:c8b0" which means that your load balancer is serving IPV6 traffic.

Also you can connect to Amazon EC2 instance with suffix "Web Server" and tail the httpd logs to see both IPV4 and IPV6 access logs.

```
sudo tail -f /var/log/httpd/access_log | grep -v "ELB-HealthChecker"
```

###  Clean up
After completing your demo, delete AWS CloudFormation Stack using AWS Console or AWS CLI:
```
aws cloudformation delete-stack --stack-name aws-sample-app-with-ipv6-support --region us-west-2
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
