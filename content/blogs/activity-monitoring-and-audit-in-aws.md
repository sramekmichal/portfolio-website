---
title: "Activity Monitoring and Audit in AWS"
date: 2024-03-16
draft: false
author: "Michal Šrámek"
devto: "https://dev.to/sramek5/activity-monitoring-and-audit-in-aws-1hd3"
tags:
  - AWS
  - Security
  - Audit
image: /images/blogs/aws-security.jpg
description: "Activity Monitoring and Audit in AWS"
toc: true
---

Following text involves AWS native tools for the continuous monitoring of activities within an AWS environment to detect and respond to security threats and breaches. 

*It encompasses the discovery collection of best practices*, how to achieve this goal. Implementing this <u>secure</u> **Activity Monitoring and Audit in AWS**, will enhance security posture, mitigate risks, and better protect all AWS resources and sensitive data from unauthorised access or malicious activities.

Every decision made in AWS environment should be in accordance with the [AWS Security Reference Architecture](https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/architecture.html). This involves understanding the principles outlined in the architecture, conducting a risk assessment to identify potential security risks, and designing AWS architecture to incorporate security controls at every layer. 

## Framework

I personally recommend "internal audit framework" which is in compliance with AWS and should include the following (16) steps:

1. ***Start to use [AWS Landing Zone](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/understanding-landing-zones.html) with [AWS Control Tower](https://digitalcloud.training/what-is-aws-control-tower/) ([AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html))***

AWS Landing Zone primarily sets up the initial infrastructure (multi-account AWS environment, core infrastructure components like networking and IAM). AWS Control Tower provides ongoing management and enforcement of security and compliance controls (centralised governance, compliance capabilities and enforcing policies across entire AWS environment). AWS Organizations is the underlying AWS service of AWS Control Tower.

2. ***Turn on [AWS CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) in each AWS account***

AWS CloudTrail logs can be analysed in real-time to detect unauthorised access attempts or changes to critical resources. Integration with monitoring tools enables proactive threat detection and compliance monitoring across the AWS environment.

3. ***Store [AWS CloudTrail log](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-log-file-examples.html) in a centralised logging account with very restricted access***

Proper stream log management and analysis processes enable more efficient threat detection and incident response. Restricting access to the centralised logging account minimises the risk of unauthorised access, ensuring data confidentiality and integrity. 

4. ***Create [AWS CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_architecture.html) alarms for specific API calls***

Real-time notification alarms of critical events for specific API calls (high-volume data transfers, sensitive resource modifications) should be enabled. These alarms serve as proactive measures. AWS CloudWatch Logs Insights can also search API history beyond the last 90 days. Additional useful info [HERE](https://medium.com/free-code-camp/how-to-auto-create-cloudwatch-alarms-for-apis-with-cloudwatch-events-and-lambda-b128920857aa).

5. ***Use Logging IP traffic for VPCs and DNS logs***

Obtaining valuable informations using [VPC Flow Logs](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html) and [Amazon Route 53 resolver query logs](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-query-logs.html) and streaming them to either an Amazon S3 bucket or a CloudWatch log group is crucial.

6. ***Periodically examine “AWS log files” with [AWS GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html)***

This process enhances activity monitoring in AWS by proactively identifying security threats and suspicious activities. AWS GuardDuty can automatically analyse threat detection of AWS CloudTrail Events, VPC Flow Logs, DNS Logs and generally alerts to unexpected activity.

7. ***Enable [AWS S3 buckets logging](https://docs.aws.amazon.com/AmazonS3/latest/userguide/logging-with-S3.html) to monitor requests made to each bucket***

It allows to monitor requests made to each bucket and track access attempts, changes, and other activities. Analysing S3 access logs can help identify unauthorised access attempts, data breaches or misconfigurations.

8. ***Use [AWS Config](https://docs.aws.amazon.com/appconfig/latest/userguide/what-is-appconfig.html) for viewing historical IAM configuration and changes over time***

AWS Config is enabling to view the IAM policy that was assigned to a user, group, or role at any time. It is basically resource inventory (existing as well as deleted).

9. ***Collect alerts for IAM configuration changes and their audits***

By setting up alerts we can be notified on IAM configuration changes. Additional useful info [HERE](https://aws.amazon.com/blogs/security/how-to-receive-alerts-when-your-iam-configuration-changes/).

10. ***Set up [AWS Detective](https://docs.aws.amazon.com/detective/latest/adminguide/what-is-detective.html) controls around user creation and using a user credentials***

It is need to be implemented together with AWS Config when a new user or group is created and for any API actions performed by a non-federated IAM principal.

11. ***Periodically generate and download IAM credential report*** 

Report can be used to audit the effects of credential lifecycle requirements (lists all users,  status of their passwords, access key updates and MFA devices). It can be further reported to an external auditor.

12. ***Check regularly the [AWS IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)***

Achieving least privilege and grant the right fine-grained permissions. It provides capabilities to set, verify, and refine permissions, analyse external access and validate, that policies match corporate security standards.

13. ***Audit session activity using [AWS EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html)***

Set up rules to detect when changes happen to any AWS resources. It provides comprehensive visibility into user actions and system events.

14. ***Enable the session activity logging in [AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html)***

Continual stream of session data logs to AWS CloudWatch Logs with details (user’s commands in a session, the ID of the user and timestamps). Additional useful info [HERE](https://aws.amazon.com/blogs/security/how-to-record-ssh-sessions-established-through-a-bastion-host/).

15. ***Use [AWS Access Advisor](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_access-advisor.html) to refine set up permission guardrails***

It analyses last accessed information in AWS accounts. Permission guardrails help control which services users and applications can access and determine the services not used by IAM users and roles. With service control policies (SCPs), access to those services can be restricted.

16. ***Automatically collect and monitor evidence by [AWS Audit Manager](https://docs.aws.amazon.com/audit-manager/latest/userguide/what-is.html)***

Proactive measure reducing risk by fine-tuning AWS controls. Evidence is a record that contains the information needed to demonstrate compliance with the requirements specified by a control. Examples of evidence could be a change activity triggered by a user, or a system configuration snapshot.

## Conclusion
Implementing the above framework will help to ensure that the AWS environment is secure and compliant with security best practices. It will also help to detect and respond to security threats and breaches in a timely manner. By continuously monitoring activities within the AWS environment, organisations can better protect their resources and sensitive data from unauthorised access or malicious activities.