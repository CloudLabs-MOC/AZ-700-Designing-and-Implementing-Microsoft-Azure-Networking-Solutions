# Lab 7a: Restrict network access to PaaS resources with virtual network service endpoints

## Lab Overview 
In this lab, you will learn how to restrict network access to Azure PaaS resources using virtual network service endpoints. Service endpoints allow you to secure access to Azure services like Azure Storage, making sure that traffic to these services stays within your private network and does not go over the public internet. You will configure network security groups (NSGs) to control inbound and outbound traffic, and you’ll test connectivity by deploying virtual machines (VMs) and verifying access to the resources.

## Lab Objectives
  
In this lab, you will complete the following tasks:

+ Task 1: Create a virtual network
+ Task 2: Enable a service endpoint
+ Task 3: Restrict network access for a subnet
+ Task 4: Add additional outbound rules 
+ Task 5: Allow access for RDP connections
+ Task 6: Restrict network access to a resource
+ Task 7: Create a file share in the storage account
+ Task 8: Restrict network access to a subnet
+ Task 9: Create virtual machines
+ Task 10: Confirm access to storage account
+ Task 11: Confirm access is denied to storage account

## Estimated time: 35 Minutes

## Architecture Diagram

  ‎![](../media/az700-m7-unit5.png)

## Task 1: Create a virtual network

In this task, you will create a virtual network and a subnet.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual network (1)**, and then select **Virtual network (2)** under services.

    ![](../media/srchvn.png)

1. Select **+ Create**.

   ![](../media/azn119.png)

1. On the **Create virtual network** blade specify the following information and then select **Address space (5)**:
 
   | **Setting**    | **Value**                                     |
   | -------------- | --------------------------------------------- |
   | Subscription   | Select your subscription **(1)**                     |
   | Resource group | Select **myResourceGroup (2)**                               |
   | Name           | **CoreServicesVNet (3)**                              |
   | Region         | Select **<inject key="Region" enableCopy="false"/> (4)**                     |

    ![](../media/azv21.png)

1. On the **Address space** tab, select **default** to change the subnet name. 

    ![](../media/dflt.png)

1. Specify the following values and then **Save (4)**:    

   | **Setting**          | **Value**   |
   | -------------------- | ----------- |
   | Subnet Name          | Public **(1)** |
   | Starting address     | 10.0.0.0 **(2)** |
   | Subnet size          | /24 **(3)**  |

    ![](../media/azv25.png)
   
1. Select **Review + Create**. Once the resource is validated select **Create**.

      ![](../media/azn120.png)
   
1. Click on **Go to resource**.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="b5846aa2-f0da-4c3f-983d-c6a572536624" />

## Task 2: Enable a service endpoint

In this task, you will add a subnet to the CoreServicesVNet and enable a service endpoint for Microsoft.Storage.

1. Add a subnet to the virtual network. Under **Settings**, select **Subnets (1)**, and then select **+ Subnet (2)**.

    ![](../media/azv22.png)  

1. Under **Add subnet**, select or enter the following information and select **Add (4)**.

   | **Setting**                 | **Value**                    |
   | --------------------------- | ---------------------------- |
   | Name                        | **Private (1)**               |
   | Subnet address range        | **10.0.1.0/24 (2)**           |
   | Service endpoints: Services | Select **Microsoft.Storage (3)** |

    ![](../media/pvt.png)

    ![](../media/pvtadd.png)

1. You should now have two subnets configured:

   ![](../media/L7U5-2.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="b38b6528-13cf-4e7f-8026-b48a4c05044f" />

## Task 3: Restrict network access for a subnet

In this task, you will create a Network Security Group (NSG) to restrict network access for a subnet by allowing only specific communication, such as outbound traffic to Azure Storage.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **security group (1)** and select **Network Security groups (2)**. 

    ![](../media/azv23.png)

1. In Network security groups, select **+ Create**. 

   ![](../media/azn121.png)

1. Enter or select, the following information and then click on **Review+Create (5)**

    | **Setting**    | **Value**                                                    |
    | -------------- | ------------------------------------------------------------ |
    | Subscription   | Select your subscription **(1)**                                   |
    | Resource group | Select **myResourceGroup (2)**                                              |
    | Name           | **ContosoPrivateNSG (3)**                                            |
    | Region         | Select **<inject key="Region" enableCopy="false"/> (4)**                                           |

    ![](../media/azv26.png)

1. Once the resource is validated select **Create**.

   ![](../media/azn122.png)

1. After the ContosoPrivateNSG network security group is created, select **Go to resource**.

   ![](../media/azn123.png)

1. On the **ContosoPrivateNSG** page, from the left navigation menu, under **Settings** section, select **Outbound security rules (1)** and then **+Add (2)**.

   ![](../media/azv27.png)

1. Create a rule that allows outbound communication to the Azure Storage service. Enter, or select, the following information:
  
    | **Setting**             | **Value**                 |
    | ----------------------- | ------------------------- |
    | Source                  | Select **Service Tag (1)**    |
    | Source service tag      | Select **VirtualNetwork (2)** |
    | Source port ranges      | * **(3)**                      |
    | Destination             | Select **Service Tag (4)**    |
    | Destination service tag | Select **Storage (5)**        |
    | Service                 | Custom **(6)**                   |
    | Destination port ranges | *  **(7)**                     |
    | Protocol                | Any **(8)**                   |
    | Action                  | Allow **(9)**                 |
    | Priority                | 100 **(10)**               |
    | Name                    | Allow-Storage-All **(11)**     |

    ![](../media/unit59.png)

    ![](../media/unit60.png)

1. Select **Add**.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="c4efff35-0afb-4294-aa9e-fdeeb9462024" />

## Task 4: Add additional outbound rules 

In this task, you will add an outbound rule to deny communication to the internet for the subnet, overriding the default rule that allows outbound internet communication. 

1. Select **+ Add** under **Outbound security rules**.

   ![](../media/azv27.png)

1. Enter, or select, the following information and then **Add (12)**:
 
   | **Setting**             | **Value**                 |
   | ----------------------- | ------------------------- |
   | Source                  | Select **Service Tag (1)**    |
   | Source service tag      | Select **VirtualNetwork (2)** |
   | Source port ranges      | `*` **(3)**                         |
   | Destination             | Select **Service Tag (4)**    |
   | Destination service tag | Select **Internet (5)**       |
   | Service                 | **Custom  (6)**                   |
   | Destination port ranges | `*` **(7)**                         |
   | Protocol                | **Any (8)**                       |
   | Action                  | **Deny (9)**                      |
   | Priority                | **110  (10)**                      |
   | Name                    | **Deny-Internet-All (11)**         |

   ![](../media/azv28.png)

   ![](../media/azv29.png)   

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="8e8574e1-0474-4f42-a2a6-7ccdef62c692" />

## Task 5: Allow access for RDP connections

In this task, you will create an inbound security rule to allow Remote Desktop Protocol (RDP) traffic (port 3389) to the subnet from anywhere, allowing remote management of resources.

1. On **ContosoPrivateNSG | Outbound security rules**, from left navigation menu, under **Settings**, select **Inbound security rules (1)** and then click on **+Add (2)**.

   ![](../media/azv30.png)

1. In Add inbound security rule, enter the following values and then click on **Add (11)**:

   | **Setting**             | **Value**                 |
   | ----------------------- | ------------------------- |
   | Source                  | Any **(1)**                      |
   | Source port ranges      | * **(2)**                        |
   | Destination             | Select **Service Tag (3)**    |
   | Destination service tag | Select **VirtualNetwork (4)** |
   | Service                 | Custom **(5)**                  |
   | Destination port ranges | 3389 **(6)**                    |
   | Protocol                | Any **(7)**                   |
   | Action                  | Allow **(8)**                   |
   | Priority                | 120  **(9)**                    |
   | Name                    | Allow-RDP-All **(10)**           |

   ![](../media/azv31.png)

   ![](../media/azv32.png)
   
    > **Warning**: RDP port 3389 is exposed to the Internet. This is only recommended for testing. For production environments, we recommend using a VPN or private connection.

1. From the left navigation menu, under **Settings**, select **Subnets (1)**.

   - Select **+ Associate (2)**

   - Under **Associate subnet**, from the **Virtual network** dropdown select **CoreServicesVNet (3)**.

   - Under **Subnet**, select **Private (4)**, and then select **OK (5)**.

    ![](../media/azv33.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="86eeda16-df94-42ca-b4e3-ff17ba6aae26" />

## Task 6: Restrict network access to a resource

In this task, you will create an Azure Storage account and restrict network access to it by configuring network rules and service endpoints.

The steps necessary to restrict network access to resources created through Azure services enabled for service endpoints varies across services. See the documentation for individual services for specific steps for each service. The remainder of this exercise includes steps to restrict network access for an Azure Storage account, as an example.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Storage Account (1)**, and then select **Storage accounts (2)** under services.

    ![](../media/strg.png)

1. Select **+ Create**.

   ![](../media/azn124.png)

1. Enter, or select, the following information and accept the remaining defaults and then click on **Review+Create (7)**:

    | **Setting**    | **Value**                                                    |
    | -------------- | ------------------------------------------------------------ |
    | Subscription   | **Select your subscription (1)**                                    |
    | Resource group | **myResourceGroup (2)**                                              |
    | Name           | Enter **contosostorage<inject key="DeploymentID" enableCopy="false"/> (4)** |
    | Region         | **<inject key="Region" enableCopy="false"/> (3)** 
    | Performance    | **Standard (5)**                     |                                              |
    | Redundancy    | **Locally-redundant storage (LRS) (6)**                              |

    ![](../media/azv35.png)

1. Once the resource is validated select **Create**.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="1e92fc4e-6f33-47d4-ad47-c22395472a07" />

## Task 7: Create a file share in the storage account

In this task, your creating a file share in the storage account.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Storage Account (1)**, and then select **Storage accounts (2)** under services.

    ![](../media/strg.png)

1. From the list select **contosostorage<inject key="DeploymentID" enableCopy="false"/>** storage account.

    ![](../media/conslct.png)

1. From left navigation pane of storage account under **Data storage**, select **File shares (1)** and then select **+ File share (2)**.

    ![](../media/azv36.png)

1. Enter **marketing (1)**, under **Name** and then select **Next: Backup > (2)**

    ![](../media/azv37.png)

1. Unselect the **Enable backup (1)** and then select **Review + create (2)**

    ![](../media/azv38.png)

1. Once the resource is validated select **Create**.

    ![](../media/azv55.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="c814f060-1d78-451a-9e4e-344c0e35e582" />

## Task 8: Restrict network access to a subnet

In this task, you will configure the storage account to restrict network access by allowing connections only from the Private subnet in the CoreServicesVNet virtual network.

1. Navigate to **contosostorage<inject key="DeploymentID" enableCopy="false"/>** storage account blade.

1. From the left navigation pane, under **Security + networking (1)** section, select **Networking (2)**.

   - Select **Enabled from all networks (3)**

      ![](../media/enb.png)

   - On the **Public network access** page, under **Public network access scope** select **Enable from selected networks (1)**.

   - Then select **+ Add a virtual network (2)** drop down then choose **Add existing virtual network (3)**.

     ![](../media/kernel-image77.png)
       
1. Under **Add networks**, select the following values and select **Add (4)**. 
   
   | **Setting**      | **Value**                    |
   | ---------------- | ---------------------------- |
   | Subscription     | Select your subscription **(1)**    |
   | Virtual networks | Select **CoreServicesVNet (2)**  |
   | Subnets          | Select **Private (3)**.          |
   |||
   
   ![Graphical user interface, application Description automatically generated](../media/azv41.png)

1. Select **Save** from the bottom left corner of the page.

   ![](../media/savet8.png) 

1. Under **Security + networking** for the storage account, select **Access keys (1)**.

   - Select **Show Keys**. make a note the **Key (2)** value, as you'll have to manually enter it in a later step when mapping the file share to a drive letter in a VM.

     ![](../media/azv43.png)   

## Task 9: Create virtual machines

In this task, you'll create two virtual machines (VMs) to test network access to a storage account, and deploy them to different subnets.

1. On the Azure portal, select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

   ![](../media/unit6-image1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

   ![](../media/pwershell1.png)

1. On **Getting started** window choose **Mount storage account (1)** then under **Storage account subscription (2)** select your available subscription from the dropdown and click on **Apply (3)**.
   
     ![](../media/msa.png)
   
1. Within the Mount storage account pane, select **I want to create a storage account (1)** and click **Next (2)**.

     ![](../media/pwershell4.png)

1. On the **Create a Storage account** page, provide the following details and then **Create (6)**: 

   - Subscription: Leave the default one **(1)**

   - Please make sure you have selected your resource group **myResourceGroup (2)**
   - Select **Region** as **<inject key="Region" enableCopy="false"/> (3)**

   - Enter **blob<inject key="DeploymentID" enableCopy="false"/> (4)** for the **Storage account name**
   
   - Enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/> (5)** for the  **File share** 

     ![](../media/azv-44.png)   

1. On the toolbar of the Cloud Shell pane, select the Select **Manage files (1)** icon, in the drop-down menu, select **Upload (2)**.

     ![](../media/pwershell2.png)

1. Navigate to `C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M07` **(1)** then select the following files **VMs.json** and **VMs.parameters.json** **(2)** and then **Open (3)**. 

     ![](../media/azv45.png)
   
1. Deploy the following ARM templates to create the VMs needed for this exercise:

   ```powershell
   $RGName = "myResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile VMs.json -TemplateParameterFile VMs.parameters.json
   ```

1. You will be prompted to provide an Admin password, enter `Pa55w.rd!!`

1. When the deployment is complete, go to the Azure portal home page, and then select **Virtual Machines** to find the newly created virtual machines.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="52806dfa-dd22-4a6f-8fb7-935bf4e6c237" />

## Task 10: Confirm access to storage account

In this task, you'll connect to the ContosoPrivate VM, map the Azure file share to drive Z using PowerShell, and confirm there is no outbound connectivity to the internet.

1. Navigate to the **Virtual Machine** blade, select **ContosoPrivate** VM.

   ![](../media/cpr.png)

1. On **ContosoPrivate** Virtual machine page, click on **Connect (1)** and from the dropdown click on **Connect (2)** again.

   ![](../media/azv46.png)

1. On **ContosoPrivate | Connect** page under **Native RDP**, select **Download RDP file**. 

   ![](../media/drdp.png)

1. Ignore the warning and click on **Keep**.

1. Click on **Open file** to open the downloaded RDP file. 

1. Select **Connect**.

1. Select **More choices**.

1. Select **Use a different account**.
   
1. Enter the user name `.\TestUser` **(1)** and password **Pa55w.rd!!** **(2)** and then **OK (3)**.

   ![](../media/azv47.png)

1. You may receive a certificate warning during the sign-in process. If you receive the warning, select **Yes** to proceed with the connection.

1. On the **ContosoPrivate** VM, from the **Start (1)** menu, right click on the **Windows Powershell (2)** tile and then click on **More (3)** and select **Run as administrator (4)**.

   ![](../media/psad.png)

    >**Note:** In the pop-up related to network visibility click **No**.

1. On the ContosoPrivate VM, map the Azure file share to drive Z using PowerShell. Before running the commands that follow, replace **[storage-account-key]** that you noted in eariler task.

     ```azurecli
     $acctKey = ConvertTo-SecureString -String "[storage-account-key]" -AsPlainText -Force
     
     $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\contosostorage<inject key="DeploymentID" enableCopy="false"/>", $acctKey
     
     New-PSDrive -Name Z -PSProvider FileSystem -Root "\\contosostorage<inject key="DeploymentID" enableCopy="false"/>.file.core.windows.net\marketing" -Credential $credential
     
     ```

   >**Note**: The Azure file share successfully mapped to the Z drive.

   ![](../media/unit69.png)

1. Confirm that the VM has no outbound connectivity to the internet by running the command:

   **`ping bing.com`**

   ![](../media/unit691.png)
   
   >**Note**: You receive no replies because the network security group associated to the Private subnet does not allow outbound access to the internet.

1. Close the remote desktop session to the ContosoPrivate VM.

## Task 11: Confirm access is denied to storage account

In this task, you'll confirm that the ContosoPublic VM doesn't have access to the storage account because it's deployed in the Public subnet, which doesn't have the required service endpoint enabled.

1. Navigate to the **Azure portal**, and select the **Virtal machines**.

1. Select **ContosoPublic** Virtal machine.

   ![](../media/cpb.png)

1. Complete steps 1-13 of previous task to get Confirm access to storage account task for the ContosoPublic VM.  
     
1. ‎After a short wait, you receive a New-PSDrive : Access is denied error. Access is denied because the ContosoPublic VM is deployed in the Public subnet. The Public subnet does not have a service endpoint enabled for Azure Storage. The storage account only allows network access from the Private subnet, not the Public subnet.

   ![](../media/L7U5-4.png)

1. Confirm that the public VM does have outbound connectivity to the internet from a command prompt:

   **`ping bing.com`**

   ![](../media/unit693.png)

1. Close the remote desktop session to the ContosoPublic VM.

1. From your computer, browse to the Azure portal.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Storage Account (1)**, and then select **Storage accounts (2)** under services.

    ![](../media/strg.png)

1. Select **contosostorage<inject key="DeploymentID" enableCopy="false"/>**.

1. From left navigation pane of storage account under **Data storage**, select **File shares (1)** and select the **marketing (2)** file share.

   ![](../media/azv50.png)

1. On the **marketing** blade, from the left navigation menu, click on **Browse**.

   ![](../media/fsbr.png)

1. You receive the error shown in the following screenshot:

   ![](../media/azv52.png)

   **Note**:  Access is denied, because your computer is not in the Private subnet of the CoreServicesVNet virtual network.

   **Warning**: Prior to continuing you should remove all resources used for this lab. To do this On the Azure portal select Resource groups. Select any resources groups you have created. On the resource group blade select Delete Resource group, enter the Resource Group Name and select Delete. Repeat the process for any additional Resource Groups you may have created. Failure to do this may cause issues with other labs.

## Key takeaways
+ Virtual network service endpoints extend your private address space in Azure by providing a direct connection to your Azure services.
+ Service endpoints let you secure your Azure resources to only your virtual network. Service traffic will remain on the Azure backbone, and doesn't go out to the internet.
+ Azure service endpoints are available for many services, such as: Azure Storage, Azure SQL Database, and Azure Cosmos DB.
+ Virtual network service endpoints are not, by default, accessible from on-premises networks. To access resources from an on-premises network, use NAT IPs.

## Review
In this lab, you have completed:

+ Created a virtual network
+ Enabled a service endpoint
+ Restricted network access for a subnet
+ Added additional outbound rules 
+ Allowed access for RDP connections
+ Restricted network access to a resource
+ Created a file share in the storage account
+ Restricted network access to a subnet
+ Created virtual machines
+ Confirmed access to storage account
+ Confirmed access is denied to storage account

## You have successfully completed the lab.
