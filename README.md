# Aim

To learn how to deploy infrastructure as code in Azure through Terraform

## Getting started

### Step 1: Run a Ubuntu virtual machine using VirtualBox 7

#### Why
You need Virtualbox to run the Ubuntu as a virtual machine on your local computer.

#### How
Follow the instructions on the <a href="https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview">Ubuntu tutorial</a> page

### Step 2: Clone this repo

#### Why

This will give you the basic Terraform code to get you started, plus a few other helpful files.

#### How

1. Go into VirtualBox and run the Ubuntu virtual machine you created in Step 1
2. Go to the terminal
3. Create a labs folder using this command:

`mkdir labs`

4. Go into that folder using this command:

`cd labs`

5. Now that we are in the 'labs' folder, we will now clone (create a copy) this repo within it. Use this command:

`git clone https://github.com/daraymonsta/terraform-lab`

You will see that the 'terraform-lab' folder with this repo's files is created for you.  

6. Check that you can see the 'terraform-lab' folder listed. Use this command:

`ls`

### Step 3: Install Terraform

#### Why

We will use Terraform as our main tool to deploy infrastructure as code to Azure

#### How
**Option 1**

Follow the instructions on the <a href="https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli">Terraform installation</a> page.

**Option 2**

Run the script file to do the Terraform install for you instead - script files really help with automation.

1. Following on from Step 2, go into the 'terraform-lab' folder using this command:

`cd terraform-lab`

2. List the files in this folder using the command `ls`.  You should see the file 'install-tf.sh' (it ends with .sh so it is a script file).

3. Try to run the file by typing this:

`./install-tf.sh`

You should get a *Permission denied* error.  This is because this file does not yet have executable permissions.

4. Add executable permissions to the 'install-tf.sh' by using this command:

`chmod +x install-tf.sh`

5. Now repeat 3.  The script should now run.  It will take a minute or two to finish.

6. Test the `terrform` command now works.  If it does, you should see a long list of commands it can do.  If it doesn't, you will see the error: *terraform: command not found*.

You are now setup to try Terraform!

### Step 4: Try to deploy an Azure resource group

#### Why

We want to automate the create of a resource group in Azure cloud.

#### How

1. Go to the *deploy-rg* folder using this command: `cd deploy-rg`

2. Initialise this folder for terraforming using this command: `terraform init`