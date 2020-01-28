# Overview

This standard defines the baseline security configuration and procedural requirements for Client’s technical infrastructure. Exceptions to this policy must be requested to, and approved by the Client leadership team.
# Laptops - Mac
 Allow Mac laptops should have the following configuration:
 Firewall enabled
 Disk encryption enabled
# Application Deployments
 All inbound traffic into a Client environment must use an encrypted transport.
 All data stored at rest must be encrypted; this included backups.
# Servers Deployed in EC2
 Must have IAM policies secured using IAM Roles for EC2
 IAM Access keys should not be deployed to EC2 services via application configuration.
 Must be managed via AWS OpsWorks
# For Linux
 All users must use SSH with their own private key to access EC2 Servers.
 Shared keys are not permitted.
 Password based authentication is not permitted.
 Must be provisioned using an AMI derived from a base Amazon Linux AMI with AWS Inspector Agent installed
 Periodic operating system level vulnerability scanning must be performed using AWS Inspector.
# For Windows - All users must use temporary passwords requested via the RDP request feature in AWS OpsWorks.

## Note: Client does not use Windows in its main application architecture. Client does use Windows for testing some integration scenarios, for example we have a test server with an ADFS/IdP configuration to facilitate testing SAML integration.
# Application Configuration
## Managing Secrets
 Should not be stored in plain text application configuration files. Examples: encryption keys, sftp credentials, etc.
 Internally managed secrets should use AWS Parameter Store, using environment specific Encrypted String values.
 Tenant secrets provided by customers during deployments should be stored in encrypted fields within the database used for the customer.
