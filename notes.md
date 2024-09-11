## Revision Notes

#### IAM (global)
- User and Groups , groups have many users ,  users can belong to mupltiple groups , users can exist alone without group
- Permissions : JSON document that outlines permission for users or group aka policies , policies attached to user or group
- Policies : (Version , Statement , Effect - [Allow, Deny] , Action [ec2:describe, iam:Get*] , Principal [which account/user policies applied to] ,  Resource [AWS resource , S3 etc])
- Custome policies can be created and applied
- Password , force pwd reset can be enabled , Multi Factor authentication (MFA) , Google Authnticator , Authy
- AWS CLI - direct access to API of AWS services , access keys needed to connect (ID & PASSWORD)
- AWS SDK - software develoment kit (java , python etc) , embedded within applicatoin
- Roles (for services not user) , permission to AWS services with IAM Roles (EC2 instance , lambda function roles)
- Roles have policies attached to control the access
- IAM Credentials Report (account level) - list all users and credentials status , audit
- IAM Access Advisor (user level) - service permission granted to users nn last accessed details , used to revise policy etc , audit

#### AWS CloudShell (not global)
- Terminal on AWS portal

#### EC2 (Elastic Compute Cloud) (global)
-  OS : Linux , Windows or Mac OS
-  CPU , RAM , Network attached (EBS & EFS) , Hardware (EC2 instance stoire)
-  Security group , bootstrapping scripts (runs once at boot time)
-  AMI (Amazon Machine Images) package OS , additional installations as reusable template .
-  Instance types
    * t-type and m-type for general purpose (web servers , code repositories -> balance compute , memeory n networking)
    * c-type for compute optimized (compute-intensive , batch processing , media trnascoding, high perf web server , high perf comuting , ML , gaming server)
    * r-type, x-type, and z-type for memory-optimized (high perf , relatinal/non db's , distributed cache , in-memeory database for PI , real time processing of unstrutured data)
    * d-type, h-type, and i-type for storage optimized (high sequential r/w . OLTP system , relatinal/non db's , redis (in-memery) , DFS)
    * f-type, g-type, and p-type for accelerated computing
