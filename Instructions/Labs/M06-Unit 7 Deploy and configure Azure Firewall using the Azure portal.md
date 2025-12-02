# Lab 6b: Deploy and configure Azure Firewall using the Azure portal

## Lab Overview

In this lab, as a member of the Network Security team at Contoso, you will focus on configuring Azure Firewall to control access to various websites by creating rules that either allow or deny specific connections. The lab will guide you through setting up network infrastructure and deploying firewall rules. You'll also configure routes, application rules, network rules, DNAT rules, and test the firewall to ensure it's operating correctly.

## Lab Objectives

In this lab, you will complete the following tasks:

+ Task 1: Create a virtual network and subnets
+ Task 2: Create a virtual machine
+ Task 3: Deploy the firewall and firewall policy
+ Task 4: Create a default route
+ Task 5: Configure an application rule
+ Task 6: Configure a network rule
+ Task 7: Configure a Destination NAT (DNAT) rule
+ Task 8: Change the primary and secondary DNS address for the server's network interface
+ Task 9: Test the firewall

## Estimated time: 60 minutes

## Architecture diagram

 ‎![](../media/az700-m6-unit7.png)
   
## Task 1: Create a virtual network and subnets

In this task, you will create a virtual network in Azure and configure two subnets: one for the Azure Firewall and another for the workload server.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual network(1)**, and then select **Virtual 
   network(2)** under services.

    ![](../media/azv20.png)

1. Select **+ Create**.

1. On the Create virtual network of **Basic** tab specify the following information to create Virtual Network and the select **IP addresses (5)**:

     |  **Setting**     | **Value**            |
     | ---------------- | ------------------   |
     | Subscription   |  Leave the default one **(1)** |     
     | Resource group   |  **Test-FW-RG-<inject key="DeploymentID" enableCopy="false"/> (2)** |
     | Name             |  **Test-FW-VN (3)**       |
     | Region           |  **<inject key="Region" enableCopy="false"/> (4)**  |

     ![](../media/azz1.png)

1. Navigate to the **IP Addresses** tab and enter IPv4 address space **10.0.0.0/16** **(1)** if not already there by default. 

     - Under **Subnet name**, select the word **default(2)**.

       ![](../media/azz2.png)

1. In the **Edit subnet** pane, specify the following and then click on **Save (6)**.

     |  **Setting**     | **Value**            |
     | ---------------- | ------------------   | 
     | Subnet purpose  | Select **Azure Firewall (1)**|
     | Name             | (Pre-populated) **AzureFirewallSubnet (2)**|
     | IPv4 address range | Select **10.0.0.0/16 (3)** |
     | Starting address | **10.0.1.0 (4)** |
     | Size      | **/26 (5)** |
 
     ![](../media/azz3.png)

1. Select **+ Add a subnet**, to create another subnet, which will host the workload server that you will create shortly.
    
1. In the **Add a subnet** page, specify the following and then select **Add (6)**.

     |  **Setting**     | **Value**            |
     | ---------------- | ------------------   | 
     | Subnet purpose  | Select **Default (1)**|
     | Name             | **Workload-SN (2)**|
     | IPv4 address range | Select **10.0.0.0/16 (3)** |
     | Starting address | **10.0.3.0 (4)** |
     | Size      | **/24 (5)** |
 
    ![](../media/azz4.png)
   
1. Select **Review + create** and  **Create**.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

    <validation step="08881c22-489b-4f9d-b164-9dd736b25031" />

## Task 2: Create a virtual machine

In this task, you will create the workload virtual machine and place it in the Workload-SN subnet created previously.

1. On the Azure portal, select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

      ![](../media/unit6-image1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

      ![](../media/pwershell1.png)

1. On **Getting started** window choose **Mount storage account (1)** then under **Storage account subscription (2)** select your available subscription from the dropdown and click on **Apply (3)**.
   
      ![](../media/pwershell3.png)
   
1. Within the Mount storage account pane, select **I want to create a storage account (1)** and click **Next (2)**.

      ![](../media/pwershell4.png)
   
1. On the **Create a Storage account** page, provide the following details and then **Create (6)**: 

     - Subscription: Leave the default one **(1)**
     - Please make sure you have select your resource group **Test-FW-RG-<inject key="DeploymentID" enableCopy="false"/> (2)**
     - Select **Region** as **<inject key="Region" enableCopy="false"/> (3)**
     - Enter **blob<inject key="DeploymentID" enableCopy="false"/> (4)** for the **Storage account name**
     - Enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/> (5)** for the  **File share** 

       ![](../media/azz6.png)     

1. On the toolbar of the Cloud Shell pane, select the Select **Manage files (1)** icon, in the drop-down menu, select **Upload (2)**.

      ![](../media/pwershell2.png)

1. Navigate to `C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M06` **(1)**, then upload the following files **firewall.json** and **firewall.parameters.json** files **(2)** and then **Open (3)**.

      ![](../media/azz7.png)

1. Deploy the following ARM templates to create the VM needed for this exercise:

   ```powershell
   $RGName = "Test-FW-RG-<inject key="DeploymentID" enableCopy="false"/>"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile firewall.json -TemplateParameterFile firewall.parameters.json
   ```

    ![](../media/azz9.png)   

     >**Note**: You will be prompted to provide an Admin password, enter **Pa55w.rd!!**.

1. When the deployment is complete, go to the Azure portal home page, then search **Virtual Machines (1)** and select **Virtual Machines (2)**.

     ![](../media/azz8.png)

1. Verify that the virtual machine has been created.

1. Select **Srv-Work** virtual machine.

     ![](../media/unit74.png)

1. On the **Overview** page of **Srv-Work**, within the left navigation pane, under the **Networking** section, select **Network settings (1)** and make a note of the **Private IP address (2)** for this VM (e.g., **10.0.2.4**) you may need this in next coming tasks.

     ![](../media/azz10.png)
 
## Task 3: Deploy the firewall and firewall policy

In this task, you will deploy the firewall into the virtual network with a firewall policy configured.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Firewalls (1)**, and then select **Firewalls (2)** under 
   services.

    ![](../media/azz11.png)
  
1. On the **Firewall** page, select **+ Create**.

1. On the **Basics** tab, create a firewall using the information in the table below and navigate to the **Next:Tags> (12)** tab.

    | **Setting**              | **Value**                                                    |
    | --------------------     | ------------------------------------------------------------ |
    | Subscription             | Select your subscription **(1)**                                    |
    | Resource group           | **Test-FW-RG-<inject key="DeploymentID" enableCopy="false"/> (2)**   |
    | Firewall name            | **Test-FW01 (3)**                                                |
    | Region                   | **<inject key="Region" enableCopy="false"/> (4)**               |
    | Firewall SKU             | **Standard (5)**                                                 |
    | Firewall management      | **Use a Firewall Policy to manage this firewall (6)**            |
    | Firewall policy          | Select **Add new**<br /> Name: **fw-test-pol (7)**<br />         |
    | Choose a virtual network | **Use existing (8)**                                             |
    | Virtual network          | **Test-FW-VN** (Please ignore the error it will disappear once you disable the Firewall Mangagement NIC) **(9)**                                               |
    | Public IP address        | Select **Add new**<br /> Name: **fw-pip (10)**                    |
    | **Enable Firewall Management NIC**              | **Unselected (11)** (Once you disable, please add the Public Ip again that is mentioned in the above step)                                                   |
    |||

    ![](../media/l6u7-4.png)    
    ![](../media/azz12.png)

1. Click on **Review+create**.    

1. Select **Create** and wait for the firewall deployment to complete.

    ![](../media/azz13.png)

1. When deployment of the firewall is completed, select **Go to resource**.

1. On the **Overview (1)** page of **Test-FW01**, on the right of the page, make a note of the **Firewall private IP (2)** for this firewall (e.g., **10.0.1.4**) you may need this in coming tasks.

    ![](../media/azz14.png)

1. In the menu on the left, under **Settings**, select **Public IP configuration (1)**.

     - Make a note of the address under **IP Address (2)** for the **fw-pip** public IP configuration (e.g., **20.90.136.51**) you may need this in coming tasks.

       ![](../media/azz16.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

   <validation step="bbcd151e-4daf-4277-a102-9c908ccce3a7" />

## Task 4: Create a default route

In this task, on the Workload-SN subnet, you will configure the outbound default route to go through the firewall.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Route tables (1)**, and then select **Route tables (2)** 
   under services.

     ![](../media/azz17.png)
   
1. On the **Route table** page, select **+ Create**.

1. On the **Basics** tab, create a new route table using the information in the table below.

    | **Setting**              | **Value**                |
    | ------------------------ | ------------------------ |
    | Subscription             | Select your subscription **(1)** |
    | Resource group           | **Test-FW-RG-<inject key="DeploymentID" enableCopy="false"/> (2)**  |
    | Region                   | **<inject key="Region" enableCopy="false"/> (3)** |
    | Name                     | **Firewall-route (4)**       |
    | Propagate gateway routes | **Yes (5)**                  |
 
     ![](../media/unit76.png)

1. Select **Review + create**.

1. Select **Create**.

1. After deployment completes, select **Go to resource**.

1. Select **Firewall-route** route table.

     ![](../media/azz18.png)

1. On the **Firewall-route** page, from the left navigation menu, under **Settings**, select **Subnets (1)** and then select **+ Associate (2)**.

     ![](../media/azz19.png)

1. Specify the following and **OK (3)**.     
 
     | **Setting**              | **Value**                |
     | ------------------------ | ------------------------ |
     | Virtual Network          | select **Test-FW-VN (1)**    |
     | Subnet                   | **Workload-SN (2)**          |
 
     ![](../media/azz20.png)

      >**Note**:  Make sure that you select only the Workload-SN subnet for this route, otherwise your firewall won't work correctly.     

1. From the left navigation menu, under **Settings**, select **Routes** and then select **+ Add (2)**.

     ![](../media/azz21.png) 

1. Specify the following details and the **Add (6)**:

     | **Setting**                              | **Value**                |
     | --------------------------------------   | ------------------------ |
     | **Route name**                           | **fw-dg (1)**                |
     | **Destination type**                     | **IP address (2)**           |
     | **Destination IP addresses/CIDR ranges** |  **0.0.0.0/0 (3)**           |
     | **Next hop type**                        | **Virtual appliance (4)**    |
     | **Next hop address**                     | Enter the private IP address for the firewall that you noted previously **(5)** (e.g., **10.0.1.4**) |

     ![](../media/azz22.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

   <validation step="6f72e424-9c22-483a-b2ff-d23e76f16156" />

## Task 5: Configure an application rule

In this task, you will add an application rule that allows outbound access to www.google.com.

1. On the Azure portal home page, from top left corner of page click **Show portal menu** and select **All resources**.

     ![](../media/l6u7-9.png)

1. In the list of resources, select your firewall policy, **fw-test-pol**.

     ![](../media/azz23.png)

1. From the left navigation menu, under **Rules** section, select **Application Rules (1)** and then select **+ Add a rule collection (2)**.

     ![](../media/azz24.png)

1. On the **Add a rule collection** page, create a new application rule using the information in the table below, and select **Add (12)**.

    | **Setting**            | **Value**                                 |
    | ---------------------- | ----------------------------------------- |
    | Name                   | **App-Coll01 (1)**                            |
    | Rule collection type   | **Application (2)**                           |
    | Priority               | **200 (3)**                                   |
    | Rule collection action | **Allow (4)**                                 |
    | Rule collection group  | **DefaultApplicationRuleCollectionGroup (5)** |
    | **Rules Section**      |                                           |
    | Name                   | **Allow-Google (6)**                          |
    | Source type            | **IP Address (7)**                            |
    | Source                 | **10.0.3.0/24 (8)**                           |
    | Protocol               | **http,https (9)**                            |
    | Destination type       | **FQDN (10)**                                  |
    | Destination            | **www.google.com (11)**                    |

    ![Add an application rule collection](../media/azz25.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

   <validation step="9888c186-d2ae-48de-8856-e29a597e6fab" />

## Task 6: Configure a network rule

In this task, you will add a network rule that allows outbound access to two IP addresses at port 53 (DNS).

1. On the **fw-test-pol** page, from the left navigation menu, under **Rules** section, select **Network Rules (1)** and then select **+ Add a rule collection (2)**.

     ![](../media/azz27.png)

1. On the **Add a rule collection** page, create a new network rule using the information in the table below, and select **Add (13)**.

    | **Setting**            | **Value**                                                    |
    | ---------------------- | ------------------------------------------------------------ |
    | Name                   | **Net-Coll01 (1)**                                               |
    | Rule collection type   | **Network (2)**                                                  |
    | Priority               | **200 (3)**                                                      |
    | Rule collection action | **Allow (4)**                                                    |
    | Rule collection group  | **DefaultNetworkRuleCollectionGroup (5)**                        |
    | **Rules Section**      |                                                              |
    | Name                   | **Allow-DNS (6)**                                                |
    | Source type            | **IP Address (7)**                                               |
    | Source                 | **10.0.3.0/24 (8)**                                              |
    | Protocol               | **UDP (9)**                                                      |
    | Destination Ports      | **53 (10)**                                                       |
    | Destination Type       | **IP Address (11)**                                               |
    | Destination            | **209.244.0.3, 209.244.0.4 (12)** <br />These are public DNS servers operated by Century Link. |
 
    ​![Add a network rule collection](../media/azz26.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

   <validation step="9879c295-9be5-4eec-b280-5597936582ba" />

## Task 7: Configure a Destination NAT (DNAT) rule

In this task, you will add a DNAT rule that allows you to connect a remote desktop to the Srv-Work virtual machine through the firewall.

1. On the **fw-test-pol** page, from the left navigation menu, under **Rules** section, select **DNAT Rules (1)** and select **+ Add a rule collection (2)**.

     ![](../media/azz28.png)

1. On the **Add a rule collection** page, create a new DNAT rule using the information in the table below.

      | **Setting**           | **Value**                                                    |
      | --------------------- | ------------------------------------------------------------ |
      | Name                  | **rdp (1)**                                                      |
      | Rule collection type  | **DNAT (2)**                                                     |
      | Priority              | **200 (3)**                                                      |
      | Rule collection group | **DefaultDnatRuleCollectionGroup (5)**                           |
      | **Rules Section**     |                                                              |
      | Name                  | **rdp-nat**                                                  |
      | Source type           | **IP Address**                                               |
      | Source                | *                                                            |
      | Protocol              | **TCP**                                                      |
      | Destination Ports     | **3389**                                                     |
      | Destination           | Enter the firewall public IP address from **fw-pip** that you noted earlier.<br />(For e.g. - 20.90.136.51) |
      | Translated type       | **IP Address**                                                     |
      | Translated address or    | Enter the private IP address from **Srv-Work** that you noted earlier.<br />For e.g. - 10.0.2.4 |
      | Translated port       | **3389 (6)**                                                     |

      ![Add a DNAT rule collection](../media/add-a-dnat-rule1.png)

1. Select **Add**.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

   <validation step="b427e7bd-b2fb-46ce-93e9-8d17c0499133" />

## Task 8: Change the primary and secondary DNS address for the server's network interface

In this task, you will configure the Srv-Work server's primary and secondary DNS addresses. However, this is not a general Azure Firewall requirement.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Resource groups (1)**, and then select **Resource 
   groups (2)** under services.

     ![](../media/azz29.png)

1. In the list of resource groups, select your resource group, **Test-FW-RG-<inject key="DeploymentID" enableCopy="false"/>**.

1. In the list of resources in this resource group, select the **Network interface** for the **Srv-Work** virtual machine (e.g., **srv-work-nic**).

     ![](../media/azz30.png)

1. From the left navigation menu, under **Settings**, select **DNS servers (1)**.

     - Under **DNS servers**, select **Custom (2)**

     - Enter **209.244.0.3** in the **Add DNS server** text box, and **209.244.0.4** in the next text box **(3)**

     - Select **Save (4)**.

       ![](../media/azz31.png)

1. Navigate back to the **Srv-Work** virtual machine. Restart the **Srv-Work** virtual machine.

     ![](../media/azz32.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. You can proceed to the next task if you receive a success message.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

    <validation step="d9ca7dd9-969d-43af-a063-949cf3294bf8" />

## Task 9: Test the firewall

In this task, you will test the firewall to verify that the rules are configured correctly and working as expected. This configuration will enable you to connect a remote desktop connection to the Srv-Work virtual machine through the firewall, via the firewall's public IP address.

1. Within Labvm from start menu, search dor **RDP (1)** and then open **Remote Desktop Connection (2)**.

     ![](../media/azz33.png)

1. On the **Computer** box, enter the firewall's public IP address followed by **:3389** (e.g., **20.90.136.51:3389**) **(1)** and select **Connect (2)**.

     ![Add a network rule collection](../media/azz34.png)

1. Select **More choices**.

1. Select **Use a different account**.

1. On the **Enter your credentials** dialog box, log into the **Srv-Work** server virtual machine, 

     - On the **Username** box, enter **.\TestUser**
     - Enter Password as `Pa55w.rd!!`
     - Select **OK**.

1. Select **Yes** on the certificate message.

1. Open Internet Explorer and browse to **https://www.google.com**

1. On the **Security Alert** dialog box, select **OK**.

1. Select **Close** on the Internet Explorer security alerts that may pop-up.

1. You should see the Google home page.

     ![RDP session on Srv-work server - browser on google.com](../media/L6U7-2.png)

1. Browse to **https://www.microsoft.com**

1. You should be blocked by the firewall.

     ![RDP session on Srv-work server - browser blocked on microsoft.com](../media/L6U7-3.png)

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 
+ A firewall is a network security feature that sits between a trusted network and an untrusted network, such as the internet. The firewall's job is to analyze and then allow or deny network traffic.
+ Azure Firewall is a cloud-based firewall service. In most configurations, Azure Firewall is provisioned inside a hub virtual network. Traffic to and from the spoke virtual networks and the on-premises network is directed to the firewall.
+ Firewall rules evaulate the network traffic. Azure Firewall has three types of rules: Application, Network, and NAT. 
+ Azure Firewall is offered in three SKUs: Standard, Premium, and Basic.

## Review

In this lab, you have completed:

+  Create a virtual network and subnets
+  Create a virtual machine
+  Deploy the firewall and firewall policy
+  Create a default route
+  Configure an application rule
+  Configure a network rule
+  Configure a Destination NAT (DNAT) rule
+  Change the primary and secondary DNS address for the server's network interface
+  Test the firewall
 
## You have successfully completed the lab.
