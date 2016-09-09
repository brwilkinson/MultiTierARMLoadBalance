# Key Features of this deployment

1. Deployment (uniqueID for each deployment) = "Prefix" + "Environment" + "DeploymentID"

	DeploymentID is an Integer = 101
	Prefix is a unique user code = BRW
	Environment = Dev,Test,Prod

	Deployment = BRWDEV101

	* This ensures that each time you deploy you will never have any naming conflicts.
	* All resources are easy to identify.

2. JSON Parameter Allowed Values ensure that we have a validated set of values for any parameter

		E.g. vmStorageAccountType = "Standard_LRS","Standard_ZRS","Standard_GRS","Standard_RAGRS","Premium_LRS"
		E.g. vmDomainName         = "Contoso.com","AlpineSkiHouse.com","Fabrikam.com","TreyResearch.net"
		E.g. vmWindowsOSVersion   = "2008-R2-SP1","2012-Datacenter","2012-R2-Datacenter","Windows-Server-Technical-Preview"

3. Dynamically create resource names based on the unique Deployment *Build your naming standards into this process
	
	The Subnet is named: __snBRWDEV101-01__
	
		"[concat('sn', variables('Deployment'),'-01')]"

	---
	
	The Storage account is named: __sabrwdev101__

		"[toLower(concat('sa', variables('Deployment')))]"

	---
	
	The virtual machine DC1 is named: __vmBRWDEV101DC1__

		"[concat('vm', variables('Deployment'),'DC1')]"
	

4. Function are used within the Template

	The Storage account is named: sabrwdev101 which is lower case, which is a requirement.

		"[toLower(concat('sa', variables('Deployment')))]"

	The public DNS is named: contosovmbrwdev101dc1, which is lower case, which is a requirement
	
		"DC1PublicDNSName": "[toLower(concat(variables('Domain'),variables('DC1vmName')))]"

	The full name will be: __contosovmbrwdevdc1.eastus.cloudapp.azure.com__, which has to be unique dns per region.

5. A subdeployment is called twice during the Deployment to adjust the DNS Servers setttings on the Subnet

	The public URL for the nested deployment template

		"uri": "[variables('dpSetvNetDNS')]",

		"dpSetvNetDNS": "https://raw.githubusercontent.com/brwilkinson/Azure/master/dpSetvNetDNS.json",
	
	That is publicly accessible, however you can use private storage and add the URL that includes the SAS token

	Shared Access Signature token is included as part of the URL

	This token, can be stored in the KeyVault and retrieved as a parameter from the KeyVault.
	
	The Nested deployment takes some parameters:

	1. The DNS Servers
	2. The Deployment, since it uses this name to update the correct VNet/Subnet based on the naming standard

		E.g. __vnBRWDev101__/__snBRWDev101-01__

6. The AzureRM Modules that are required for this project are made available via the Azure SDK

	If you execute the following:
	
		Get-Module -ListAvailable -name azurerm*

	The modules should all be found under the following directory:

	Directory: __C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ResourceManager\AzureResourceManager__

	If you have any Azure Modules under the following directory, they have likely been manually installed
	or installed via the PowerShell Gallery. This can cause versioning issues and you should remove those modules.

	Directory: __C:\program files\WindowsPowershell\Modules__

	i.e. Use either the Azure SDK OR the PowerShell gallery, however not both.

7. This deployment script will download your RDP Files after the Deployment Completes

	Just set the directory within the file: Deploy-AzureResourceGroup.ps1

		[string] $RDPFileDirectory = "$home\OneDrive\RDP\Azure"

8. Outputs, provides the Public DNS Name for the VM's

9. You can connect to any Virtual Machine in Azure via it's Public IP Address or DNS name with MSTSC/RDP

10. See the sample scripts to create a Certificate and also upload it to the KeyVault

	These create the certs and upload them to keyvault

	They also create secrets and upload them to KeyVault

	Optionally you can access the KeyVault at deploytime using the Parameter dialog to manage and use secrets via KeyVault

	
	

	

