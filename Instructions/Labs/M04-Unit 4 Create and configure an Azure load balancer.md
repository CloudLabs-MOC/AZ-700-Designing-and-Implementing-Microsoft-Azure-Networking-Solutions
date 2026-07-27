# Lab 4a: Create and configure an Azure load balancer

## Lab Overview

In this lab, you will explore Privileged Identity Management (PIM) in Microsoft Entra ID, configure a user with administrative roles, and manage user access and permissions. you will also experience activating roles, setting up multi-factor authentication, and making changes to user groups.

## Lab Objectives

In this lab, you will complete the following tasks:

+ Task 1: Create the virtual network
+ Task 2: Create backend servers
+ Task 3: Create the load balancer
+ Task 4: Create load balancer resources
+ Task 5: Test the load balancer

## Estimated time: 90 minutes

## Architecture diagram

![internal standard loadbalancer diagram](../media/exercise-internal-standard-load-balancer-environment-diagram.png)

## Task 1: Create the virtual network

In this task, you will create a virtual network (VNet) in Azure with two subnets. The process includes configuring settings for the network, enabling Azure Bastion, defining an IPv4 address space, and setting up subnets for backend and frontend services. 

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual networks(1)**, and then select **Virtual networks(2)** under services.

    ![](../media/azn84.png)

1. Select **+ Create** on the Virtual networks page.  

1. On the **Basics** tab, use the information in the table below to create the virtual network and then **Next (5)**:

   | **Setting**    | **Value**                                  |
   | -------------- | ------------------------------------------ |
   | Subscription   | Select your subscription **(1)**                  |
   | Resource group | Select **IntLB-RG-<inject key="DeploymentID" enableCopy="false"/> (2)** |
   | Name           | **IntLB-VNet (3)**                                                      |
   | Region         | **<inject key="Region" enableCopy="false"/> (4)**                   |

   ![](../media/azn75.png)

1. On the **Security** tab, under **Azure Bastion** select **Enable Azure Bastion (1)**, then enter the information from the table below.

   | **Setting**                       | **Value**                                     |
   | --------------------------------- | --------------------------------------------- |
   | Azure Bastion host name           | **myBastionHost (2)**                             |
   | Azure Bastion public IP address   | Select **Create a public IP address**  Name: **myBastionIP (3)** and then **OK** |

   - Select **Next (4)**.

     ![](../media/azn76.png)
   
1. On the **IP Addresses** tab, in the **IPv4 address space** box, don't remove the default, click on **Add IPV4 address space**.

   ![](../media/azn77.png)

1. In new **IPV4 address space**, enter **10.1.0.0 (1)** in address space and **/16 (2)** in size field and select **+ Add a subnet (3)** in the new IPv4 address space.

   ![](../media/azn78.png)

1. On **Add a subnet** blade, specify the following and select **Add (5)**.

   | **Setting**                  | **Value**     |
   | ---------------------------- | ------------- |
   | Name                         | **myBackendSubnet (1)** |
   | IPv4 address range                         | Select **10.1.0.0/16 (2)** |   
   | Starting address             | **10.1.0.0 (3)**        |
   | Size                         | **/24 (4)**             |
   |||

   ![](../media/azn79.png)
   
1. On the Create virtual network of **IP address** tab select **+ Add a subnet** in the new IPv4 address space. On **Add a subnet** blade specify the following and select **Add (5)**.

   | **Setting**                  | **Value**     |
   | ---------------------------- | ------------- |
   | Name                         | **myFrontEndSubnet** |
   | IPv4 address range                         | Select **10.1.0.0/16 (2)** |    
   | Starting address             | **10.1.2.0 (3)**        |
   | Size                         | **/24 (4)**             |

   ![](../media/azn80.png)
   
1. Select **Review + create**.

   ![](../media/azn81.png)

1. Select **Create**.

1. It will take `10-15 minutes` to complete the deployment.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="f3f7612a-945c-453b-87cc-9ac4592a00cc" />

## Task 2: Create backend servers

In this task, you will create three VMs, that will be in the same availability set, for the backend pool of the load balancer, add the VMs to the backend pool, and then install IIS on the three VMs to test the load balancer.

1. On the Azure portal select the **Cloud shell** (**[>_]**)  button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

   ![](../media/unit6-image1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell**.

   ![](../media/pwershell1.png)

1. On **Getting started** window choose **Mount storage account (1)** then under **Storage account subscription (2)** select your available subscription from the dropdown and click on **Apply (3)**.
   
   ![](../media/pwershell3.png)

1. Within the Mount storage account pane, select **I want to create a storage account (1)** and click **Next (2)**.

   ![](../media/pwershell4.png)
       
1. On the **Create a storage account** page, provide the following details and then click on **Create (6)**:

   - Subscription: Leave the deafult one **(1)**
   - Resource group: Select **IntLB-RG-<inject key="DeploymentID" enableCopy="false"/> (2)**
   - Region: Select **Region** **<inject key="Region" enableCopy="false"/> (3)**
   - Storage account name: Enter **blob<inject key="DeploymentID" enableCopy="false"/> (4)**
   - File share: Enter **blobfileshare<inject key="DeploymentID" enableCopy="false"/> (5)** 

     ![](../media/azn82.png)   

1. On the toolbar of the Cloud Shell pane, select the Select **Manage files (1)** icon, in the drop-down menu, select **Upload (2)**. 

   ![](../media/pwershell2.png)

1. Navigate to `C:\AllFiles\AZ-700-Designing-and-Implementing-Microsoft-Azure-Networking-Solutions-prod\Allfiles\Exercises\M04` **(1)** then select all 4 files **azuredeploy.json**, **azuredeploy.parameters.json** and then **Open (3)**.

   ![](../media/azn83.png)

1. Deploy the following ARM templates to create the VMs needed for this exercise:

   ```powershell
   $RGName = "IntLB-RG-<inject key="DeploymentID" enableCopy="false"/>"
   
   New-AzResourceGroupDeployment -ResourceGroupName $RGName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
   ```

 1. Each VM will be deployed sequentially. During the process, you will be prompted to enter the administrator password every time.  So please provide adminPassword: **Pa55w.rd!!**

 1. It may take `20-25 mins` to create these three VMs. Please wait until this job completes, and you will be prompted to provide password three times for each VM deployment.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="3107853e-92fb-4898-a59b-a7cb10f5051a" />

## Task 3: Create the load balancer

In this task, you will create an internal Standard SKU load balancer. The reason we are creating a Standard SKU load balancer here in the exercise, instead of a Basic SKU load balance, is for later exercises that require a Standard SKU version of the load balancer.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Load Balancer (1)**, and then select **Load Balancer (2)** under services.

   ![](../media/azn85.png)

1. Select **+ Create (1)** and then choose **Standard Load balancer (2)** on **Load balancing | Load Balancer** page.

   ![](../media/azn86.png)

1. On the **Basics** tab, use the information in the table below to create the load balancer and then select **Next: Frontend IP configuration> (7)**:

   | **Setting**           | **Value**                |
   | --------------------- | ------------------------ |
   | Subscription          | Select your subscription **(1)** |
   | Resource group        | **IntLB-RG-<inject key="DeploymentID" enableCopy="false"/> (2)**             |
   | Name                  | **myIntLoadBalancer (3)**    |
   | Region                | **<inject key="Region" enableCopy="false"/> (4)**         |
   | SKU                   | **Standard (5)**             |
   | Type                  | **Internal (6)**             |

   ![](../media/azn87.png)     

1. Click on  **+ Add a frontend IP configuration**.

1. On the **Add frontend IP configuration** blade, enter the information from the table below and select **Save (6)**.
 
   | **Setting**     | **Value**                |
   | --------------- | ------------------------ |
   | Name            | **LoadBalancerFrontEnd (1)** |
   | IP version      | **IPv4 (2)**                 |
   | Virtual network | **IntLB-VNet (3)**           |
   | Subnet          | **myFrontEndSubnet (4)**     |
   | Assignment      | **Dynamic (5)**              |

   ![](../media/azn89.png)     
   
1. Select **Review + create**.

   ![](../media/azn90.png)

1. Select **Create**. Wait for deployment to complete successfully.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="0aa25baa-7ec4-49ad-a99e-43e629157f51" />

## Task 4: Create load balancer resources

In this task, you will configure load balancer settings for a backend address pool, then create a health probe and a load balancer rule.

### Task 4.1: Create a backend pool and add VMs to the backend pool

The backend address pool contains the IP addresses of the virtual NICs connected to the load balancer.

1. On the Azure portal, from top left corner of page click **Show portal menu** and select **All resources**.

    ![](../media/unit4-image5.png)

1. Then select on **myIntLoadBalancer** from the resources list.    

    ![](../media/azn91.png)

1. On **myIntLoadBalancer** blade, from the left navigation menu, under **Settings** section, select **Backend pools (1)**, and then select **+ Add (2)**.

    ![](../media/azn92.png)

1. On the **Add backend pool** page, enter the information from the table below.

   | **Setting**     | **Value**            |
   | --------------- | -------------------- |
   | Name            | **myBackendPool (1)**    |
   | Virtual network | **IntLB-VNet**       |

   - Under **IP configurations**, select **+ Add (2)**.

     ![](../media/azn93.png)

1. Select the checkboxes for all 3 VMs (**myVM1**, **myVM2**, and **myVM3**) **(1)**, then select **Add (2)**.

    ![](../media/azn94.png)

1. Then select **Save**.

   ![Picture 7](../media/azn95.png)
   
### Task 4.2: Create a health probe

The load balancer monitors the status of your app with a health probe. The health probe adds or removes VMs from the load balancer based on their response to health checks. Here you will create a health probe to monitor the health of the VMs.

1. From **myIntLoadBalancer | Backend pools** blade, under **Settings** section, select **Health probes (1)**, then select **+ Add (2)**.

    ![](../media/azn96.png)

1. On the **Add health probe** page, enter the information from the table below and then **Save (6)**:

   | **Setting**         | **Value**         |
   | ------------------- | ----------------- |
   | Name                | **myHealthProbe(1)** |
   | Protocol            | **HTTP(2)**          |
   | Port                | **80(3)**            |
   | Path                | **/(4)**             |
   | Interval            | **15(5)**            |

   ![Picture 7](../media/azn97.png)
  

### Task 4.3: Create a load balancer rule

In this task, you will create a load balancer rule to manage traffic distribution to virtual machines. The rule specifies the frontend IP address, backend pool, protocol, port settings, and health probe,

A load balancer rule is used to define how traffic is distributed to the VMs. You define the frontend IP configuration for the incoming traffic and the backend IP pool to receive the traffic. The source and destination port are defined in the rule. Here you will create a load balancer rule.

1. From the **myIntLoadBalancer | Health probes** page of your load balancer, under **Settings** section, select **Load balancing rules (1)**, then click on **+ Add (2)**.

   ![Picture 7](../media/azn98.png)

1. On the **Add load balancing rule** page, enter the information from the table below and then **Save(12)**:

   | **Setting**            | **Value**                |
   | ---------------------- | ------------------------ |
   | Name                   | **myHTTPRule(1)**           |
   | IP Version             | **IPv4(2)**                 |
   | Frontend IP address    | **LoadBalancerFrontEnd(3)** |
   | Backend pool           | **myBackendPool(4)**       |
   | Protocol               | **TCP(5)**                  |
   | Port                   | **80(6)**                   |
   | Backend port           | **80(7)**                   |
   | Health probe           | **myHealthProbe(8)**        |
   | Session persistence    | **None(9)**                 |
   | Idle timeout (minutes) | **15(10)**                   |
   | Enable Floating IP     | **Disabled(11)**             |

   ![Picture 7](../media/az7005.png)

   ![Picture 7](../media/l4-u4-1.png)


> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="725af5b8-9a2d-4aeb-9adb-da47aa36d4aa" /> 
 
## Task 5: Test the load balancer

In this task, you will create a test VM, and then test the load balancer.

### Task 5.1: Create test VM

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual machines (1)**, and then select **Virtual machines (2)** under services.

    ![Picture 7](../media/azn99.png)

1. Select **+ Create** and choose **Azure virtual machine**. On the **Create a virtual machine** page, on the **Basics** tab, use the information in the table below to create the first VM.

    | **Setting**          | **Value**                                    |
    | -------------------- | -------------------------------------------- |
    | Subscription         | Select your subscription **(1)**                     |
    | Resource group       | **IntLB-RG-<inject key="DeploymentID" enableCopy="false"/> (2)**        |
    | Virtual machine name | **myTestVM (3)**                                 |
    | Region               |  **<inject key="Region" enableCopy="false"/> (4)**                              |
    | Availability options | **No infrastructure redundancy required (5)**    |
    | Security type        | **Standard (6)**                                    |
    | Image                | **Windows Server 2025 Datacenter - x64 Gen 2 (7)**   |
    | Size                 | **Standard_D2s_v3 - 2 vcpu, 8 GiB memory (8)**   |
    | Username             | **TestUser (9)**                                 |
    | Password             | `Pa55w.rd!!123` **(10)**                |
    | Confirm password     | `Pa55w.rd!!123` **(11)**                |
    | Select inbound port     | **RDP(3389) (12)**                |

    ![Picture 7](../media/az753.png)

    ![Picture 7](../media/l4-u4-3.png)

1. Select **Next : Disks (13)**, then select **Next : Networking**. 

1. On the **Networking** tab, use the information in the table below to configure networking settings and then select **Review + create (7)**:

    | **Setting**                                                  | **Value**                     |
    | ------------------------------------------------------------ | ----------------------------- |
    | Virtual network                                              | **IntLB-VNet (1)**                |
    | Subnet                                                       | **myBackendSubnet (2)**           |
    | Public IP                                                    | Change to **None (3)**            |
    | NIC network security group                                   | **Advanced (4)**                  |
    | Configure network security group                             | Select the existing **myNSG (5)** |
    | Load balancing options                                       | **None (6)**                      |

    ![Picture 7](../media/azn103.png)

1. Select **Create**.

1. Wait for this last VM to be deployed before moving forward with the next task.

### Task 5.2: Connect to the test VM to test the load balancer

In this task, you will connect to the myTestVM and test the internal load balancer by accessing its IIS web server. First, retrieve the Private IP address of your internal load balancer and use it to connect to the VM via Bastion. 

1. On the Azure portal, from top left corner of page click **Show portal menu** and select **All resources**.

    ![](../media/unit4-image5.png)

1. Then select on **myIntLoadBalancer** from the resources list.    

    ![](../media/azn91.png)

1. On the **Overview** page, make a note in notepad of the **Frontend IP address**, or copy it to the clipboard. You may need to select **See more** in order to see the **Frontend IP address** field.

    ![Picture 7](../media/l4-u4-2.png)

1. Select **Home**, then on the Azure portal home page, select **All resources**, then select the **myTestVM** virtual machine that you just created.

    ![Picture 7](../media/azn105.png)

1. On the **Overview** page, select **Connect (1)**. Under Configured connection section, select **Connect via Bastion (2)**.

    ![Picture 7](../media/azn106.png)

1. In the **Username** box, enter **TestUser (1)** and in the **Password** box, enter the password you created during **myTestVM** virtual machine deployment in task: 5.1 that is `Pa55w.rd!!123` **(2)**, then select **Connect (3)**.

    ![Picture 7](../media/azn107.png)
  
     >**Note**: If popup blocker is preventing the new window, at top of the page select **Always allow pop-ups and redirects from https://portal.azure.com** and select 
    **Done**, repeat step-5.

      ![Picture 7](../media/az70011.png)

1. The **myTestVM** window will open in another browser tab.

1. On the diagnostic data screen, click **Accept**.

    ![Picture 7](../media/az745.png)

1. Close the **Windows Admin Center / Azure Arc pop‑up** in Server Manager, then close or minimize Server Manager, and open **Microsoft Edge** from the taskbar.

    ![Picture 7](../media/az746.png)

1. When Microsoft Edge opens, select **Start without your data.**

    ![Picture 7](../media/az747.png)

1. On the browsing data settings screen, click **Confirm and continue** to continue with the default Microsoft Edge settings.

    ![Picture 7](../media/az748.png)

1. On the Microsoft Edge welcome screen, click **Continue without Google data** to proceed without importing browser data.

    ![Picture 7](../media/az749.png)

1. Open a new browser tab and enter the **Private IP address** from the previous step into the address bar of the browser and press Enter.

1. The default web home page of the IIS Web server is displayed in the browser window. One of the three virtual machines in the backend pool will respond.

    ![](../media/az751.png)

1. If you select the refresh button in the browser a few times, you will see that the response comes randomly from the different VMs in the backend pool of the internal load balancer.
   
> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully completed the task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="d643606f-8c58-498a-a834-5c18e2d7072a" />


## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 
+ Load balancing refers to efficiently distributing incoming network traffic across a group of backend servers or resources.
+ Azure Load Balancer distributes inbound flows from the load balancer's frontend to backend pool instances. These flows are distributed according to configured load-balancing rules and health probes. The backend pool instances can be Azure virtual machines (VMs) or virtual machine scale sets.
+ Azure offers both public and private load balancers. Public Load Balancers are ideal for internet-facing applications, outbound connections, and web applications. Private load balancers are better for internal applications, backend services, and hybrid scenarios.

## Review

In this lab, you have completed:
+ Created the virtual network
+ Created backend servers
+ Created the load balancer
+ Created load balancer resources
+ Tested the load balancer

## You have successfully completed the lab.
