# Lesson 2 - Infrastructure as code: Deploy using Terraform

Note: Extra hints are at the end of this document

## Steps
1.	Boot into your Linux VM
2.	Open your Terminal
3.	Set environment variables with your Service Principal credentials

    Stuck?  Go to the README.md in the root of this repo, Step 7.

    Haven't made your Service Principal yet?  Go to the README.md in the root of this repo, Step 6.

4.	Go to the directory "terraform-lab" (the repo you should have cloned)

    If you haven't cloned it yet?  See Hint 1.1. 

5.	Go into the child directory "deploy-rg" and run "terraform apply" to get if you can deploy just a resource group.  Once it's created, go ahead and destroy it using the "terraform destroy" command.

6.  Go up to the parent directory (Not sure how to? See Hint 1.2) - you will then be in the "terraform-lab" directory.

7.  Copy the "deploy-rg" directory to another folder called "deploy-rg-storage" so you can make changes in the new directory (make sure you use the recursive switch to get the folder and everything in it).

    Stuck?  See Hint 1.3.

8.	Go to the new directory you just made called "deploy-rg-storage"
9.	DO NOT close your Terminal window.  Go to a web browser either on your VM or local computer, and Google search for Terraform code that will deploy an Azure storage container and blob.
10.	Copy the sample Terraform code you find from terraform.io.
11.	Using Visual Studio Code on your Linux VM, open the main.tf file in the "deploy-rg-storage" folder (I prefer to open the parent folder "terraform-rg", then navigate to the file).
12.	Paste the sample code in the appropriate place.  Modify it to suit the resource group in needs to go into.
    Extension:  If you are also creating the storage container + uploading a file to it from your local computer, you can create a random file to upload - it will be easiest to store it in the same folder as your main.tf file.
    Want to create an empty file using Terminal?  Hint 1.4
13.	Deploy your code.

    Stuck? Hint: 1.5

14.	Check your storage account, storage container and uploaded file exists in Azure cloud using the portal.

    Can't remember the portal address?  Hint 1.6

15.	Destroy the resources you made using Terraform.

    Stuck?  Hint 1.7



<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>






## Extra hints:
1.1	Run the command:  git clone https://github.com/daraymonsta/terraform-lab

1.2 Run the command: cd ..

    Make sure there's the space between "cd" and ".."

1.3	Run the command: cp -r deploy-rg deploy-rg-storage

1.4	To create a blank file using your Terminal window, run the command: touch <name of file>

1.5	In your previously-used Terminal window, run the command: terraform deploy

1.6	Use portal.azure.com

1.7	Run the command: terraform destroy

