# cloud9-kafka-console
Tools to install to use cloud9 with Kafka

## Setup Environment
In Cloud9 find the terminal and run the commands below from the ~/environment/kafka directory. We want to resize the Cloud9 instances disk before starting so we can download the needed kafka tools. The setup needs to be run as sudo mostly cause I'm lazy, but downloads the required java version, aws cli, and kafka client tools. Finall, you need to source the `set-env.sh` to set the required environment variables. You can look at the scripts to see what each does.

You will need a CloudFormation stack that has an `MSKClusterArn` output value to setup the environment.

```bash
chmod +x *.sh
./resize.sh 50 
sudo ./setup.sh
. ./set-env.sh <StackName>
```

To find the kafka bootstrap brokers run the command `echo $brokers` from the terminal.

## Using Kafka Tools

The MSK cluster has no topics created when first created. We want to create the topic in the MSK cluster by running the commands below in the terminal. The `setup.sh` script downloaded the kafka tools in the `/home/ec2-user/kafka` directory. You will ned to `cd` to there and run the `bin/kafka-topics.sh` script to create the `ExampleTopic`.

```bash
export region=$(curl http://169.254.169.254/latest/meta-data/placement/region)
cd /home/ec2-user/kafka
bin/kafka-topics.sh --create --zookeeper $zoo --replication-factor 3 --partitions 3 --topic <Topic>
```