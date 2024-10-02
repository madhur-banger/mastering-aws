
![Security VS NACL](https://github.com/saikiranpi/mastering-aws/assets/109568252/2fe56c98-cc17-4589-821d-9b54e16ac47c)


Here’s a comprehensive comparison of Security Groups (SG) and Network Access Control Lists (NACLs) in AWS, integrating the analogy of firewalls and practical examples to illustrate their functionalities:

# Security Groups vs. Network Access Control Lists (NACLs)

## Overview

## Security Groups (SG):

	•	Definition: Security groups act as stateful firewalls controlling traffic at the instance level based on rules.
	•	Functionality: They regulate both inbound and outbound traffic and are associated with individual instances.
	•	Default Behavior: All inbound traffic is denied by default, while all outbound traffic is allowed.
	•	Evaluation: They require explicit rules to allow traffic, and once a connection is allowed, responses are automatically permitted.

## Network Access Control Lists (NACLs):

	•	Definition: NACLs function as stateless firewalls controlling traffic at the subnet level based on rules.
	•	Functionality: They evaluate inbound and outbound traffic separately and are associated with subnets.
	•	Default Behavior: All inbound and outbound traffic is allowed by default when a custom NACL is created (initially denies all).
	•	Evaluation: Rules are evaluated in numerical order; both allow and deny options are available.

## Practical Examples

Security Groups Example:
Suppose you have an EC2 instance with default security group settings:

	1.	All inbound traffic is denied by default.
	2.	Outbound traffic is allowed by default.

## Testing Scenarios:

	•	Allow All Inbound Traffic: Remove all outbound rules. Test internet connectivity; the instance can connect to the internet due to outbound allowance.
	•	Restrict Outbound Access to Websites: Add outbound rules for HTTP (port 80) and HTTPS (port 443). Test again to ensure access is limited to only specified websites.
	•	Allow ICMP Protocol for Ping: To enable ping functionality, add an outbound rule allowing ICMP traffic.

## Key Takeaway: Security groups start with a default deny stance and require explicit rules to allow traffic.

## Network Access Control Lists (NACLs) Example:
Consider a scenario where you need a web server accessible from the internet:

	1.	Outbound Rules: Allow all traffic.
	2.	Inbound Rules:
	•	Allow TCP port 80 from 0.0.0.0/0 (anywhere) for web traffic.
	•	Allow TCP port 22 from your IP address for SSH access.

Testing Scenario:
By configuring NACLs this way, web traffic (HTTP) is permitted from anywhere, while SSH access is restricted to your specific IP address.



Conclusion

	•	Security Groups: Best for controlling instance-level traffic. They provide simpler management and automatic statefulness, making them suitable for managing access to individual resources.
	•	Network Access Control Lists (NACLs): Ideal for subnet-level traffic control, providing an additional layer of security across multiple instances. They require more careful management of inbound and outbound rules due to their stateless nature.

By understanding these distinctions, you can effectively manage AWS network security in your applications and prepare for potential interview scenarios regarding AWS networking concepts.


![Sg vs NACL](https://github.com/saikiranpi/mastering-aws/assets/109568252/8a623a87-f5f0-4d5d-a84c-268f28bd690a)

