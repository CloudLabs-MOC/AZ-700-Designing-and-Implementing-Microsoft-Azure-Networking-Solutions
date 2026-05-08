# Lab 1a: Design and implement a Virtual Network in Azure

## Lab Overview

In this lab, you will create and configure multiple virtual networks (VNets) and subnets in Azure. You will start by setting up the CoreServicesVnet, followed by the ManufacturingVnet, and finally the ResearchVnet, each with their respective subnets. This lab will guide you through the process of verifying the creation and configuration of these VNets and subnets, enhancing your skills in network architecture and management within Azure.

## Lab Objectives

In this lab, you will complete the following tasks:

+ Task 1: Create the CoreServicesVnet virtual network and subnets
+ Task 2: Create the ManufacturingVnet virtual network and subnets
+ Task 3: Create the ResearchVnet virtual network and subnets
+ Task 4: Verify the creation of VNets and Subnets

## Estimated Duration: 25 Minutes

## Architecture diagram
![](../media/design-implement-vnet-peering01.png)

You will create the following resources:
 

| **Virtual Network** | **Region**   | **Virtual network address space** | **Subnet**                | **Subnet**    |
| ------------------- | ------------ | --------------------------------- | ------------------------- | ------------- |
| CoreServicesVnet    | East US      | 10.20.0.0/16                      |                           |               |
|                     |              |                                   | GatewaySubnet             | 10.20.0.0/27  |
|                     |              |                                   | SharedServicesSubnet      | 10.20.10.0/24 |
|                     |              |                                   | DatabaseSubnet            | 10.20.20.0/24 |
|                     |              |                                   | PublicWebServiceSubnet    | 10.20.30.0/24 |
| ManufacturingVnet   | West Europe  | 10.30.0.0/16                      |                           |               |
|                     |              |                                   | ManufacturingSystemSubnet | 10.30.10.0/24 |
|                     |              |                                   | SensorSubnet1             | 10.30.20.0/24 |
|                     |              |                                   | SensorSubnet2             | 10.30.21.0/24 |
|                     |              |                                   | SensorSubnet3             | 10.30.22.0/24 |
| ResearchVnet        |Southeast Asia| 10.40.0.0/16                      |                           |               |
|                     |              |                                   | ResearchSystemSubnet      | 10.40.0.0/24  |


These virtual networks and subnets are structured in a way that accommodates existing resources yet allows for projected growth. Let's create these virtual networks and subnets to lay the foundation for our networking infrastructure.

### Task 1: Create the CoreServicesVnet virtual network and subnets

In this task, you'll be setting up a virtual network (VNet) called CoreServicesVnet and its associated subnets in Azure.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual Networks (1)**, and then select **Virtual Networks (2)** under services.
   
   ![](../media/azv20.png)

1. Select **+ Create** on the **Network foundation | Virtual networks** page. 
   
1. On **Basic** tab of **Create virtual network** use the information in the following table to create the **CoreServicesVnet** virtual network and select **IP Address (5)** tab.
 
   | **Tab**      | **Option**         | **Value**            |
   | ------------ | ------------------ | -------------------- |
   | Basics       | Subscription       | Leave default **(1)**       |
   |              | Resource Group     | **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> (2)** |
   |              | Name               | **CoreServicesVNet (3)**     |
   |              | Region             | **(US) East US (4)**         |

   ![](../media/azn1.png) 

1. On the **IP Address** tab of **Create virtual network** use the information:

   - Remove the default IP Address space by clicking on **Delete the address space**

     ![](../media/unit4-image2.png)

   - After deleting **address space**, select **Add IPV4 Address space** use the information in the following table. 

     ![](../media/unit4-image3.png)

      |    **Tab**      | **Option**         | **Value**            |
      | --------------  | -------------------- | -------------------|
      | IP Addresses    | IPv4 address space | **10.20.0.0   (1)**         |
      |                 | IPv4 address Size  | **/16         (2)**         |

      ![](../media/l1-3.png)

1. Use the information in the following table to create the CoreServicesVnet subnets, to begin creating each subnet on the **Create virtual network** page, select **+ Add a subnet**.

    ![](../media/azn2.png)

1. To finish creating each subnet, select **Add**. If needed, **Edit** (pencil icon) or **Delete** the default subnet.

    | **Subnet**             | **Option**           | **Value**              |
    | ---------------------- | -------------------- | ---------------------- |
    | GatewaySubnet          | Subnet purpose       | Select **Virtual Network Gateway** |
    |                        | Name                 | GatewaySubnet          |
    |                        | Starting address     | 10.20.0.0              |
    |                        | Size                 | /27                    |
    | SharedServicesSubnet   | Subnet name          | SharedServicesSubnet   |
    |                        | Starting address     | 10.20.10.0             |
    |                        | Size                 | /24                    |
    | DatabaseSubnet         | Subnet name          | DatabaseSubnet         |
    |                        | Starting address     | 10.20.20.0             |
    |                        | Size                 | /24                    |
    | PublicWebServiceSubnet | Subnet name          | PublicWebServiceSubnet |
    |                        | Starting address     | 10.20.30.0             |
    |                        | Size                 | /24                    |

1. To finish creating the CoreServicesVnet and its associated subnets, select **Review + create**. 

      ![](../media/azn3.png)

1. Select **Create**.      

1. Once, the creation of the **CoreServicesVnet** is completed, select **Go to resource**.

      ![](../media/azn4.png)

1. Verify your configuration passed validation, Go back to virtual network and then again select **+ Create**.

    ![](../media/l1-4.png)
 
1. Repeat steps 1 -8 for each VNet based on the tables below mentioned in **Task 2** and **Task3**.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="d51cb8e1-a452-4f19-86d3-a6bb63c41bb1" />

### Task 2: Create the ManufacturingVnet virtual network and subnets

In this task, you'll be setting up the ManufacturingVnet virtual network and its associated subnets, similar to what you did in previous task. 

1. On **Basic** tab of **Create virtual network** use the information in the following table to create the **ManufacturingVnet** virtual network and select **IP 
   Address (5)** tab.

   | **Tab**      | **Option**         | **Value**             |
   | ------------ | ------------------ | --------------------- |
   | Basics       | Subscription     | Leave the default one **(1)**  |   
   |        | Resource Group     | **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> (2)**  |
   |              | Name               | **ManufacturingVnet (3)**     |
   |              | Region             | **(Europe) West Europe (4)**  |

   ![](../media/azn5.png)   

1. Delete the existing ip address in the IP adress Tab.

1. Add the below ip adress with the subnets as performed in the previous task.   

    | **IP/Subnet**                | **Option**           | **Value**                 |
    | ------------------------- | -------------------- | ------------------------- |
    | IP Addresses              | IPv4 address space   | 10.30.0.0/16              |
    | ManufacturingSystemSubnet | Name                 | ManufacturingSystemSubnet |
    |                           | Starting address     | 10.30.10.0                |
    |                           | Size                 | /24                       |
    | SensorSubnet1             | Name                 | SensorSubnet1             |
    |                           | Starting address     | 10.30.20.0                |
    |                           | Size                 | /24                       |
    | SensorSubnet2             | Name                 | SensorSubnet2             |
    |                           | Starting address     | 10.30.21.0                |
    |                           | Size                 | /24                       |
    | SensorSubnet3             | Name                 | SensorSubnet3             |
    |                           | Starting address     | 10.30.22.0                |
    |                           | Size                 | /24                       |

1. To finish creating the CoreServicesVnet and its associated subnets, select **Review + create**.

    ![](../media/azn6.png)

1. Select **Create**.

1. Verify your configuration passed validation.

1. Go back to **virtual network** and then again select **+ Create**.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="78f4c13b-e110-4236-9f30-a8bcbbf5d2a5" />

### Task 3: Create the ResearchVnet virtual network and subnets

In this task, you'll create the ResearchVnet virtual network and its subnet.

1. On **Basic** tab of **Create virtual network** use the information in the following table to create the **ResearchVnet** virtual network and select **IP 
   Address (5)** tab.

   | **Tab**      | **Option**         | **Value**            |
   | ------------ | ------------------ | -------------------- |
   | Basics       | Subscription     | Leave the default one **(1)** |   
   |       | Resource Group     | **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> (2)** |
   |              | Name               | **ResearchVnet (3)**         |
   |              | Region             | **Southeast Asia (4)**       |

   ![](../media/azn7.png)   

1. Delete the existing ip address in the IP adress Tab.

1. Add the below ip adress with the subnets as performed in the previous task.    

   | **IP/Subnet**        | **Option**           | **Value**            |
   | -------------------- | -------------------- | -------------------- |
   | IP Addresses         | IPv4 address space   | 10.40.0.0/16         |
   | ResearchSystemSubnet | Name                 | ResearchSystemSubnet |
   |                      | Starting address     | 10.40.0.0            |
   |                      | Size                 | /24                  |

1. To finish creating the CoreServicesVnet and its associated subnets, select **Review + create**.

    ![](../media/azn8.png)

1. Select **Create**.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="69271e47-83aa-448c-896f-5d5a42ba4ee0" /> 

### Task 4: Verify the creation of VNets and Subnets

In this task, you'll be verifying the creation of the virtual networks (VNets) and subnets. 

1. On the Azure portal home page, from top left corner of page click **Show portal menu (1)** and select **All resources (2)**.

      ![](../media/azn9.png)

1. Verify that the **CoreServicesVnet, ManufacturingVnet, and ResearchVnet** are listed.

      ![](../media/azn10.png)

1. Select **CoreServicesVnet**. 

      ![](../media/azn11.png)

1. In CoreServicesVnet, from the left navigation pane, under **Settings**, select **Subnets (1)**.

1. In CoreServicesVnet | Subnets, verify that the subnets you created are listed, and that the IP address ranges are correct **(2)**.

      ![](../media/azn12.png)

1. Repeat steps 3 - 5 and select **ManufacturingVnet**, and **ResearchVnet** Virtual Network to verify the subnets.

## Key takeaways

+ Azure Virtual Network is a service that provides the fundamental building block for your private network in Azure. An instance of the service (a virtual network) enables many types of Azure resources to securely communicate with each other, the internet, and on-premises networks. Ensure nonoverlapping address spaces. Make sure your virtual network address space (CIDR block) doesn't overlap with your organization's other network ranges.
+ All Azure resources in a virtual network are deployed into subnets within the virtual network. Subnets enable you to segment the virtual network into one or more subnetworks and allocate a portion of the virtual network's address space to each subnet. Your subnets shouldn't cover the entire address space of the virtual network. Plan ahead and reserve some address space for the future.

## Review

In this lab, you have completed:

+ Created the CoreServicesVnet virtual network and subnets
+ Created the ManufacturingVnet virtual network and subnets
+ Created the ResearchVnet virtual network and subnets
+ Verifyed the creation of VNets and Subnets

## You have successfully completed the lab.
