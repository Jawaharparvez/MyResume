# MyResume
# Hosting Resume on AWS EC2 Instance with CI/CD setup using GitHub Actions
### Step 1: Go to EC2 from AWS Console 

### Step 2: Create a new Security Group
  * Go to Security Groups in EC2 Dashboard
  * Select Create Security Group (My SecurityGroupName: Linux-SG)
  > * Inbound Rules
  > * Add Rule
  > * ***Type***:SSH  ***Port Range***: 22 ***Source***: Anywhere-IPv4 (0.0.0.0/0)
  > * ***Type***:HTTP  ***Port Range***: 80 ***Source***: Anywhere-IPv4 (0.0.0.0/0)
  > * ***Type***:HTTPS  ***Port Range***: 443 ***Source***: Anywhere-IPv4 (0.0.0.0/0)
  >  
  * Then Click On **Create Security Group**
 
### Step 3: Create EC2 Instance
  * Click on Launch Instance
  * Create an Instance Name (My First Web Server)
  * Select OS(Operating System) as Ubuntu at Amazon Machine Image
  * Select Instance type as t2.micro (Because it is available under Free tier)
  * Select / Create a New Keypair (MyKeypair name: My-kp1 and Type: RSA)
  * Click On *Select existing security group* select on the security group we created in step 2
  * Leave the Configure Storage the same
  * Click on **Launch Instance**
 

