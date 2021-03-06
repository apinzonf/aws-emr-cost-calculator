## EMR Cost Calculator

This project is a fork of [https://github.com/marko-bast/aws-emr-cost-calculator].

Original features:
- Calculates exact costs of an EMR cluster (EMR + EC2 costs)
- Multiple EMR clusters cost calculation for a given period
- Spot prices and all other prices are exact and retrieved every time from AWS Pricing API
- If a cluster is still running, costs incurred up to current time are displayed

In addition to the original project:
- calculate cost for EMR Instance Fleets
- compute cost of EBS

### Why the need for this script

Given that Amazon doesn't provide a straightforward solution to calculate the cost of an EMR workflow, this module aims to calculate the cost of an EMR workflow given a period of days, or the cost of a single cluster given the cluster id. The simple way to do that would be to use the information given by the JobFLow method of the boto.emr module. However, this method doesn't return any information about the Task nodes of a cluster, and whether or not spot instances were used. This cost calculator takes care of both. OnDemand instance prices are retrieved using the AWS pricing API. In case spot instances were used, the price is retrieved using the AWS EC2 API.

EBS cost is fixed to 0.1 $ for month.

### Install

To install or upgrade the package it's best to use pip:
`pip install -U aws-emr-cost-calculator2`

### How it works

This module is using [docopt](http://docopt.org/) to parse command line arguments.

It currently supports two operations:

1. Get the cost of an EMR cluster given the cluster id
  * `aws-emr-cost-calculator2 cluster --cluster_id=<j-xxxxxxxxxxxx>`

Authentication to AWS API is done using credentials of AWS CLI which are configured by executing
`aws configure`

For example (if you want to use a different profile):
  * `aws-emr-cost-calculator2 cluster --cluster_id=<j-xxxxxxxxxxxx> --profile=<your profile>`

Remember to include your default region in the default section of your ./aws/config file.

### Example run
```
$ aws-emr-cost-calculator cluster --cluster_id=j-K3C155R34111 --profile=myorg
CORE.EC2    :   0.40
CORE.EMR    :   0.11
CORE.EBS    :   0.01
MASTER.EC2  :   0.40
MASTER.EBS  :   0.01
MASTER.EMR  :   0.11
TASK.EC2    :   2.80
TASK.EMR    :   3.12
TOTAL       :   6.95
```

### How release a new module

To release a new module you have to use:
- pypi
- twine

For example, to deploy the 0.1.1 version:
```
python setup.py sdist
twine upload dist/*0.1.1* -r pypi
```

### License

Distributed under the MIT license. See `LICENSE` for more information.
