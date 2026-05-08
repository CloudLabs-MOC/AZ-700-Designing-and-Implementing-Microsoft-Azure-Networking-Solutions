# Lab 7b: Create an Azure private endpoint using Azure PowerShell

## Lab Overview

In this lab, you will deploy a prerequisite web app to serve as the foundation for your tasks. Next, you will create a virtual network and a bastion host to securely manage your resources. You will then create a test virtual machine within this network. Following that, you will create a Private Endpoint to securely connect to your web app. You will configure the private DNS zone to ensure proper name resolution for the Private Endpoint. Finally, you will test the connectivity to the Private Endpoint to verify that everything is set up correctly. This setup ensures secure and efficient access to your web app within a private network environment.

## Lab Objectives

In this lab, you will complete the following tasks:

+ Task 1: Deploy the prerequisite web app
+ Task 2: Create a virtual network and bastion host
+ Task 3: Create a test virtual machine
+ Task 4: Create a Private Endpoint
+ Task 5: Configure the private DNS zone
+ Task 6: Test connectivity to the Private Endpoint

## Estimated time: 45 Minutes

## Architecture diagram

   ‎![](../media/az700-m7-unit6.png)

## Task 1: Deploy the web app

In this task, you'll create a Private Endpoint for an Azure Web App and deploy a virtual machine to test the private connection. You’ll use ARM templates to create the resources and a storage account for the web app deployment.

- An Azure Web App with a PremiumV2-tier or higher app service plan deployed in your Azure subscription.

1. Open the **Visual studio** from the desktop.

   ![](../media/e7bt1p1.png)

1. Select **File (1)** and then **Open file (2)**.

   ![](../media/azv-2.png)

1. Navigate to `C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M07` **(1)** then select **parameters.json (2)** and the **Open (3)**.

   ![](../media/azv3.png)

1. Select **Open**.

   ![](../media/azv4.png)

1. Find `"value": "GEN-UNIQUE"`, replace it with a unique web app name **webapp<inject key="DeploymentID" enableCopy="false"/>** and save the file.

   ![](../media/e7bt1p5.png)

1. Use **Ctrl+S** to save the file.   

1. On the Azure portal, select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

   ![](../media/unit6-image1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

   ![](../media/pwershell1.png)

1. On **Getting started** window choose **Mount storage account (1)** then under **Storage account subscription** select your available subscription from the dropdown **(2)** and click on **Apply (3)**.
   
     ![](../media/pwershell3.png)
   
1. Within the Mount storage account pane, select **I want to create a storage account (1)** and click **Next (2)**.

     ![](../media/pwershell4.png)

1. On the **Create a Storage account** page, provide the following details and then click **Create (6)**:

   - Subscription: Leave the default one **(1)**
   - Resource group: Select **CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/> (2)**
   - Region: Select **Region** **<inject key="Region" enableCopy="false"/> (3)**
   - Storage account name: Enter **blob<inject key="DeploymentID" enableCopy="false"/> (4)**
   - File share: Enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/> (5)**

     ![](../media/azv6.png)   

1. On the toolbar of the Cloud Shell pane, select the Select **Manage files (1)** icon, in the drop-down menu, select **Upload (2)**.

   ![](../media/mod2.3-t1p1.png)

1. Navigate to `C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M07` **(1)** then select the following files **template.json** and **parameters.json** **(2)** and then **Open (3)**.

   ![](../media/azv7.png)
   
1. Deploy the following ARM templates to create the PremiumV2-tier Azure Web App needed for this exercise:

   ```powershell
   $RGName = "CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile template.json -TemplateParameterFile parameters.json
   ```

    ![](../media/e7bt1p14.png)   

    >**Note**: If you receive an error (for example while looking into the Deployment status in the Portal) like "Website with given name GEN-UNIQUE already exists." please make sure to go to the step 1 mentioned above regarding editing the template.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="291360d1-df23-4be1-8de3-fd266feccd04" />

## Task 2: Create a virtual network and bastion host

In this task, you'll create a virtual network (VNet), a bastion host, and the required configurations to securely connect to a virtual machine for testing the Private Endpoint. You’ll use PowerShell to accomplish this.

Create a virtual network and bastion host with:

- New-AzVirtualNetwork

- New-AzPublicIpAddress

- New-AzBastion

   ```PowerShell
   ## Create backend subnet config. ##
   
   $subnetConfig = New-AzVirtualNetworkSubnetConfig -Name myBackendSubnet -AddressPrefix 10.0.0.0/24
   
   ## Create Azure Bastion subnet. ##
   
   $bastsubnetConfig = New-AzVirtualNetworkSubnetConfig -Name AzureBastionSubnet -AddressPrefix 10.0.1.0/24
   
   ## Create the virtual network. ##
   
   $parameters1 = @{
   
    Name = 'MyVNet'
   
    ResourceGroupName = 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>'
   
    Location = 'eastus'
   
    AddressPrefix = '10.0.0.0/16'
   
    Subnet = $subnetConfig, $bastsubnetConfig
   
   }
   
   $vnet = New-AzVirtualNetwork @parameters1
   
   ## Create public IP address for bastion host. ##
   
   $parameters2 = @{
   
    Name = 'myBastionIP'
   
    ResourceGroupName = 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>'
   
    Location = 'eastus'
   
    Sku = 'Standard'
   
    AllocationMethod = 'Static'
   
   }
   
   $publicip = New-AzPublicIpAddress @parameters2
   
   ## Create bastion host ##
   
   $parameters3 = @{
   
    ResourceGroupName = 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>'
   
    Name = 'myBastion'
   
    PublicIpAddress = $publicip
   
    VirtualNetwork = $vnet
   
   }
   
   New-AzBastion @parameters3
   ```

   ![](../media/e7bt2p1.png)

1. If the last command is still running, please click on **New session** from top in the terminal and proceed to the next task.  

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="24473272-9692-4d60-b444-dc4952a3be04" />

## Task 3: Create a test virtual machine

In this task, you will create a virtual machine (VM) to test the Private Endpoint by setting up the necessary configuration for networking, credentials, operating system, and image. You will use PowerShell commands to deploy the VM and configure its network interface.

Create the virtual machine with the following commands:

- Get-Credential (Note: You will be prompted to provide an Admin password.)

- New-AzNetworkInterface

- New-AzVM

- New-AzVMConfig

- Set-AzVMOperatingSystem

- Set-AzVMSourceImage

- Add-AzVMNetworkInterface

   ```PowerShell
   ## Set credentials for server admin and password. ##
   
   $cred = Get-Credential
   
   ## Command to get virtual network configuration. ##
   
   $vnet = Get-AzVirtualNetwork -Name myVNet -ResourceGroupName CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>
   
   ## Command to create network interface for VM ##
   
   $parameters1 = @{
   
    Name = 'myNicVM'
   
    ResourceGroupName = 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>'
   
    Location = 'eastus'
   
    Subnet = $vnet.Subnets[0]
   
   }
   
   $nicVM = New-AzNetworkInterface @parameters1
   
   ## Create a virtual machine configuration.##
   
   $parameters2 = @{
   
    VMName = 'myVM'
   
    VMSize = 'Standard_DS1_v2'
   
   }
   
   $parameters3 = @{
   
    ComputerName = 'myVM'
   
    Credential = $cred
   
   }
   
   $parameters4 = @{
   
    PublisherName = 'MicrosoftWindowsServer'
   
    Offer = 'WindowsServer'
   
    Skus = '2019-datacenter-gensecond'
   
    Version = 'latest'
   
   }
   
   $vmConfig = New-AzVMConfig @parameters2 | Set-AzVMOperatingSystem -Windows @parameters3 | Set-AzVMSourceImage @parameters4 | Add-AzVMNetworkInterface -Id $nicVM.Id
   
   ## Create the virtual machine ##
   
   New-AzVM -ResourceGroupName 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>' -Location 'eastus' -VM $vmConfig 
   
   
   ```

   ![](../media/l7-u6-1.png)     

   >**Note**: You will be prompted to provide UserName enter **TestUser** and Admin password enter **Pa55w.rd!!**.

   >**Note**: If you get a prompt related to NIC enter **Y**
  
   >**Note**: Azure provides an ephemeral IP for Azure Virtual Machines which aren't assigned a public IP address, or are in the backend pool of an internal Basic Azure Load Balancer. The ephemeral IP mechanism provides an outbound IP address that isn't configurable.  

   >**Note**: The ephemeral IP is disabled when a public IP address is assigned to the virtual machine or the virtual machine is placed in the backend pool of a Standard Load Balancer with or without outbound rules. If a Azure Virtual Network NAT gateway resource is assigned to the subnet of the virtual machine, the ephemeral IP is disabled.

   >**Note**: For more information on outbound connections in Azure, see Using Source Network Address Translation (SNAT) for outbound connections.


   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="b1deea5f-8396-49d6-9c3b-c0a0217677c8" />

## Task 4: Create a Private Endpoint

In this task, you will create a Private Endpoint to securely connect to the Azure Web App using Azure Private Link. You'll also configure the private endpoint connection to the web app and adjust the necessary network policies for the virtual network.

In this section, you'll create the Private Endpoint and connection using:

- New-AzPrivateLinkServiceConnection

- New-AzPrivateEndpoint

   ```PowerShell
   ## Place web app into variable. This assumes that only one web app exists in the resource group. ##
   
   $webapp = Get-AzWebApp -ResourceGroupName CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>
   
   ## Create Private Endpoint connection. ##
   
   $parameters1 = @{
   
    Name = 'myConnection'
   
    PrivateLinkServiceId = $webapp.ID
   
    GroupID = 'sites'
   
   }
   
   $privateEndpointConnection = New-AzPrivateLinkServiceConnection @parameters1
   
   ## Place virtual network into variable. ##
   
   $vnet = Get-AzVirtualNetwork -ResourceGroupName 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>' -Name 'myVNet'
   
   ## Disable private endpoint network policy ##
   
   $vnet.Subnets[0].PrivateEndpointNetworkPolicies = "Disabled"
   
   $vnet | Set-AzVirtualNetwork
   
   ## Create private endpoint
   
   $parameters2 = @{
   
    ResourceGroupName = 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>'
   
    Name = 'myPrivateEndpoint'
   
    Location = 'eastus'
   
    Subnet = $vnet.Subnets[0]
   
    PrivateLinkServiceConnection = $privateEndpointConnection
   
   }
   
   New-AzPrivateEndpoint @parameters2 
   ```

    ![](../media/e7bt4p1.png)     

## Task 5: Configure the private DNS zone

In this task, you'll configure the Private DNS Zone to enable DNS resolution for the Private Endpoint you created in the previous task. This will allow resources within your virtual network to resolve the private IP address associated with your Azure Web App.

In this section you'll create and configure the private DNS zone using:

- New-AzPrivateDnsZone

- New-AzPrivateDnsVirtualNetworkLink

- New-AzPrivateDnsZoneConfig

- New-AzPrivateDnsZoneGroup

   ```PowerShell
   ## Place virtual network into variable. ##
   
   $vnet = Get-AzVirtualNetwork -ResourceGroupName 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>' -Name 'myVNet'
   
   ## Create private dns zone. ##
   
   $parameters1 = @{
   
    ResourceGroupName = 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>'
   
    Name = 'privatelink.azurewebsites.net'
   
   }
   
   $zone = New-AzPrivateDnsZone @parameters1
   
   ## Create dns network link. ##
   
   $parameters2 = @{
   
    ResourceGroupName = 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>'
   
    ZoneName = 'privatelink.azurewebsites.net'
   
    Name = 'myLink'
   
    VirtualNetworkId = $vnet.Id
   
   }
   
   $link = New-AzPrivateDnsVirtualNetworkLink @parameters2
   
   ## Create DNS configuration ##
   
   $parameters3 = @{
   
    Name = 'privatelink.azurewebsites.net'
   
    PrivateDnsZoneId = $zone.ResourceId
   
   }
   
   $config = New-AzPrivateDnsZoneConfig @parameters3
   
   ## Create DNS zone group. ##
   
   $parameters4 = @{
   
    ResourceGroupName = 'CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>'
   
    PrivateEndpointName = 'myPrivateEndpoint'
   
    Name = 'myZoneGroup'
   
    PrivateDnsZoneConfig = $config
   
   }
   
   New-AzPrivateDnsZoneGroup @parameters4 
   ```

    ![](../media/e7bt5p1.png)      

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="a70480ef-c326-4973-959d-2245397125ef" />

## Task 6: Test connectivity to the Private Endpoint

In this task, you will use the virtual machine you created in the previous step to connect to the web app across the Private Endpoint.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Resource groups (1)**, and then select **Resource groups (2)** under services.

   ![](../media/l7-u6-2n.png)

1. Select **CreatePrivateEndpointQS-rg-<inject key="DeploymentID" enableCopy="false"/>**.

1. Select **myVM**.

   ![](../media/l7-u6-3.png)

1. On the overview page for **myVM**, select **Connect (1)**. From the dropdown select **Connect via Bastion (2)** button.

   ![](../media/l7-u6-4.png)

1. Enter the username **TestUser (1)** and password **Pa55w.rd!! (2)** that you entered during the virtual machine creation and then **Connect (3)**.

   ![](../media/azv15.png)

1. If you encounter any error like the below, please follow the below step.

   ![](../media/azv16.png)

1. If the popup blocker prevents the new window, click on the extension **(1)**, select the allow popup blocker **(2)** and click on **Done (3)**.

   ![](../media/bastionaz7001.png)

1. Select **Connect** again.   

1. Within **myVM**, right click on **Start (1)** and then open **Windows PowerShell (Admin) (2)** on the server after you connect.

   ![](../media/azv17.png)

1. Enter the below command, make sure to replace &lt;your-webapp-name&gt; with **webapp<inject key="DeploymentID" enableCopy="false"/>** the name of the web app you created in the previous steps.

      ```
      nslookup <your- webapp-name>.azurewebsites.net 
      ```

1. You'll receive a message similar to what is displayed below:

      ```
       Server: UnKnown
     
       Address: 168.63.129.16
     
       Non-authoritative answer:
     
       Name: mywebapp8675.privatelink.azurewebsites.net
     
       Address: 10.0.0.5
     
       Aliases: mywebapp8675.azurewebsites.net 
      ```  

      ![](../media/mod7.6-t6p1.png)

1. A private IP address of **10.0.0.5** is returned for the web app name. This address is in the subnet of the virtual network you created previously.

1. Close the connection to **myVM**.

## Key takeaways

+ Azure Private Link enables you to access Azure PaaS Services (for example, Azure Storage and SQL Database) and Azure hosted customer-owned/partner services over a private endpoint in your virtual network.
+ A private endpoint is a network interface that uses a private IP address from your virtual network. This network interface connects you privately and securely to a service that's powered by Azure Private Link. 

## Review

In this lab, you have completed:
+ Deployed the prerequisite web app
+ Created a virtual network and bastion host
+ Created a test virtual machine
+ Created a Private Endpoint
+ Configured the private DNS zone
+ Tested connectivity to the Private Endpoint

## You have successfully completed the lab.
