# AWS Tutorial

Cloud computing uses on-demand, scalable, and elastic computational resources for more powerful and efficient processes. Here, we provide instructions for getting set up with a leading cloud service, [Amazon Web Services (AWS)](https://aws.amazon.com/). Then we will set up a virtual machine instance. This process will require a Amazon account, which you can create [here](https://portal.aws.amazon.com/billing/signup#/start). It will also require billing information, but AWS currently offers 12 months of free tier access. AWS will not charge you if you stay within the limits of the AWS free tier.

## Getting set up on AWS
   * In a new window, sign in at <dfn id="def-ncbi"><a href="https://aws.amazon.com/">Amazon Web Services</a></dfn>.

## Creating a Virtual Machine (VM)
  * Open a [AWS Management Console](https://console.aws.amazon.com/console/home)
  * If prompted, enter your AWS username and password
  * Type "EC2" in the search bar
  * Select "Amazon EC2" from the search results
  * On the page that loads, select "Launch Instance"
  ![AWS Launch Instance](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/aws-launch.png "AWS Launch Instance")
  * Find the entry that begins with "Amazon Linux AMI" and click "Select" on the right
  ![AWS AMI](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/aws-1.png "AWS AMI")
  * Select the row with "t2 micro" in the "Type" column
  ![AWS Micro](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/aws-2.png "AWS Micro")
  * Click "Review and Launch" at the bottom of the page.
  * Click "Launch" at the bottom of the page.
  ![AWS settings](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/aws-3.png "AWS settings")
  * From the first dropdown menu, select "Create a new key pair"
  * In the field for "Key pair name", enter the name "MyKeyPair"
  * Click the "Download Key Pair" button
  ![AWS key pair](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/aws-4.png "AWS key pair")
  * **Windows users:** Save your key pair to a sub-directory called .ssh (ex. C:\user\{yourusername}\.ssh\MyKeyPair.pem). If you try to use Windows Explorer to do this, you will have to title the ".ssh" folder as ".ssh." 
  * **Mac & Linux users:** Save your key pair to a sub-directory from your home directory (ex. ~/.ssh/MyKeyPair.pem). For Mac users, the key pair is downloaded to your Downloads directory by default; you can move the key pair into the .ssh sub-directory by entering the following command in a terminal window: mv ~/Downloads/MyKeyPair.pem ~/.ssh/MyKeyPair.pem
  * Returning to the AWS website, click "Launch Instance"
  * Click "View Instances" and wait until the "Instance State" column on your instance changes to "running"
  ![AWS key pair](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/aws-5.png "AWS key pair")
  * Copy the IP address of your AWS instance
  ![AWS key pair](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/aws-6.png "AWS key pair")

## Windows users only: Access VM from local machine
  * If you do not have Git installed, download it [here](https://git-scm.com/download/win).
  * Right click on your desktop and select "Git Bash Here"
  * Enter the following into the window and press the enter key, replacing "yourusername" with your username and "IP_Address" with the IP address we copied earlier: ssh -i 'c:\Users\yourusername\.ssh\MyKeyPair.pem' ec2-user@{IP_Address}
  * You should see a response that looks like "The authenticity of host 'ec2-198-51-100-1.compute-1.amazonaws.com (10.254.142.33)' can't be established. RSA key fingerprint is 1f:51:ae:28:df:63:e9:d8:cf:38:5d:87:2d:7b:b8:ca:9f:f5:b1:6f. Are you sure you want to continue connecting (yes/no)?"
  * Type **yes** and press the enter key
  * You should see a response that looks like "Warning: Permanently added 'ec2-198-51-100-1.compute-1.amazonaws.com' (RSA) to the list of known hosts."
  * You are now connected to your VM from your local machine


## Mac and Linux users only: Access VM from local machine
  * Open a terminal window. 
  * Enter the following into the window and press the enter key: chmod 400 ~/.ssh/MyKeyPair.pem
  * Enter the following into the window and press the enter key, replacing "IP_Address" with the IP address we copied earlier: ssh -i ~/.ssh/MyKeyPair.pem ec2-user@{IP_Address}
  * You should see a response that looks like "The authenticity of host 'ec2-198-51-100-1.compute-1.amazonaws.com (10.254.142.33)' can't be established. RSA key fingerprint is 1f:51:ae:28:df:63:e9:d8:cf:38:5d:87:2d:7b:b8:ca:9f:f5:b1:6f. Are you sure you want to continue connecting (yes/no)?"
  * Type **yes** and press the enter key
  * You are now connected to your VM from your local machine

## Terminating your instance
You must remember to stop or delete your VM to prevent incurring additional cost.
  * On the AWS page, select the box next to your instance
  * Click the "Actions" button
  * Navigate to "Instance State"
  * Click "Terminate"
  ![Terminate part 1](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/aws-10.png "Terminate part 1")
  * Click "Yes, Terminate"
  * Wait a few seconds and confirm that "Instance State" has changed to "terminated"
  ![Terminate part 2](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/aws11.png "Terminate part 2")

## More help
If any of these steps did not work, please check out the official AWS VM tutorial [here](https://aws.amazon.com/getting-started/tutorials/launch-a-virtual-machine/).
