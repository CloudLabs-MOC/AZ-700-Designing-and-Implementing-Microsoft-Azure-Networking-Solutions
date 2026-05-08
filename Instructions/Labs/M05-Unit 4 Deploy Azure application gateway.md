# Lab 5a: Deploy Azure Application Gateway

## Lab Overview

In this lab, you use the Azure portal to create an application gateway. Then you test it to make sure it works correctly.

The application gateway directs application web traffic to specific resources in a backend pool. You assign listeners to ports, create rules, and add resources to a backend pool. For the sake of simplicity, this article uses a simple setup with a public front-end IP, a basic listener to host a single site on the application gateway, a basic request routing rule, and two virtual machines in the backend pool.

For Azure to communicate between the resources that you create, it needs a virtual network. You can either create a new virtual network or use an existing one. In this example, you'll create a new virtual network while you create the application gateway. Application Gateway instances are created in separate subnets. You create two subnets in this example: one for the application gateway, and another for the backend servers.

## Lab Objectives
In this lab, you will complete the following tasks:

+ Task 1: Create an application gateway
+ Task 2: Create virtual machines
+ Task 3: Add backend servers to backend pool
+ Task 4: Test the application gateway

## Estimated time: 25 Minutes

## Architecture diagram

  ‎![](../media/az700-m5-unit4.png)

## Task 1: Create an application gateway

In this task, you'll create an Application Gateway in Azure, which is a fully managed load balancing solution for web applications. An Application Gateway operates at the application layer (OSI Layer 7), providing features like URL-based routing, SSL termination, and cookie-based session affinity. 

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Application gateways (1)**, and then select **Application 
   gateways (2)** from the results.

    ![Azure Portal search for application gateway](../media/l5u4-1.png)    

1. On the **Load balancing and content delivery | Application gateways** page, select **+ Create (1)** then from drop down choose **Application gateways (2)** .

      ![Azure Portal search for application gateway](../media/lab02-az700-ima26.png)
   
1. On the Create application gateway **Basics** tab, enter, or select the following information:

   | **Setting**         | **Value**                                    |
   | ------------------- | -------------------------------------------- |
   | Subscription        | Select your subscription **(1)**.                    |
   | Resource group      | Select **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> (2)**       |
   | Application gateway name | **ContosoAppGateway (3)**                            |
   | Region              | **<inject key="Region" enableCopy="false"/> (4)**                           |
   | Virtual Network     | Select **Create new (5)**                        |

    ![Azure Portal search for application gateway](../media/lab02-az700-ima27.png)    
    
1. On Create virtual network, configure the following and then click on **OK (5)** to return to the Create application gateway Basics tab:

   | **Setting**       | **Value**                          |
   | ----------------- | ---------------------------------- |
   | Name              | **ContosoVNet (1)**                        |
   | **ADDRESS SPACE** |                                    |
   | Address range     | **10.0.0.0/16 (2)**                        |
   | **SUBNETS**       |                                    |
   | Subnet name       | Change **default** to **AGSubnet (3)** |
   | Address range     | **10.0.0.0/24 (4)**                        |

   ![Azure Portal search for application gateway](../media/lab02-az700-ima28.png)

1. Accept the default values for the other settings and then select **Next: Frontends>**.

1. On the **Frontends (1)** tab, verify **Frontend IP address type** is set to **Public (2)**. Select **Add new (3)** for the **Public IP address** and enter **AGPublicIPAddress (4)** for the public IP address name, and then select **OK (5)**.

   ![Azure Portal search for application gateway](../media/lab02-az700-ima29.png)

1. Select **Next: Backends>**.

1. On the **Backends** tab, select **Add a backend pool (1)**. Then on the **Add a backend pool** window that opens, enter the following values to create an empty backend pool:

    | **Setting**                      | **Value**   |
    | -------------------------------- | ----------- |
    | Name                             | **BackendPool (2)** |
    | Add backend pool without targets | **Yes (3)**     |
    
1. On the **Add a backend pool** window, select **Add (4)** to save the backend pool configuration and return to the **Backends** tab.

      ![Azure Portal search for application gateway](../media/lab02-az700-ima30.png)

1. On the **Backends** tab, select **Next: Configuration>**.

1. On the **Configuration** tab, you'll connect the frontend and backend pool you created using a routing rule.

1. On the **Routing rules** column, select **+ Add a routing rule**.

   ![](../media/IMG3.png)

1. On **Add a routing rule** window, enter or select the following information:

    | **Setting**   | **Value**         |
    | ------------- | ----------------- |
    | **Rule name** | **RoutingRule (1)**   |
    | **Priority**  | **100 (2)**           |

    ![Azure Portal search for application gateway](../media/l5u4-2-4.png)

1. On the **Listener (1)** tab, enter or select the following information:

    | **Setting**   | **Value**         |
    | ------------- | ----------------- |
    | Listener name | **Listener**  **(2)**        |
    | Frontend IP   | Select **Public IPv4 (3)** |

    ![Azure Portal search for application gateway](../media/l5u4-2-5.png)

1. Accept the default values for the other settings on the **Listener** tab.

1. Select the **Backend targets (1)** tab to configure the rest of the routing rule. Then enter or select the following information:

    | **Setting**      | **Value**      |
    | -------------    | -------------- |
    | Target type      | **Select Backendpool in drop down (2)**  |
    | Backend Settings | **Add new (3)** |

    ![Azure Portal search for application gateway](../media/lab5-1.png)  

1. In **Add a Backend Setting**, enter or select the following information:

    | **Setting**          | **Value**   |
    | ------------------   | ----------- |
    | Backend settings name | **HTTPSetting  (1)**|
    | Backend port         | **80 (2)**         |

    ![Azure Portal search for application gateway](../media/l5u4-2-7.png)  

1. Accept the default values for the other settings in the **Add a Backend Setting** window, then select **Add** to return to **Add a routing rule**.

   ![Azure Portal search for application gateway](../media/IMG45.png)

1. Select **Add** to save the routing rule and return to the **Configuration** tab.

1. Select **Next: Tags** and then **Next: Review + create**.

1. Review the settings on the **Review + create** tab, click on **Create** to create the virtual network, the public IP address, and the application gateway.

   ![Azure Portal search for application gateway](../media/IMG6.png)

    > **Note:** It may take 5 minutes for Azure to create the application gateway. Wait until the deployment finishes successfully before moving on to the next section.
   
1. On any Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual networks (1)**, and then select **Virtual 
   networks (2)** from the results.

   ![Azure Portal search for application gateway](../media/IMG7.png)
   
1. On the Virtual networks page select **ContosoVNet**. 
   
   ![Azure Portal search for application gateway](../media/IMG8.png)

1. On the ContosoVNet page from left side menu under the Settings section, click on **Subnets (1)**. select **+ Subnet (2)**. On Add subnet page fill the follwing details(leave other field as default) and click on **Add (6)**.

    | **Setting**           | **Value**   |
    | --------------------- | ----------- |
    | Name                  | **BackendSubnet (3)** |
    | Starting address      | **10.0.1.0 (4)**|
    | Size |   **/24 (5)**  |              
     
    ![Azure Portal search for application gateway](../media/lab02-az700-ima31.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="b6e75632-552c-426b-806e-3de77ccca05f" />

## Task 2: Create virtual machines

In this task, you'll use Azure Cloud Shell to deploy two virtual machines (VMs) using ARM templates. You will configure the necessary settings for the VMs by uploading configuration files and deploying them using PowerShell commands.

1. On the Azure portal, select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

   ![](../media/unit6-image1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

   ![](../media/pwershell1.png)

1. On **Getting started** window choose **Mount storage account (1)** then under **Storage account subscription (2)** select your available subscription from the dropdown and click on **Apply (3)**.
   
     ![](../media/pwershell3.png)
   
1. Within the Mount storage account pane, select **I want to create a storage account (1)** and click **Next (3)**.

     ![](../media/pwershell4.png)
   
1. Please make sure you have selected your resource group **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>** and then select **Region** **<inject key="Region" enableCopy="false"/>** and enter **blob<inject key="DeploymentID" enableCopy="false"/>** for the **Storage account** and enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/>** for the  **File share** , then click on **Create**.

   ![](../media/pwershell5.png)

1. On the toolbar of the Cloud Shell pane, select the Select **Manage files (1)** icon, in the drop-down menu, select **Upload (2)** and upload the following files **backend.json**, **backend.parameters.json**, and **install-iis.ps1** into the Cloud Shell home directory one by one from the source folder **C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M05**.

      ![](../media/pwershell2.png)

1. Deploy the following ARM templates to create the VMs needed for this exercise:
  

   ```powershell
   $RGName = "ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile backend.json -TemplateParameterFile backend.parameters.json
   ```
1. You will be prompted to provide an Admin password. Provide Admin password Password: **Pa55w.rd!!**
  
1. When the deployment is complete, go to the Azure portal home page, and then select **Virtual Machines**.

1. Verify that both virtual machines have been created.

   ![](../media/a7z3.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="d91accdd-32fd-4212-96ac-3f721c86f133" />
   
### Install IIS on each virtual machine

1. Each backend server needs IIS installed.

1. Continue at the PowerShell prompt and use the provided script to install IIS on **BackendVM1**.

   ```powershell
   Invoke-AzVMRunCommand -ResourceGroupName 'ContosoResourceGroup' -Name 'BackendVM1' -CommandId 'RunPowerShellScript' -ScriptPath 'install-iis.ps1'
   ```

   >**Note**: While you wait review the PowerShell script. Notice that the IIS home page is being customized to provide the virtual machine name.

1. Run the command again, this time for **BackendVM2**.

   ```powershell
   Invoke-AzVMRunCommand -ResourceGroupName 'ContosoResourceGroup' -Name 'BackendVM2' -CommandId 'RunPowerShellScript' -ScriptPath 'install-iis.ps1'
   ```
   >**Note**: Each command will take a couple of minutes to complete.

## Task 3: Add backend servers to backend pool

In this task, you will add two virtual machines to the backend pool of your Application Gateway, enabling the gateway to distribute traffic between the backend servers.

1. On the Azure portal menu, from top left corner of page click **Show portal** menu and select **All resources**.

   ![](../media/unit4-image5.png)

1. Then select **ContosoAppGateway**.   
   
1. On the **ContosoAppGateway** blade, under **Settings** section, select **Backend pools**.

1. Select **BackendPool**.

    ![Azure Portal search for application gateway](../media/lab02-az700-ima32.png)
   
1. On the **Edit backend pool** page, under **Backend targets (1)**, 

   - In **Target type**, select **Virtual machine**.

      - Under **Target**, select **BackendVM1-nic.** 

   - On **Target type**, select **Virtual machine**.

      - Under **Target**, select **BackendVM2-nic.**
   
1. Select **Save (2)**.

    ![Azure Portal search for application gateway](../media/a7z4.png)  

    **Note:** Wait for the deployment to complete before proceeding to the next step.

1. Check to ensure the backend servers are healthy. Select **Monitoring** and then **Backend Health**. Both targets should be healthy. 

   ![Azure Portal check backend health.](../media/lab02-az700-ima34.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="2c0059d3-d02b-4c7a-9937-c06f943ec1ef" />

## Task 4: Test the application gateway

In this task, you will test the functionality of the Application Gateway by accessing the public IP address assigned to it. This will allow you to verify that the gateway is working correctly by routing requests to the backend virtual machines.

Although IIS isn't required to create the application gateway, you installed it in this exercise to verify if Azure successfully created the application gateway.

### Use IIS to test the application gateway:

1. On the application gateway navigate to **Overview** page and find the public IP address. 

    ![Azure Portal search for application gateway](../media/lab02-az700-ima33.png)  

1. Copy the public IP address, and then paste it into the address bar of your browser to browse that IP address.

1. Check the response. A valid response verifies that the application gateway was successfully created and can successfully connect with the backend.

   ![Broswer - display BackendVM1 or BackendVM2 depending which backend server reponds to request.](../media/browse-to-backend1.png)

1. Refresh the browser multiple times and you should see connections to both BackendVM1 and BackendVM2.

   Congratulations! You have configured and tested an Azure Application Gateway.

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 
+ Azure Application Gateway is a web traffic (OSI layer 7) load balancer that enables you to manage traffic to your web applications.
+ Application Gateway can make routing decisions based on additional attributes of an HTTP request, for example URI path or host headers.
+ Use Application Gateway for application hosted in a single region and when you need URL based routing. 

### Review
In this lab, you have completed:

- Created an application gateway
- Created virtual machines
- Added backend servers to backend pool
- Tested the application gateway

## You have successfully completed the lab.

