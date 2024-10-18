# What AWS VPC?

## VPC -> Amazon Virtual Private Cloud (Amazon VPC)
<div align="center">
  <img alt="Demo" src="../img/vpc.png" />
</div>

## Subnets -> A subnet is a range of IP addresses in your VPC

## Routing -> Use route tables to determine where network traffic from your subnet or gateway is directed.

## internet gateway->  A gateway connects your VPC to another network. For example, use an internet gateway to connect your VPC to the internet. 
## NAT Gateway ->  Network Address Translation (NAT) NAT gateway so that instances in a private subnet can connect to services outside your VPC but external services cannot initiate a connection with those instances.

## NACL -> Network Access Control List
 ### NACL Apply at subnet level 
 ### security group for allow traffic but NACL For deny and allow traffic  
 ### vpc -> subnet -> route table -> internet gateway -> security group -> nacl +api ->last point 

 ## Route 53 -> A reliable way to route users to internet applications. Amazon Route 53 is a highly available and scalable cloud Domain Name System (DNS) web service.

###  main functions of  Route 53 
<ul>
<li>Domain names</li>
<li>Hosted zones</li>
<li>Health checks</li>
<li>Traffic flow</li>
<li>Resolver</li>
</ul>

## Benefits and features
<ul>
<li><p>Highly available and reliable</p></li>
<li><p>Designed for use with other AWS services</p></li>
<li><p>Simple</p></li>
<li><p>Flexible</p></li>
</ul>



# What AWS IAM ?  

## Identity and Access Management (IAM)
 ### With IAM, you can manage permissions that control which AWS resources users can access. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources. IAM provides the infrastructure necessary to control authentication and authorization for your AWS accounts.


 ## Users-> Identity Center is enabled. We recommend managing workforce users' access to AWS accounts and cloud applications in Identity Center.


 ## User groups-> A user group is a collection of IAM users. Use groups to specify permissions for a collection of users.

 ## Roles -> An IAM role is an identity you can create that has specific permissions with credentials that are valid for short durations. Roles can be assumed by entities that you trust.

 ## Policies -> A policy is an object in AWS that defines permissions.
  
## User -> access key-> 