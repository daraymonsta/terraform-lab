# Aim

To learn how to deploy infrastructure as code in Azure through Terraform

## Getting started

### Step 1: Run a Ubuntu virtual machine using VirtualBox 7

#### Why
You need Virtualbox to run Ubuntu as a virtual machine on your local computer.

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

   `. ./install-tf.sh`

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

3. Next, use this command to plan the terraform deployment: `terraform plan`

You will notice that you get this error:

*Error: building AzureRM Client: please ensure you have installed Azure CLI version 2.0.79 or newer. Error parsing json result from the Azure CLI: launching Azure CLI: exec: "az": executable file not found in $PATH.*

Can you work out why?  Hint: It says *"az": executable file not found in $PATH.*

It is because Azure CLI has not been installed.  Azure CLI commands usually start with `az`.

4. Install Azure CLI.

**Option 1**
Follow the instructions on <a href="https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt">Microsoft's Install the Azure CLI on Linux page</a>  to install it.

**Option 2**
Run this command: `curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`

5. Test the `az` command now works.  If it does, you should see a long list of commands it can do.  If it doesn't, you will see the error: *az: command not found*.

6. Repeat 3.

You will notice you get a new error:

*Error: building AzureRM Client: obtain subscription() from Azure CLI: parsing json result from the Azure CLI: waiting for the Azure CLI: exit status 1: ERROR: Please run 'az login' to setup account.*

Once again, can you work out why?  Hint: *ERROR: Please run 'az login' to setup account.*

It is because we need to login to Azure cloud using the login `az login`.  Let's do that next...

7. Login to Azure cloud using the command `az login`.  You will need to login as a user with at least the User Access Administrator role (so that later the user can create a Service Principal with Contributor permissions).

If you need to add the role to a user, go to Step 5.  Otherwise, skip Step 5.

### Step 5: Adding 'User Access Administrator' role to a user

1. Login to Azure portal using your 'Owner' account.

2. Go to *Subscriptions*, then click on the subscription you want to use.

3. From the left-side pane, click *Access control (IAM)*.

4. At the top, click on *+Add*, then *Add role assignment*.

5. Search for the *User Access Administrator* role, then click on it so that it is selected in a grey colour.

6. At the top, click on *Members*, then click *Select members*

7. Click on the user in the list if you can see it (otherwise search to find it).  Click *Select* button at the bottom.

8. Click the *Review + assign* button at the bottom.

### Step 6: Create a Service Principal with 'Contributor' permissions

#### Why are we doing this?

A Service Principal is like a user account used by applications - when an application needs permission to do something it can use this instead of real person's user account.

1. Run this command to create a Service Principal with *Contributor* permissions.  Update the `<SUBSCRIPTION_ID>` with your own subscription ID.  You can also change the name if you prefer.

`az ad sp create-for-rbac --name="terraform-service-principal" \
                          --role="Contributor" \
                          --scopes="/subscriptions/<SUBSCRIPTION_ID>"`

If it is successful, you will see some credentials for this user account on the screen similar to this:

{
  "appId": "xxxxxx-xxx-xxxx-xxxx-xxxxxxxxxx",
  "displayName": "terraform-service-principal",
  "password": "xxxxxx~xxxxxx~xxxxx",
  "tenant": "xxxxx-xxxx-xxxxx-xxxx-xxxxx"
}

Save this information in a safe place such as a key or password vault.  You will need it later.

### Step 7: Set your environment variables

#### Why are we doing this?

Terraform needs to be able to perform actions on your behalf to create resources in Azure.  It needs to look up the credentials from somewhere.  You could save them in your Terraform configuration files - but this would not be good practice from a security perspective.  You are better to set the credentials to environment variables each time you use your VM before running terraform deployments.

1. Run these commands using the values for the Service Principal's credentials you saved in Step 6.  Replace the variable values with the values returned by the command in Step 6 (plus the subscription ID you also used in Step 6).

Note: The ARM_CLIENT_ID is the *AppId*, the M_CLIENT_SECRET is the *password*, the ARM_TENANT_ID is the *tenant*.

`export ARM_CLIENT_ID="<APPID_VALUE>"
export ARM_CLIENT_SECRET="<PASSWORD_VALUE>"
export ARM_SUBSCRIPTION_ID="<SUBSCRIPTION_ID>"
export ARM_TENANT_ID="<TENANT_VALUE>"`

Recommended: Save these commands (with the values ready to go) in a key/password vault so they can be run as soon as you have started up your VM.

Other options: You could also save the commands in a script file (you will remember they have the extension .sh).  But you will need do one of these to keep your credentials from being leaked onto a public repo:

Option 1: Keep your script file in a place outside your repo.

Option 2: Keep your script file in your repo, but add the script file's name to your *.gitignore* file so that it will not be copied/committed to the public repo.

### Step 8: Try to deploy an Azure resource group (again)

1. Make sure you are back in the *deploy-rg* folder.

#### How to check if you are in the *deploy-rg* folder

Check A: Your command prompt will start similar to this:

`~/labs/github/terraform-lab/deploy-rg$`

Note: You could also use the command `pwd` (to check your *Present Working Directory*).

Check B: Run the `ls` command to check if there is the *main.tf* file in the folder you are currently in.

If you are not in the correct folder, use the `cd` command to change directory.  Use `cd` by itself or `cd ~` to go back to your home directory/folder if that helps.  Use `cd ..` to out of the folder you are in.

2. Run `terraform apply`.  

If you are not in the folder we initialised in Step 4.2, you will get this error:

`Error: No configuration files`

If you are on the right track, you should see this on your screen:

`erraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_resource_group.rg will be created
  + resource "azurerm_resource_group" "rg" {
      + id       = (known after apply)
      + location = "uksouth"
      + name     = "test-rg"
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: `

3. Enter *yes* and push Enter.

If your deployment was successful, you should see something like this:

`azurerm_resource_group.rg: Creating...
azurerm_resource_group.rg: Creation complete after 1s [id=/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/test-rg]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.`

4. If you like, check it was created by going to the Azure portal.  It may take a few seconds to show up.

5. You can also check the state of your deployed configuration (according to Terraform) by running this command:

`terraform show`

6. You can also check the list of resources that were deployed (according to Terraform) by running this command:

`terraform state list`

If you have got this far, well done!  You are successfully made your first Azure cloud deployment using Terraform.  It's not over though...

### Step 9: Destroy (remove) the resource group your created using Terraform

#### Why do we need to do this?

If you don't, the resource group will stay there in the cloud.  This is not a problem for a resource group (which is free), but if you deploy resources that cost you money, you will want to remove them as soon as they are not needed.

1. Run this command: `terraform destroy`

You should see the following:

`azurerm_resource_group.rg: Refreshing state... [id=/subscriptions/ad76532f-949a-44f6-af8e-61ac4fe2e53d/resourceGroups/test-rg]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # azurerm_resource_group.rg will be destroyed
  - resource "azurerm_resource_group" "rg" {
      - id       = "/subscriptions/ad76532f-949a-44f6-af8e-61ac4fe2e53d/resourceGroups/test-rg" -> null
      - location = "uksouth" -> null
      - name     = "test-rg" -> null
      - tags     = {} -> null
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value:`

2. Type 'yes' and push Enter.

You should see this:

`azurerm_resource_group.rg: Destroying... [id=/subscriptions/ad76532f-949a-44f6-af8e-61ac4fe2e53d/resourceGroups/test-rg]
azurerm_resource_group.rg: Still destroying... [id=/subscriptions/ad76532f-949a-44f6-af8e-61ac4fe2e53d/resourceGroups/test-rg, 10s elapsed]
azurerm_resource_group.rg: Destruction complete after 15s`

You have now removed your first Azure resource group using Terraform!  Well done!