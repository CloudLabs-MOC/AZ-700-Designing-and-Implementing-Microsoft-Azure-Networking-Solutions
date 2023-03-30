# M01-Unit 8 Connect two Azure Virtual Networks using global virtual network peering

## Exercise scenario 
In this unit, you will configure connectivity between the CoreServicesVnet and the ManufacturingVnet by adding peerings to allow traffic flow. 

In this unit, you will:

+ Task 1: Create the Contoso resource group
+ Task 2: Create the CoreServicesVnet virtual network and subnet
+ Task 3: Create the ManufacturingVnet virtual network and subnets
+ Task 4: Create the ResearchVnet virtual network and subnets
+ Task 5: Create a private DNS Zone
+ Task 6: Link subnet for auto registration
+ Task 7: Create Virtual Machines to test the configuration
+ Task 8: Create a Virtual Machine to test the configuration
+ Task 9: Test the connection between the VMs
+ Task 10: Create VNet peerings between CoreServicesVnet and ManufacturingVnet
+ Task 11: Test the connection between the VMs

## Task 1: Create the Contoso resource group

1. Go to [Azure portal](https://portal.azure.com/).

2. On the home page, under **Azure services**, select **Resource groups**.  

3. In the Resource groups, select **+ Create**.

4. Use the information in the following table to create the resource group.

| **Tab**         | **Option**                                 | **Value**            |
| --------------- | ------------------------------------------ | -------------------- |
| Basics          | Resource group                             | ContosoResourceGroup-<inject key="DeploymentID" enableCopy="true"/> |
|                 | Region                                     | (US) East US         |
| Tags            | No changes required                        |                      |
| Review + create | Review your settings and select **Create** |                      |


5. In Resource groups, verify that **ContosoResourceGroup** appears in the list.


## Task 2: Create the CoreServicesVnet virtual network and subnet

1. On the Azure portal home page, navigate to the Global Search bar and search **Virtual Networks** and select virtual networks under services.  ![Azure portal home page Global Search bar results for virtual network.](../media/global-search-bar.png)
2. Select **Create** on the Virtual networks page.  ![Create a virtual network wizard.](../media/create-virtual-network.png)
3. Use the information in the following table to create the CoreServicesVnet virtual network.  
   ‎Remove or overwrite the default IP Address space![ip address configuration for azure virtual network deployment ](../media/default-vnet-ip-address-range-annotated.png)

 

| **Tab**      | **Option**         | **Value**            |
| ------------ | ------------------ | -------------------- |
| Basics       | Resource Group     | ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> |
|              | Name               | CoreServicesVnet     |
|              | Region             | (US) East US         |
| IP Addresses | IPv4 address space | 10.20.0.0/16         |

 4. Use the information in the following table to create the CoreServicesVnet subnets.

 5. To begin creating each subnet, select **+ Add subnet**. To finish creating each subnet, select **Add**.

| **Subnet**             | **Option**           | **Value**              |
| ---------------------- | -------------------- | ---------------------- |
| DatabaseSubnet         | Subnet name          | DatabaseSubnet         |
|                        | Subnet address range | 10.20.20.0/24          |

6. To finish creating the CoreServicesVnet and its associated subnets, select **Review + create**.

7. Verify your configuration passed validation, and then select **Create**.
 
8. Repeat steps 1 -8 for each VNet based on the tables below.

## Task 3: Create the ManufacturingVnet virtual network and subnets


| **Tab**      | **Option**         | **Value**             |
| ------------ | ------------------ | --------------------- |
| Basics       | Resource Group     | ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>  |
|              | Name               | ManufacturingVnet     |
|              | Region             | (Europe) West Europe  |
| IP Addresses | IPv4 address space | 10.30.0.0/16          |



| **Subnet**                | **Option**           | **Value**                 |
| ------------------------- | -------------------- | ------------------------- |
| ManufacturingSystemSubnet | Subnet name          | ManufacturingSystemSubnet |
|                           | Subnet address range | 10.30.10.0/24             |

## Task 4: Create the ResearchVnet virtual network and subnets


| **Tab**      | **Option**         | **Value**            |
| ------------ | ------------------ | -------------------- |
| Basics       | Resource Group     | ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> |
|              | Name               | ResearchVnet         |
|              | Region             | Southeast Asia       |
| IP Addresses | IPv4 address space | 10.40.0.0/16         |

| **Subnet**           | **Option**           | **Value**            |
| -------------------- | -------------------- | -------------------- |
| ResearchSystemSubnet | Subnet name          | ResearchSystemSubnet |
|                      | Subnet address range | 10.40.0.0/24         |


## Task 5: Create a private DNS Zone

1. Go to [Azure Portal](https://portal.azure.com/).

2. On the Azure home page, in the search bar, type dns, and then select **Private DNS zones**.  
   ‎![Azure Portal home page with dns search.](../media/create-private-dns-zone.png)

3. In Private DNS zones, select **+ Create**.

4. Use the information in the following table to create the private DNS zone.

| **Tab**         | **Option**                             | **Value**            |
| --------------- | -------------------------------------- | -------------------- |
| Basics          | Resource group                         | ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> |
|                 | Name                                   | Contoso.com          |
| Tags            | No changes required                    |                      |
| Review + create | Review your settings and select Create |                      |

   ‎![](../media/vn9.png)

5. Wait until the deployment is complete, and then select **Go to resource**.

6. Verify that the zone has been created.

## Task 6: Link subnet for auto registration

1. On the Azure home page, in the search bar, type dns, and then select Private DNS zones then select newly created **Contoso.com**. In Contoso.com, under **Settings**, select **Virtual network links**.

2. On Contoso.com | Virtual network links, select **+ Add**.

![contoso.com | Virtual links with + Add highlighted.](../media/add-network-link-dns.png)

3. Use the information in the following table to add the virtual network link.

| **Option**                          | **Value**                               |
| ----------------------------------- | --------------------------------------- |
| Link name                           | CoreServicesVnetLink                    |
| Subscription                        | No changes required                     |
| Virtual Network                     | CoreServicesVnet (ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>) |
| Enable auto registration            | Selected                                |
| Review your settings and select OK. |                                         |

   ‎![](../media/vn10.png)

4. Select **Refresh**.

5. Verify that the CoreServicesVnetLink has been created, and that auto-registration is enabled.

6. Repeat steps 2 - 5 for the ManufacturingVnet, using the information in the following table: 

| **Option**                          | **Value**                                |
| ----------------------------------- | ---------------------------------------- |
| Link name                           | ManufacturingVnetLink                    |
| Subscription                        | No changes required                      |
| Virtual Network                     | ManufacturingVnet (ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>) |
| Enable auto registration            | Selected                                 |
| Review your settings and select OK. |                                          |


7. Select **Refresh**.

8. Verify that the ManufacturingVnetLink has been created, and that auto-registration is enabled.

9. Repeat steps 2 - 5 for the ResearchVnet, using the information in the following table: 

| **Option**                          | **Value**                           |
| ----------------------------------- | ----------------------------------- |
| Link name                           | ResearchVnetLink                    |
| Subscription                        | No changes required                 |
| Virtual Network                     | ResearchVnet (ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>) |
| Enable auto registration            | Selected                            |
| Review your settings and select OK. |                                     |


10. Select **Refresh**.

11. Verify that the ResearchVnetLink has been created, and that auto-registration is enabled.

   ‎![](../media/vn11.png) 

##  Task 7: Create Virtual Machines to test the configuration

In this section, you will create two test VMs to test the Private DNS zone configuration.

1. On the Azure portal, open the **PowerShell** session within the **Cloud Shell** pane.
    
    > **Note:** If this is the first time opening Cloud Shell, you might be prompted to create a storage account. Select **Show advanced settings** provide your details and then click on **Create storage**.

2. On the toolbar of the Cloud Shell pane, select the **Upload/Download files** icon, in the drop-down menu, select **Upload** and upload the following files **azuredeploy.json**, **ManufacturingVMazuredeploy.json** and **azuredeploy.parameters.json**, **ManufacturingVMazuredeploy.parameters.json** into the Cloud Shell home directory one by one from the source folder **F:\Allfiles\Exercises\M01**.

3. Deploy the following ARM templates to create the VMs needed for this exercise:

   ```powershell
   $RGName = "ContosoResourceGroup"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json ManufacturingVMazuredeploy.json -TemplateParameterFile azuredeploy.parameters.json ManufacturingVMazuredeploy.parameters.json
   ```
  
4. When the deployment is complete, go to the Azure portal home page, and then select **Virtual Machines**.

5. Verify that both virtual machines have been created.

## Task 8: Create a Virtual Machine to test the configuration

In this section, you will create a test VM on the Manufacturing VNet to test if you can access resources inside another Azure virtual network from your ManufacturingVnet.


## Connect to the Test VMs using RDP

1. On the Azure Portal home page, select **Virtual Machines**.

2. Select **ManufacturingVM**.

3. In ManufacturingVM, select **Connect &gt; RDP**.

4. In ManufacturingVM | Connect, select **Download RDP file**.

5. Save the RDP file to your desktop.

6. Connect to ManufacturingVM using the RDP file, and the username and password you specified when you created the VM.

7. On the Azure Portal home page, select **Virtual Machines**.

8. Select **TestVM1**.

9. In TestVM1, select **Connect &gt; RDP**.

10. In TestVM1 | Connect, select **Download RDP file**.

11. Save the RDP file to your desktop.

12. Connect to TestVM1 using the RDP file, and the username and password you specified when you created the VM.

13. On both VMs, in **Choose privacy settings for your device**, select **Accept**.

14. On both VMs, in **Networks**, select **Yes** if prompted.

15. On the start menu of TestVM1 type **PowerShell** and click to open it then Enter the command: ipconfig

16. Note the IPv4 address. 

 

## Task 9: Test the connection between the VMs

1. On the ManufacturingVM, open a PowerShell prompt.

2. Use the following command to verify that there is no connection to TestVM1 on CoreServicesVnet. Be sure to use the IPv4 address for TestVM1.

| PowerShell                               |
| ---------------------------------------- |
| Test-NetConnection 10.20.20.4 -port 3389 |


3. The test connection should fail, and you will see a result similar to the following:
   ![PowerShell window with Test-NetConnection 10.20.20.4 -port 3389 showing failed ](../media/test-netconnection-fail.png)

 

## Task 10: Create VNet peerings between CoreServicesVnet and ManufacturingVnet

1. On the Azure home page, select **Virtual Networks**, and then select **CoreServicesVnet**.

2. In CoreServicesVnet, under **Settings**, select **Peerings**.
   ![screen shot of core services VNet Peering settings ](../media/create-peering-on-coreservicesvnet.png)

3. On CoreServicesVnet | Peerings, select **+ Add**.

4. Use the information in the following table to create the peering.

| **Section**                          | **Option**                                    | **Value**                             |
| ------------------------------------ | --------------------------------------------- | ------------------------------------- |
| This virtual network                 |                                               |                                       |
|                                      | Peering link name                             | CoreServicesVnet-to-ManufacturingVnet |
|                                      | Traffic to remote virtual network             | Allow (default)                       |
|                                      | Traffic forwarded from remote virtual network | Allow (default)                       |
|                                      | Virtual network gateway or Route Server       | None (default)                        |
| Remote virtual network               |                                               |                                       |
|                                      | Peering link name                             | ManufacturingVnet-to-CoreServicesVnet |
|                                      | Virtual network deployment model              | Resource manager                      |
|                                      | I know my resource ID                         | Not selected                          |
|                                      | Subscription                                  | Use default available                 |
|                                      | Virtual network                               | ManufacturingVnet                     |
|                                      | Traffic to remote virtual network             | Allow (default)                       |
|                                      | Traffic forwarded from remote virtual network | Allow (default)                       |
|                                      | Virtual network gateway or Route Server       | None (default)                        |
| Review your settings and select Add. |                                               |                                       |
|                                      |                                               |                                       |


5. In CoreServicesVnet | Peerings, verify that the **CoreServicesVnet-to-ManufacturingVnet** peering is listed.

6. Under Virtual networks, select **ManufacturingVnet**, and verify the **ManufacturingVnet-to-CoreServicesVnet** peering is listed.

 

## Task 11: Test the connection between the VMs

1. On the ManufacturingVM, open a PowerShell prompt.

2. Use the following command to verify that there is now a connection to TestVM1 on CoreServicesVnet. 

| PowerShell                               |
| ---------------------------------------- |
| Test-NetConnection 10.20.20.4 -port 3389 |


3. The test connection should succeed, and you will see a result similar to the following:
   ![Powershell window with Test-NetConnection 10.20.20.4 -port 3389 showing TCP test succeeded: true](../media/test-connection-succeeded.png)

 

Congratulations! You have successful configured connectivity between VNets by adding peerings. 
