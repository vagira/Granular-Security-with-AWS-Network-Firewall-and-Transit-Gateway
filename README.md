# Granular-Security-with-AWS-Network-Firewall-and-Transit-Gateway
Granular Security with AWS Network Firewall and Transit Gateway
AWS Network firewall, along with AWS Transit Gateway route tables provides powerful mechanism for security isolation of the traffic between AWS VPCs and optimise traffic inspection for the selected traffic flows. 
As of now, AWS Network Firewall is still in its infancy and do not stand up to the similar Next Gen firewalls from established vendors. However, as with other AWS offerings, we can expect rapid advances and feature improvements in this space. We can make a reasonable bet on the future of Network Firewall be lot better in 6 month’s time. 
Network and Firewall security fundamentals used in this example
VPC is the isolated security environment. Whilst VPC had NACLs and Security Groups, objective is to use inter VPC communication with different levels of permissions. 
•	The highest level security is achieved by not having two VPCs to communicate with each other at all. i.e. – No Transit Gateway routing path. This is equivalent to the real life example of separating production VPC from a highly unsecure Test/Sandpit VPC. No communication between these two.

•	Second highest level of security is to permit the communication between VPCs via AWS Network Firewall. In this case traffic will flow via Inspection VPC. Network firewall is capable of doing layer 7 inspection to permit granular traffic patterns. With layer 7 inspection, firewall can inspect payload of the packet and you have the flexibility to deploy IPS rules. This can be requirement to real life example of enabling restricted communication from customer billing system workflow in one VPC from manufacturing in another VPC.

•	Sending traffic via Network firewall is however costly in this topology with Firewall usage charges and Transit Gateway charges associated with extra hop. For some workflows between VPCs such level of security is not needed and can be permitted directly between VPCs without Firewall inspection. However they still will have AWS NACL and Security Group level isolation. A real life example is workload VPCs accessing shared services VPC or Endpoint services VPC.

Objectives tested –
•	EC2 instance in VPC1 and EC2 instance in VPC2 can communicate via AWS Network Firewall in Inspection VPC.
•	EC2 instance in VPC1 and EC2 instance in VPC3 can communicate without AWS Network Firewall in Inspection VPC. 
•	EC2 instance in VPC2 and EC2 instance in VPC 3 has no communication at all. i.e. VPC2 and VPC3 are completely isolated.
   
For modularity POC is divided into multiple templates. All you need to do is to deploy them in following order 
Deployment instructions
Deploy the stacks using following templates in following order
1.	TGW deployment.yml
2.	VPC1.yml
3.	VPC2.yml
4.	VPC3.yml
5.	Inspection VPC.yml
6.	TGW route modify.yml
7.	3 EC2 Instances
(Don’t change default parameters unless you want to customise.)
Final template (3 EC2 Instances) is deployed to gain SSM access into the EC2 in VPC1 and VPC2 for testing. You cannot access EC2 in VPC3 via SSM. But can ping it for testing purpose. 
You can try blocking the firewall rules and check the reachability. Communication between EC2 in VPC1 and EC2 in VPC3 has no bearing on the firewall. 
(Public subnet used for reduce VPC endpoint cost and still access via SSM. If you plan to deploy in production please use VPC endpoints for SSM access and do not use public subnets with public IP addresses)
