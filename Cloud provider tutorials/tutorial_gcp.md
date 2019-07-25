---
css:
  - css/hero-image.css

# GCP Tutorial

Cloud computing uses on-demand, scalable, and elastic computational resources for more powerful and efficient processes. Here, we provide instructions for getting set up with a leading cloud service, [Google Cloud Platform (GCP)](https://cloud.google.com/). This process will require a Google account, which you can create [here](https://accounts.google.com). It will also require billing information, but GCP currently offers a $300 credit for first-time users. 

# Get Set up for Cloud Computing

In a new window, use your Google account to sign into [Google Cloud Platform Console](https://console.cloud.google.com/).

Click the "Activate Cloud Shell" button at the top right corner of the Google Cloud Platform Console. 

![Activate Cloud Shell](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/activate-cloud-shell.png "Activate cloud shell")
   
You now will see your Cloud Shell session window:
![Cloud Shell](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/cloud-shell-commandline.png "Cloud shell")

*Tips for navigating the shell:*

   * Note that in the shell, you must use keyboard commands to execute copy and paste commands. In Windows or Unix/Linux, use the shortcut `Control+C` to copy and `Control+V` to paste. On macOS, use `Command+C` to copy and `Command+V` to paste.
   * To scroll in the Cloud Shell, enable the scrollbar in `Terminal settings` with the wrench icon.


![Cloud shell wrench](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/cloud-shell-commandline.png "Cloud shell wrench")

## Getting set up on GCP
   * In a new window, sign in at <dfn id="def-ncbi"><a href="https://console.cloud.google.com/">Google Cloud Platform Console</a></dfn>.
   
   * Click "Activate" to activate the $300 credit.

![Activate](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/gcp-credit.png "Activate")
   
   * Enter your country and check the box indicating that you have read and accept the terms of service.
   
   * Under “Account type,” select “Individual.”
    
   * Enter your name and address.
    
   * Under “How you pay," select “Automatic payments.” This indicates that you will pay costs after you have used the service, either when you have reached your billing threshold or every 30 days, whichever comes first. 
    
   * Under “Payment method,” select “add a credit or debit card” and enter your information. You will be forced to enter this information; however, you will not be charged without your consent once the trial ends. 
    
   * Click “Start my free trial” to finish registration.

## Creating a Virtual Machine (VM)
* Click on the navigation menu in the top left corner and select "Compute Engine".

![GCP Instance](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/gcp-instance.png "GCP Instance")

* Click "Create Instance" on the top bar.  

* Create an image with the following parameters (choose default if an option is unavailable): 
    * Name: trial-vm
    * Region: us-east4 (Northern Virginia)
    * Machine Type: micro (1 shared vCPU), 0.6 GB memory, f1-micro
    * Boot Disk: Click "Change," select Ubuntu 18.04 LTS, and click "Select" (Boot disc size is 10 GB).

* At this point, you should see a cost estimate for this instance on the right side of your window. 

![GCP VM Cost](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/gcp-vm-cost1.png "GCP VM Cost")

* Click “Create”.

## Accessing VM from a local machine
Now that you've created a VM, you must access it from your local computer. On GCP, the easiest way is to SSH from the browser.

Connect to your new VM instance by clicking the "SSH" button.

![GCP SSH](https://github.com/NCBI-Hackathons/ncbi-cloud-tutorials/blob/master/images/gcp-ssh.png "GCP SSH")

Remember to [stop](https://cloud.google.com/compute/docs/instances/stop-start-instance) or [delete](https://cloud.google.com/compute/docs/instances/stop-start-instance) the VM to prevent incurring additional cost.

[= top_nav =]

{! _top-nav.md !}

[= mobile_nav =]

{! _mobile-nav.md !}
