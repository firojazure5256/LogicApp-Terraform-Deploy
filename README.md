# Deploy Logic App Standard with Terraform and Azure DevOps pipelines

This project provides examples on how to use Terraform and Azure DevOps to create standard Logic App. 

Overview

Terraform is an open source infrastructure as code tool to create, change, and improve infrastructures. You can use Terraform to easily deploy Azure resources such as resource group, app service, storage account etc. This blog provides sample code and detailed instructions on the deployment of the standard Logic App infrastructure on Azure using Terraform in an Azure DevOps pipeline.

 

Terraform 
You can clone this repo LogicApp-Terraform-Deploy from Github. Find the Terraform/LAstandard.tf and check the terraform code.

 

In this code you can see that we will be creating the following resources on Azure:

1. The resource group which your Logic App resources will de deployed to.

2. The storage account hosting the Logic App standard.

3. The app service plan.

4. The application insights and the associated log analytics workspace.

5. The standard Logic App

Example:
Overview

Terraform is an open source infrastructure as code tool to create, change, and improve infrastructures. You can use Terraform to easily deploy Azure resources such as resource group, app service, storage account etc. This blog provides sample code and detailed instructions on the deployment of the standard Logic App infrastructure on Azure using Terraform in an Azure DevOps pipeline.

 

Terraform 
You can clone this repo LogicApp-Terraform-Deploy from Github. Find the Terraform/LAstandard.tf and check the terraform code.

 

In this code you can see that we will be creating the following resources on Azure:

1. The resource group which your Logic App resources will de deployed to.

2. The storage account hosting the Logic App standard.

3. The app service plan.

4. The application insights and the associated log analytics workspace.

5. The standard Logic App

Example:

thumbnail image 1 of blog post titled 
	
	
	 Overview

Terraform is an open source infrastructure as code tool to create, change, and improve infrastructures. You can use Terraform to easily deploy Azure resources such as resource group, app service, storage account etc. This blog provides sample code and detailed instructions on the deployment of the standard Logic App infrastructure on Azure using Terraform in an Azure DevOps pipeline.

 

Terraform 
You can clone this repo LogicApp-Terraform-Deploy from Github. Find the Terraform/LAstandard.tf and check the terraform code.

 

In this code you can see that we will be creating the following resources on Azure:

1. The resource group which your Logic App resources will de deployed to.

2. The storage account hosting the Logic App standard.

3. The app service plan.

4. The application insights and the associated log analytics workspace.

5. The standard Logic App

Example:

thumbnail image 1 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

 

Azure pipeline

 

Create a project
To create a pipeline, first we will need to create a new project in Azure DevOps

Once the project is created, we can go to the project-Repos, and import the code from Github or git push code from existing repository. 

thumbnail image 2 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

Create a service connection
Before we create a pipeline, to deploy the resources to Azure, we will need to create a service connection, by clicking the project settings.

Then go to the service connections, create service connection. Choose Azure Resource Manager->Service principal 

thumbnail image 3 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

Select the subscription and resource group you would like to connect, give the connection a name, and save. Now we are ready to use this service connection in the pipeline.

thumbnail image 4 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

 

Create a pipeline
Now we can go back to the project and create a pipeline. Select "New pipeline"->"Azure Repos Git"-> "Existing Azure Pipelines YAML file"  from the project repo. 

Choose the "logic-app-pipeline-infra.yml" file in the repository. This pipeline put CI and CD together, including two stages, to create the infrastructures for Logic App:

 

Stage 1: Build

This stage will build and publish the artifact from path "Terraform/LAstandard.tf"  

thumbnail image 5 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

Stage 2: Deploy

This stage has the following steps:

1. An Azure CLI task to create the storage for terraform. This storage account is different than the Logic App hosting storage.  By default, Terraform stores state locally in a file named terraform.tfstate. With remote state, Terraform writes the state data to a remote data store. This task is commented out. If you already have a storage account to save the terraform files, it can be skipped.

thumbnail image 6 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

In my case I will use an existing storage account in my subscription for the Terraform data, the values are specified in the variables:

thumbnail image 7 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

2. Download the artifact

thumbnail image 8 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

3. A PowerShell script to get the storage key

thumbnail image 9 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

4. Replace the token in the LAstandard.tf file, started with "__"  in the terraform code. For example "__storagekey__". This task will replace the token value with the values got from the PowerShell task. 

thumbnail image 10 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

5. Install the latest version of Terraform. 

thumbnail image 11 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

6. Terraform init->plan->apply to initialize, plan, and deploy the Logic App using Terraform. 

thumbnail image 12 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

 

Run the pipeline
Once the yml file is added, you can replace the variable values with your own storage account information, and the environmentServiceNameAzureRM with your service connection name.  After the yml file is edited and saved, you can start to run the pipeline. 

 

1. First stage is to build the artifact, the artifact will be published to pipeline. 

thumbnail image 13 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

 

2. Second stage is to deploy the Logic App infrastructure. 

thumbnail image 14 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

In the Terraform plan step you can review the output to verify what are the resources going to be added, changed, or destroyed. 

thumbnail image 15 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

In the terraform apply step the resources will be created. 

thumbnail image 16 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

 

After the pipeline run successfully, you can view the resources which just deployed from the Azure portal.

thumbnail image 17 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

 

Deploy the workflow
Standard Logic App allows you to deploy the application code and infrastructure separately. In the Github repo I have also included a sample code for the workflow.

thumbnail image 18 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

 

You can deploy the workflow separately using the logic-app-pipeline-wf.yml with the zip deploy method. 

 

thumbnail image 19 of blog post titled 
	
	
	 
	
	
	
				
		
			
				
						
							Deploy Logic App Standard with Terraform and Azure DevOps pipelines
							
						
					
			
		
	
			
	
	
	
	
	

 

 

 

References:

https://www.azuredevopslabs.com/labs/vstsextend/terraform/#overview

https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/logic_app_standard
	
	
	
				
		
			
				
						
							
