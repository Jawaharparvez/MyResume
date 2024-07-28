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
  ***Note***: *For Easy Understanding Use Exactly The Same Repository Secret Names*
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
  * Open this file(i.e., workflows folder) Create another file (Add File) with name extension as `github-actions-ec2.yml`
  * Open `github-actions-ec2.yml` once you open this it will show enter file contens here **Just Copy and Paste this**

```
name: Push-to-EC2

# Trigger deployment only on push to main branch
on:
  push:
    branches:
      - main

jobs:
  deploy:
    name: Deploy to EC2 on master branch push
    runs-on: ubuntu-latest

    steps:
      - name: Checkout the files
        uses: actions/checkout@v2

      - name: Deploy to Server 1
        uses: easingthemes/ssh-deploy@main
        env:
          SSH_PRIVATE_KEY: ${{ secrets.EC2_SSH_KEY }}
          REMOTE_HOST: ${{ secrets.HOST_DNS }}
          REMOTE_USER: ${{ secrets.USERNAME }}
          TARGET: ${{ secrets.TARGET_DIR }}

      - name: Executing remote ssh commands using ssh key
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.HOST_DNS }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            sudo apt-get -y update
            sudo apt-get install -y apache2
            sudo systemctl start apache2
            sudo systemctl enable apache2
            cd home
            sudo mv * /var/www/html
```
  * Click *Commit Changes*
#### Continuous Integration / Continuous Deployment
  ---Whenever you change the index.html file content and commiting change you'll automatically see the instance's content reflects the change
