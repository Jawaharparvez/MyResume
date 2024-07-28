# Hosting Resume on AWS EC2 Instance with CI/CD setup using GitHub Actions
### Step 1: Go to EC2 from AWS Console 

### Step 2: Create a new Security Group
  * Go to Security Groups in EC2 Dashboard
  * Select Create Security Group (My SecurityGroupName: Linux-SG)
  > * Inbound Rules (Add Rule)
  > * ***Type***:SSH    ***Port Range***: 22   ***Source***: Anywhere-IPv4 (0.0.0.0/0)
  > * ***Type***:HTTP    ***Port Range***: 80   ***Source***: Anywhere-IPv4 (0.0.0.0/0)
  > * ***Type***:HTTPS    ***Port Range***: 443   ***Source***: Anywhere-IPv4 (0.0.0.0/0)
  >  
  * Then Click On **Create Security Group**
 
### Step 3: Create EC2 Instance
  * Click on Launch Instance
  * Create an Instance Name (My First Web Server)
  * Select OS(Operating System) as Ubuntu at Amazon Machine Image
  * Select Instance type as t2.micro (Because it is available under Free tier)
  * Select / Create a New Keypair (MyKeypair name: My-kp1 and Type: RSA) it will be downloaded to your Computer
  * **Save the Keypair safe Cause it is generated only once**
  * Click On *Select existing security group* select on the security group we created in step 2
  * Leave the Configure Storage the same
  * Click on **Launch Instance**
    
### Step 4: GitHub Repository
  * Create new Repository and upload files that contains your Resume's HTML(index.html) File and others css etc....

### Step 5: Secrets and Variables for (CI/CD)
  * On top open *settings* inside your repository
  * On left side there is security containing ***Secrete and Variables*** open it
  * Click on New Repository Secrete (You need to create 4 secrets)
  ***Note***: *For easy Understanding Use exactly the same Repository secret names*
  * ###### Secret 1
      * Name exactly this `EC2_SSH_KEY` and secret is your keypair 
      * Open your key pair in your computer and Copy the complete Key including <br>
        ` -----BEGIN RSA PRIVATE KEY----- ` <br>
        ` -----END RSA PRIVATE KEY----- `
      * Paste the key in the secret and Click *Add secret*
  * ###### Secret 2
      * Name Exactly this `HOST_DNS` and secret is your AWS provided Public IPv4 DNS for your Instance
      * It will something look like this `ec2-xx-xxx-xxx.compute-x.amazonaws.com`
      * Copy this type of Public IPv4 DNS given by AWS for your Instance and paste in the secret and Click *Add Secret*
  * ###### Secret 3
      * Name Exactly this `TARGET_DIR`
      * secret is `home` and Click *Add secret*
  * ###### Secret 4
      * Name Exactly this `USERNAME`
      * secret is `Ubuntu` and Click *Add secret*
        
### Step 5: GitHub Actions
  * Create a new file name using(Add File) with name extension as `.github/workflows`
  * open this file(i.e., workflows folder) Create another file (Add File) with name extension as `github-actions-ec2.yml`
  * 
