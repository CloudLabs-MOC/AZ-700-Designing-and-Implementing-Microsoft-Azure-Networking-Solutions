# Lab 3a: Configure an ExpressRoute Gateway

## Lab Overview

To connect your Azure virtual network and your on-premises network via ExpressRoute, you must create a virtual network gateway first. A virtual network gateway serves two purposes: to exchange IP routes between the networks and to route network traffic. To send network traffic on a private connection, you use the gateway type 'ExpressRoute'. This is also referred to as an ExpressRoute gateway and is the type of gateway used when configuring ExpressRoute.

## Lab Objectives

In this lab, you will complete the following tasks:

+ Task 1: Create the VNet and gateway subnet
+ Task 2: Create the virtual network gateway

## Estimated time: 60 Minutes

## Architecture diagram

   ‎![](../media/az700-m3-unit4.png)

## Task 1: Create the VNet and gateway subnet

In this task, you will create the VNet and gateway subnet, you are setting up a Virtual Network (VNet) and configuring the necessary gateway subnet to support Azure VPN Gateway functionality. 

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual networks (1)**, and then select **Virtual 
   networks (2)** under services.

    ![](../media/lab02-az700-ima10.png)
   
1. On the Virtual networks page, select **+ Create**.

1. On the **Create virtual networks** blade, on the **Basics** tab, use the information in the following table to create the VNet:

   | **Setting**          | **Value**                        |
   | -------------------- | -------------------------------- |
   | Subscription         | Leave default                    |
   | Resource Group       | **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>**|
   | Virtual network name | CoreServicesVNet                 |
   | Region               | East US                          |

1. To add IPv4 address space, on the **IP Addresses (1)** tab, perform the following sub-steps:
      - Click on **Add IPv4 address space (2)** in new address space box.
      - Enter **10.20.0.0 (3)** in address space field.
      - Enter **/16 (4)** in size filed.
      - Then click on the **+ Add a subnet (5)** button.

         ![Azure portal - add gateway subnet](../media/lab02-az700-ima22.png)

1. In the **Add a subnet** pane, use the information in the following table to create the subnet and then click on **Add (6)**.

   | **Setting**                  | **Value**     |
   | ---------------------------- | ------------- |
   | Subnet purpose               | Select **Virtual Network Gateway (1)** |
   | Name                         | **GatewaySubnet (2)**  |
   | IPv4 address range           | Select **10.20.0.0/16 (3)**  |
   | Starting address             | **10.20.0.0 (4)**     |
   | Size                         | **/27 (5)**           |

    ![](../media/lab02-az700-ima23.png)

1. On the Create virtual network page, select **Review + create**.

1. Confirm that the VNet passes the validation and then select **Create**.

   >**Note**: If you are using a dual stack virtual network and plan to use IPv6-based private peering over ExpressRoute, select Add IP6 address space and input IPv6 address range values.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="4b8c0911-f548-4fa0-ad62-298ff78aa0e5" />

## Task 2: Create the virtual network gateway

In this task, you will create the virtual network gateway, you're setting up an Azure Virtual Network Gateway to enable connectivity for your virtual network. 

1. On any Azure Portal page, in **Search resources, services and docs (G+/)**, enter **Virtual network gateways**, and then select **Virtual network gateways** from the results.

    ![](../media/lab02-az700-ima24.png)

1. On the Virtual network gateways page, select **+ Create**.

1. On the **Create virtual network gateway** page, use the information in the following table to create the gateway:

   **Note:** First try to select **Region** to get virtual network option to choose. It might take upto 15 minutes for the newly created virtual network option to show up. 

   | **Setting**               | **Value**                  |
   | ------------------------- | -------------------------- |
   | **Project details**       |                            |
   | Resource Group            | **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/>** (Auto-selected)     |
   | **Instance details**      |                            |
   | Name                      | CoreServicesVnetGateway  **(1)**  |
   | Region                    | East US **(2)**                   |
   | Gateway type              | ExpressRoute **(3)**               |
   | SKU                       | Standard    **(4)**               |
   | Virtual network           | CoreServicesVNet   **(5)**        |
    
1. Select **Review + create (6)**.

     ![](../media/l3-u4-1.png)
   
1. Confirm that the Gateway configuration passes validation and then select **Create**.

1. When the deployment is complete, select **Go to Resource**.

   >**Note:** it can take up to 45 minutes to deploy a Gateway.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully completed the task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="15a76eb7-8705-4ab0-8c7d-c006a7ba304f" />

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 
+ Azure ExpressRoute allows an organization to connect their on-premises networks directly into the Microsoft Azure and Microsoft 365 clouds. Azure ExpressRoute uses a dedicated high-bandwidth connection provided by a Microsoft partner.
+ Microsoft guarantees a minimum of 99.95% availability for ExpressRoute dedicated connections. The connection is private and travels over a dedicated line, third parties can't intercept the traffic.
+ You can create a connection between your on-premises network and the Microsoft cloud in four different ways, CloudExchange Co-location, Point-to-point Ethernet Connection, Any-to-any (IPVPN) Connection, and ExpressRoute Direct.
+ ExpressRoute features is determined by the SKU: Local, Standard, and Premuium. 

### Review
In this lab, you have completed:

- Created the VNet and gateway subnet
- Created the virtual network gateway
  
## You have successfully completed the lab.
