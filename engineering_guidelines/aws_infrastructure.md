---
breadcrumb: Engineering Principles
---
# AWS Infrastructure

## General
* We host all of our systems in AWS
* We depoly our stacks either in Ireland or Frankfurt
* For each team or larger project we set up separate AWS accounts. Usally a DEV account (for development, ointegration os stage usage) and a PROD account for the productive environment
	* You will get PIA roles for these accounts to grant people access to them
* We do have an Enterprise Discount Program (EDP) agreement as well as some private pricing agreements with Amazon in place. This gives us:
	* Enterprise support for all accounts. So don't hesitate to create support tickets!
	* Special pricings on S3 storage in Frankfurt. So if you require large amounts of S3 storgae, be sure to locate them in Frankfurt!
	* Special pricings on CloudFront. So use CloudFront in front of all your services - it is much cheaper than e.g. exposinf a S3 bucket directly via http.
	* Some generic discount on the total costs

## Best practices 	

* Keep an eye on costs! So choose instance classes wisely. Keep in mind that autoscaling with multiple instances is more efficient than to have one big instance running 24/7

* Use tags. At least three: Name, Project, Environment (case-sensitive so use as is)

* Naming: Make sure that anyone with proper technical knowledge knows what a particular service is doing. So names should consists of `servicename-environment-#` e.g. `apollo-int-1` or `t01-apollo-redis-int-1` when more than one environment version is used

* VPC: never use or touch the default VPC! Create a new one and make sure that its CIDR does not overlap with the default one. If you need to delete it, use this script: [https://github.com/toddm92/vpc-delete/blob/master/remove_vpc.py]()
* Security Groups/IAM Permissions - Implement the principle of least privilege [https://en.wikipedia.org/wiki/Principle_of_least_privilege]()
* Subnetting: Make sure to have at least a private and a public subnet on each AZ. Additionally it's good practice to preserve an own public subnet for Loadbalancers
* Prefer AWS managed services over EC2 e.g. use RDS, Elasticache Redis/Memcached instead of running on "bare metal" EC2 - when you have to do so make sure that you can provide a proper patch lifecycle!
* Choose AMIs cost efficient. CentOS is the choice here. 
* Timezone: UTC
* Encryption: Secure interservice connections via SSL, make use of ACM and KMS 
* Monitoring: Make sure that you configure the loadbalancers healthchecks properly. HTTP 200 on / is unsatisfactory - the application has to provide a healthcheck path e.g. /health and it's mandatory that the instance/service is taken offline if this fails for reasons
* Don't be afraid trying out new things and consult the Docs ;-) 


Further Reading on best practices/lessons learned: [https://medium.com/@adhorn/10-lessons-from-10-years-of-aws-part-1-258b56703fcf]()

