here we are going to push application logs to cloudwatch logs with the help of Cloudwatch agent



we need to attach a role to ec2 instances

role name: CloudwatchAgentRole
needed Policies: CloudWatchAgentServerPolicy, CloudWatchAgentAdminPolicy and 

AmazonEC2RoleforSSM this role for accessing SSM store from Ec2.
attach the role to the Ec2.

first we can install cloudwatch agent
# install the agent
sudo wget https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm 

sudo rpm -U ./amazon-cloudwatch-agent.rpm

now we can run the wizard to create the config.json file.
# run the wizard
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard


Do you want to monitor any log files?
enter 1.

enter log file path:

at this point, enter the log destination which we need to push to cloudwatchlogs 
for example, in case of apache server the logs will be in /var/log/httpd/access_log (access logs) and error logs are /var/log/httpd/error_log
press enter


Do you want to store the config in the SSM parameter store?
enter yes to save this in ssm parameter stop


to start the agent:

there is two options 

1. is from ssm parameter store
2. is from local storage 

# for taking from ssm:
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:AmazonCloudWatch-linux -s


# for taking from local storage:
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s



thats it. check your cloudwatch logs to verify
