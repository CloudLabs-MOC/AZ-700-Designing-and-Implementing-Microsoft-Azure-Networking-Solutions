# Lab 5b: Create a Front Door for a highly available web application using the Azure portal

## Lab Overview
In this lab, you will set up an Azure Front Door configuration that pools two instances of a web application that runs in different Azure regions. This configuration directs traffic to the nearest site that runs the application. Azure Front Door continuously monitors the web application. You will demonstrate automatic failover to the next available site when the nearest site is unavailable.

## Lab Objectives
In this lab, you will complete the following tasks:

+ Task 1: Create two instances of a web app
+ Task 2: Create a Front Door for your application
+ Task 3: View Azure Front Door in action (Ready only)

## Estimated time: 30 Minutes

## Architecture diagram

![Network configuration for Azure Front Door.](../media/front-door-environment-diagram.png)

## Task 1: Create two instances of a web app

In this task, you'll create two instances of a web application hosted in two different Azure regions, both running in Active/Active mode. These web apps will be able to handle traffic independently, ensuring high availability.

This task requires two instances of a web application that run in different Azure regions. Both the web application instances run in Active/Active mode, so either one can take traffic. This configuration differs from an Active/Stand-By configuration, where one acts as a failover.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **WebApp (1)**, and then select **App Services (2)** under services.

   ![Web App](../media/l5-u6-1.png)

1. Select **+ Create (1)**  and then select **Web App (2)** to create a Web App.

   ![Web App](../media/l5-u6-2.png)

1. On the Create Web App page, on the **Basics** tab, enter or select the following information, and select **Review + create (10)**.

   | **Setting**      | **Value**                                                    |
   | ---------------- | ------------------------------------------------------------ |
   | Subscription     | Select your subscription .                                    |
   | Resource group   | Select **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> (1)**                |
   | Name             | **WebAppContoso-1-<inject key="DeploymentID" enableCopy="false"/> (2)** |
   | Try a secure unique default hostname  | **Disable (3)** the toggle      |
   | Publish          | Select **Code (4)**                                             |
   | Runtime stack    | Select **.NET 8 (LTS) (5)**                                     |
   | Operating System | Select **Windows (6)**                                          |
   | Region           | Select **Central US (7)**                                       |
   | Windows Plan     | Select **Create new** and enter **myAppServicePlanCentralUS (8)** in the text box |
   | Princing Plan    | Select **Standard S1 100 total ACU, 1.75 GB memory (9)**        |

   ![Web App](../media/l5-u6-9.png)

1. Review the Summary, and then select **Create**.

    >**Note:** It might take 2 minutes for the deployment to complete.

1. Create a second web app,  in **Search resources, services and docs (G+/)** box at the top of the portal, enter **WebApp**, and then select **App Services** under services.

1. Select **+ Create**  and then select **Web App** to create a Web App.

1. On the Create Web App page, on the **Basics** tab, enter or select the following information, and select **Review + create (10)**.

   | **Setting**      | **Value**                                                    |
   | ---------------- | ------------------------------------------------------------ |
   | Subscription     | Select your subscription.                                    |
   | Resource group   | Select **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> (1)**               |
   | Name             | **WebAppContoso-2-<inject key="DeploymentID" enableCopy="false"/> (2)** |
   | Try a secure unique default hostname  | **Disable (3)** the toggle      |
   | Publish          | Select **Code (4)**                                             |
   | Runtime stack    | Select **.NET 8 (LTS) (5)**                                     |
   | Operating System | Select **Windows (6)**                                          |
   | Region           | Select **East US 2 (7)**                                          |
   | Windows Plan     | Select **Create new** and enter **myAppServicePlanEastUS (8)** in the text box. |
   | Pricing Plan     | Select **Standard S1 100 total ACU, 1.75 GB memory (9)**        |
   |||

   ![Web App](../media/l5-u6-4.png)

1. Review the Summary, and then select **Create**.

   **Note:** Wait for deployment to complete it might take several minutes.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task.
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="f59939fe-eba5-483e-8067-81aa03a7bc70" />

## Task 2: Create a Front Door for your application

In this task, you will configure Azure Front Door to direct user traffic to the two web applications you created earlier based on the lowest latency. Front Door is a global service that provides high availability and low latency for applications by routing traffic to the closest available endpoint.

1. On any Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, Search for **Front Door and CDN profiles (1)**, and then select **Front Doors (2)**.

   ![Web App](../media/azn110.png)

1. On the **Front Door and CDN profiles** page, select **+ Create**.

   ![Web App](../media/azn111.png)

1. On the Compare offerings page, select **Quick create (1)**. Then select **Continue to create a Front Door (2)**.

    ![Web App](../media/l5-u6-3.png)

1. On the Basics tab, enter or select the following information.

      | **Setting**             | **Value**                                    |
      | ----------------------- | -------------------------------------------- |
      | Subscription            | Select your subscription **(1)**.                    |
      | Resource group          | Select **ContosoResourceGroup-<inject key="DeploymentID" enableCopy="false"/> (2)**                  |
      | Resource group location | Accept default setting                      |
      | Name                    | **FrontDoor<inject key="DeploymentID" enableCopy="false"/> (3)**   |
      | Tier                    | **Standard (4)**  |
      | Endpoint Name           | **FDendpoint (5)**  |
      | Origin Type             | **App Services (6)** | 
      | Origin host name        | **WebAppContoso-1-<inject key="DeploymentID" enableCopy="false"/> (7)** |
      |||

1. Select **Review and Create (8)**, and then select **Create**.

   ![Web App](../media/l5-u6-5.png)

1. Wait for the resource to deploy, and then select **Go to resource**.

1. On the Front Door resource in the Overview blade, select the **Origin groups (1)** under **Settings**, to update select the origin group **default-origin-group (2)** from the list and click on **+ Add an origin (3)**

   ![Web App](../media/l5-u6-6.png)

1. On the **Add on Origin** window, add the following information and select **Add (5)**.

   | **Setting**             | **Value**                                    |
   | ----------------------- | -------------------------------------------- |
   | Name                    | **default-origin-group (1)**                         |
   | Origin type             | **App services (2)**                                  |
   | Host name               | Select **WebAppContoso-2-<inject key="DeploymentID" enableCopy="false"/> (3)** |
   | Origin host header      | Select **WebAppContoso-2-<inject key="DeploymentID" enableCopy="false"/> (4)** |
   |||

   ![Web App](../media/l5-u6-7.png)

1. Select **Update**.

   ![Web App](../media/l5-u6-8.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help.

   <validation step="f5cf3eee-49ad-4985-88ca-5bc0c80e8ddc" />

## Task 3: View Azure Front Door in action (Read only)

In this task, you will verify that Azure Front Door is working as expected by testing the global failover capabilities. After you create the Front Door, it will take a few minutes for the configuration to propagate across all regions. Once that's done, you'll test the Front Door's ability to direct traffic to the available web app instance.

1. Navigate back to **Front Door and CDN profiles** page, on the Front Door resource in the **Overview (1)** blade, under **Properties (1)** tab locate the endpoint hostname that is created for your endpoint. This should be fdendpoint followed by a hyphen and a random string. For example, **fdendpoint-fxa8c8hddhhgcrb9.z01.azurefd.net**. **Copy** this FQDN. Copy the endpoint **(3)**.

    ![Web App](../media/azn114.png)

1. In a new browser tab, navigate to the Front Door endpoint FQDN. The default App Service page will be displayed.

    ![Web App](../media/l5u6-12.png)

     >**Note**: Due to AFD service management restrictions being lifted, configuration changes may take up to 45 minutes to fully propagate. During this time, you may encounter a **Page not found** error. No need to wait please go through the lab content.

1. To test instant global failover in action, try the following steps:

1. Switch to the Azure portal, search for and select **App services**.

1. Select one of your web apps **(1)**, then select **Stop (2)**. 

    ![Web App](../media/azn115.png)

1. Select **Yes** to verify.

    ![Web App](../media/azn116.png)

1. Switch back to your browser and select Refresh. You should see the same information page.

    **Note**: There may be a delay while the web app stops. If you get an error page in your browser, refresh the page.

1. Switch back to the Azure Portal, locate the other web app, and stop it.

1. Switch back to your browser and select Refresh. This time, you should see an error message.

    ![Web App](../media/l5u6-14.png)
    ![Web App](../media/azn117.png)      

Congratulations! You have configured and tested an Azure Front Door.

## Extend your learning with Copilot

Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to *copilot.microsoft.com*. Take a few minutes to try these prompts.
+ What are the differences between Azure Application Gateway and Azure Front Door? Provide examples where I would use each product.
+ Provide a checklist of things to do when configuring Azure Front Door.
+ What is an origin in Azure Front Door and how is it different from an endpoint?


## Learn more with self-paced training

+ [Introduction to Azure Front Door](https://learn.microsoft.com/training/modules/intro-to-azure-front-door/). In this module, you learn how Azure Front Door can protect your applications.
+ [Load balance your web service traffic with Front Door](https://learn.microsoft.com/training/modules/create-first-azure-front-door/). In this module, you learn to create and configure Azure Front Door. 

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 
+ Azure Front Door is a cloud-based service that delivers your applications anywhere across the globe. 
+ Azure Front Door uses layer 7 load balancing to distribute traffic across multiple regions and endpoints.
+ Azure Front Door supports different traffic routing methods to determine how your HTTP/HTTPS traffic is distributed. The routing methods are: latency, priority, weighted, and session affinity. 

### Review
In this lab, you have completed:

- Create two instances of a web app
- Create a Front Door for your application
- View Azure Front Door in action

## You have successfully completed the lab.
