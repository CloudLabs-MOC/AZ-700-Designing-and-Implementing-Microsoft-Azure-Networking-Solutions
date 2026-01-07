# Lab 1b: Configure DNS settings in Azure

## Lab Overview 
This lab will guide you through configuring DNS name resolution in Azure for Contoso Ltd. The objective is to set up a Private DNS Zone, link it to multiple virtual networks (VNets), and test the configuration by creating virtual machines (VMs). You will ensure that your Azure resources can correctly resolve DNS names within a private network using the created DNS zone.

## Lab Objectives

In this lab, you will complete the following tasks:

+ Task 1: Create a private DNS Zone
+ Task 2: Link subnet for auto registration
+ Task 3: Create Virtual Machines to test the configuration
+ Task 4: Verify records are present in the DNS zone

## Estimated Duration: 25 Minutes

## Architecture Diagram

   ‎![](../media/az700-m1-unit6.png)

### Task 1: Create a private DNS Zone

In this task, you'll create a Private DNS Zone in Azure. This involves setting up a DNS zone that will be used to manage and resolve DNS records for resources within a private virtual network.

1. On **Azure Portal** page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Private DNS zones (1)**, and then select **Private DNS zones (2)** under services.

    ‎![](../media/l1-u6-1.png)

1. In **Private DNS zones** page, select **+ Create**.

1. On **Basics** tab, use the information in the following table to create the private DNS zone.

   | **Tab**         | **Option**                             | **Value**            |
   | --------------- | -------------------------------------- | -------------------- |
   | Basics          | Resource group                         | **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> (1)** |
   |                 | Name                                   | **Contoso.com (2)**      | 

1. Select **Review create (3)** and **create**

    ![](../media/l1-u6-2.png)

   >**Note:** Please Wait until the deployment is completed, and then select **Go to resource**.

1. Verify that the zone has been created by clicking on the **Go to resource**.

### Task 2: Link subnet for auto registration

In this task, you're linking multiple virtual networks (VNets) to the Contoso.com Private DNS Zone. This allows the virtual networks to use the DNS zone for name resolution.

1. On **Contoso.com** Private DNS zone page, under **DNS Management** section, select **Virtual Network Links (1)** and on **Contoso.com | Virtual Network Links** page, select **+ Add (2)**.

   ![](../media/unit6-links-1.png)

1. Use the information in the following table to add the virtual network link.

   | **Option**                          | **Value**                               |
   | ----------------------------------- | --------------------------------------- |
   | Link name                           | **CoreServicesVnetLink (2)**                    |
   | Subscription                        | **No changes required**                     |
   | Virtual Network                     | Select **CoreServicesVnet (ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>) (2)** |
   | Enable auto registration            | **Selected (3)**                                |

1. Review your settings and select **Create (4)**.

    ![](../media/l1-u6-3.png)

    >**Note:** Please Wait for 2 mins to successfully create virtual network link.
   
1. Select **Refresh**.

1. Verify that the CoreServicesVnetLink has been created, and that auto-registration is enabled.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="0403a166-bc46-43a3-bb84-8f28acc59cc9" />

###  Task 3: Create Virtual Machines to test the configuration

In this task, you'll be creating two test virtual machines (VMs) to test the Private DNS zone configuration that you've set up in previous tasks. These VMs will be used to ensure that your DNS zone and network links are correctly configured for name resolution within the virtual networks. 

1. On the Azure portal, select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

      ![](../media/unit6-image1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

     ![](../media/pwershell1.png)

1. On **Getting started** window choose **Mount storage account (1)** then under **Storage account subscription (2)** select your available subscription from the dropdown and click on **Apply (3)**.
   
     ![](../media/pwershell3.png)
   
1. Within the Mount storage account pane, select **I want to create a storage account (1)** and click **Next (2)**.

     ![](../media/pwershell4.png)
   
1. Please make sure you have selected your resource group **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>**.

1. Select the Region **<inject key="Region" enableCopy="false"/>**.

1. Enter **blob<inject key="DeploymentID" enableCopy="false"/>** For the **Storage account name**.

1. Enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/>** For the  **File share** , then click on **Create**.

      ![](../media/pwershell5.png)

1. On the toolbar of the Cloud Shell pane, select the Select **Manage files (1)** icon, in the drop-down menu, select **Upload (2)**.

      ![](../media/pwershell2.png)

1. Upload the following files **azuredeploy.json** and **azuredeploy.parameters.json** into the Cloud Shell home directory one by one from the source folder **C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M01**.

1. Deploy the following ARM templates to create the VMs needed for this exercise:

      ```powershell
      $RGName = "ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>"
      
      New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
      ```

      ![](../media/l1-u6-4.png)

      >**Note**: You will be prompted to provide an Admin password enter **Pa55w.rd!!**.
  
1. When the deployment is complete, go to the Azure portal home page, and then select **Virtual Machines**.

1. Verify that both **virtual machines** have been created.

      ![](../media/l1-u6-5.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="05b159b3-b8a6-4fb9-b554-eb1cc0ff7960" />

### Task 4: Verify records are present in the DNS zone

In this task, you'll verify that the DNS records for the virtual machines (VMs) are present and correctly configured in the Private DNS Zone you created earlier.  

1. On the Azure Portal home page, select **Private DNS zones**.

1. On Private DNS zones, select **contoso.com**.

1. Click on **Recordsets** under **DNS Management** and Verify that host (A) records are listed for both VMs, as shown:

   ![](../media/unit6-image-5.png)

1. Make a note of the names and IP addresses of the VMs.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="f9b42843-78fa-4885-b912-c7be860ce3f5" />

### Task 05: Connect to the Test VMs using RDP

In this task, you'll be testing the connectivity to the virtual machines (VMs) you created earlier, ensuring that DNS name resolution and Remote Desktop Protocol (RDP) are working as expected.

1. On the Azure Portal home page, select **Virtual Machines**.

1. Select **testvm1**.

1. On TestVM1, select **Connect (1)** then select **Connect (2)** from the drop-down. 

      ![](../media/l1-u6-6.png)

1. On **testvm1 | Connect** page, under **Native RDP** window select and **Download RDP file**. 

      ![](../media/l1-u6-7.png)

1. Save the RDP file to your desktop. If any warning pops up in "edge downloads" select **Keep**.

1. Follow the same steps for **testvm2**

1. Connect to testvm1 using the RDP file, and the username **TestUser** and the password you provided during deployment **Pa55w.rd!!**.

1. Connect to testvm2 using the RDP file, and the username **TestUser** and the password you provided during deployment **Pa55w.rd!!**.

1. On TestVM1, open a command prompt and enter the command **ipconfig /all**.

1. Verify that the IP address is the same as the one you noted in the DNS zone.

1. Enter the command **ping testvm2.contoso.com**.

1. Verify that the FQDN resolves to the IP address that you noted in the Private DNS zone. The ping itself will timeout because of the Windows Firewall that is enabled on the VMs.

1. Alternatively, you can enter the command **nslookup testvm2.contoso.com** and verify that you receive a successful name resolution record for VM2.

    >**Note:** This command is successful, and demonstrates private zone name resolution. 

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 

+ Azure DNS is a cloud service that allows you to host and manage domain name system (DNS) domains, also known as DNS zones. 
+ Azure DNS public zones host domain name zone data for records that you intend to be resolved by any host on the internet.
+ Azure Private DNS zones allow you to configure a private DNS zone namespace for private Azure resources.
+ A DNS zone is a collection of DNS records. DNS records provide information about the domain.

## Review

In this lab, you have completed:

+ Creating a private DNS Zone
+ Linking subnet for auto registration
+ Creating Virtual Machines to test the configuration
+ Verifying records are present in the DNS zone

## You have successfully completed the lab.
