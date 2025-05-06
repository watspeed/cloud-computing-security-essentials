## Lab introduction

In this lab you learn to create storage accounts for Azure blobs and Azure files. You learn to configure and secure blob containers. You also learn to use Storage Browser to configure and secure Azure file shares. 


## Lab scenario

Your organization is currently storing data in on-premises data stores. Most of these files are not accessed frequently. You would like to minimize the cost of storage by placing infrequently accessed files in lower-priced storage tiers. You also plan to explore different protection mechanisms that Azure Storage offers, including network access, authentication, authorization, and replication. Finally, you want to determine to what extent Azure Files is suitable for hosting your on-premises file shares.

## Interactive lab simulations

There are interactive lab simulations that you might find useful for this topic. The simulation lets you to click through a similar scenario at your own pace. There are differences between the interactive simulation and this lab, but many of the core concepts are the same. An Azure subscription is not required. 

+ [Create blob storage](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%205). Create a storage account, manage blob storage, and monitor storage activities. 
  
+ [Manage Azure storage](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2011). Create a storage account and review the configuration. Manage blob storage containers. Configure storage networking. 


## Lab objectives
In this lab, you will complete the following tasks:
+ Task 1: Create and configure a storage account. 
+ Task 2: Create and configure secure blob storage.
+ Task 3: Create and configure secure Azure file storage.

  
Estimated timing: 45 minutes

## Architecture diagram

  ![Diagram of the tasks.](../images/lab4-13.png)

## Task 1: Create and configure Azure Storage accounts
In this task, you will create and configure an Azure Storage account.

1. On the Azure Portal page, in the **Search resources, services, and docs (G+/)** box at the top of the portal, enter **Storage accounts**, and then select **Storage accounts** under services.

    ![image](../images/lab4-14.png)

1. On the **Storage accounts** blade, click **+ Create**.
   
1. On the **Basics** tab of the **Create storage account** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group | the name of an **existing** resource group **az104-07-rg1** |
    | Storage account name | **strgaz104t07<inject key="DeploymentID" enableCopy="false" />** |
    | Region | **<inject key="Region" enableCopy="false"/>**  |
    | Performance | **Standard** |
    | Redundancy | **Geo-redundant storage (GRS)** |

1. Click **Next**, and on the **Advanced** tab of the **Create a storage account** blade, review the available options, accept the defaults, and click **Next**.

1. On the **Networking** tab of the **Create a storage account** blade, review the available options and select **Disable public access and use private access** under network access. Click **Next**.

1. On the **Data protection** tab of the **Create a storage account** blade, review the available options and accept the defaults. Click **Review + Create** and wait for the validation process to complete. Then click on **Create**.

    >**Note**: Wait for the Storage account to be created. This should take about 2 minutes.

1. On the deployment blade, click **Go to resource** to display the Azure Storage account blade.

1. On the Storage account blade, in the **Data management (1)** section, click **Redundancy (2)** and note the secondary location **(3)**.

   ![image](../images/lab4-dm.png)

1. In the same blade **Redundancy** drop-down list select **Locally redundant storage (LRS) (1)** and save the change **(2)**. 

    ![image](../images/lab4-15.png)

1. In the same blade note that, at this point, the Storage account has the primary location.

1. Select **Configuration** blade under **Settings** section of the Storage account, set **Blob access tier (default)** to **Cool**, and save the change.

    ![image](../images/lab4-16.png)

    > **Note**: The cool access tier is optimal for data that is not accessed frequently.

1. Review the **Overview** blade and the additional configurations that can be changed. These are global settings for the storage account. Notice the storage account can be used for Blob containers, File shares, Queues, and Tables.

1. In the **Security + Networking** section, select **Networking**. Notice public network access is disabled.

    + Change the **Public access level** to **Enabled from all networks**.
    + Be sure to **Save** your changes. 

1. In the **Data management** section, view the **Redundancy** blade. Notice the information about your primary data center locations.

1. In the **Data management** section, select **Lifecycle management**, and then select **Add a rule**.

    + **Name** the rule `Movetocool`. Notice your options for limiting the scope of the rule.
  
      ![image](../images/lab4-17.png)
    
    + On the **Base blobs** tab, create the following rule: *if* based blobs were *last modified* more than *30 days* ago *then* **move to cool storage**. Notice your other choices. Notice you can configure other conditions. Select **Add** when you are done exploring.

      ![image](../images/lab4-18.png)
      
### You have successfully completed the task

To confirm your completion and receive credit for this task, go to the **Lab Validation** tab in the navigation bar at the top of this guide and click the **Validate** button for **Storage Account Creation, Region, and Public Access**. If your task validation failed, read the validation information for more details, and try the steps in the task again.

## Task 2: Create and configure secure blob storage

In this task, you will create a blob container and upload a blob into it.

### Create a blob container and a time-based retention policy

1. On the Storage account blade, in the **Data storage** section, click **Containers**.

1. Click **+ Container** and create a container with the following settings:

    | Setting | Value |
    | --- | --- |
    | Name | **az104-07-container**  |
    | Public access level | **Private (no anonymous access)** |

1. Click **Create**.

1. On your container, scroll to the ellipsis (...) on the far right, select **Access Policy**.

     ![image](../images/lab4-19.png)

1. On the **Access policy** window under the **Immutable blob storage** area, select **Add policy**.

    | Setting | Value |
    | --- | --- |
    | Policy type | **Time-based retention**  |
    | Set retention period for | `180` days |
    | Select **Save** |
   
     ![image](../images/lab4-20.png)

### Manage blob uploads

1. Returning to the list of containers, click on **az104-07-container**. Then, click **Upload**.

1. Within the **Upload blob** pane click on **Browse for files**. Browse to **C:\AllFiles\AZ-104-MicrosoftAzureAdministrator-Lab-Files\Allfiles\Labs\07\\LICENSE** on your lab computer and click **Open**.

1. On the **Upload blob** blade, expand the **Advanced** section and specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Blob type | **Block blob** |
    | Block size | **4 MB** |
    | Access tier | **Hot** |
    | Upload to folder | **licenses** |

    ![image](../images/lab4-21.png)
   
    > **Note**: Access tier can be set for individual blobs.

1. Click **Upload**.

    > **Note**: Note that the upload automatically created a subfolder named **licenses**.

1. Back on the **az104-07-container** blade, click **licenses** and then click **LICENSE**.

1. On the **licenses/LICENSE** blade, review the available options.

    > **Note**: You have the option to download the blob, change its access tier (it is currently set to **Hot**), and acquire a lease, which would change its lease status to **Locked** (it is currently set to **Unlocked**) and protect the blob from being modified or deleted, as well as assign custom metadata (by specifying an arbitrary key and value pairs). You also have the ability to **Edit** the file directly within the Azure portal interface, without downloading it first. You can also create snapshots, as well as generate a SAS token (you will explore this option in the next task).
     

1. On the **licenses/LICENSE** blade, on the **Overview** tab, click the **Copy to clipboard** button next to the **URL** entry.

1. Open another browser window by using InPrivate mode and navigate to the URL you copied in the previous step.

1. You should be presented with an XML-formatted message stating **ResourceNotFound** or **PublicAccessNotPermitted**.

    > **Note**: This is expected since the container you created has the public access level set to **Private (no anonymous access)**.

1. Close the InPrivate mode browser window, return to the browser window showing the **licenses/LICENSE** blade of the Azure Storage container, and switch to the **Generate SAS** tab.

### Configure limited access to the blob storage

1. On the **Generate SAS** tab of the **licenses/LICENSE** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Signing key | **Key 1** |
    | Permissions | **Read** |
    | Start date | yesterday's date |
    | Start time | current time |
    | Expiry date | tomorrow's date |
    | Expiry time | current time |
    | Allowed IP addresses | leave blank |

    ![image](../images/lab4-22.png)
   
1. Click **Generate SAS token and URL**.

1. Scroll down, then click the **Copy to clipboard** button next to the **Blob SAS URL** entry.

    ![image](../images/lab4-clipboard.png)

1. Open another browser window by using InPrivate mode and navigate to the URL you copied in the previous step.

    > **Note**: This will automatically download the LICENSE file. You can view the content of the file by opening it with Notepad.

    > **Note**: This is expected since now your access is authorized based on the newly generated SAS token.

### You have successfully completed the task

To confirm your completion and receive credit for this task, go to the **Lab Validation** tab in the navigation bar at the top of this guide and click the **Validate** button for **Create and configure secure blob storage**. If your task validation failed, read the validation information for more details, and try the steps in the task again.

## Task 3: Create and configure an Azure File storage
In this task, you will create and configure Azure Files shares.

1. In the Azure portal, navigate back to the blade of the storage account you created in the task 2 of this lab and, in the **Data storage** section, click **File shares**.

1. Click **+ File share** and create a file share with the following settings.

    | Setting | Value |
    | --- | --- |
    | Name | **az104-07-share** |
    | Tier | **Transaction optimized** |

1. Click **Next: Backup**, then in the Backup tab, uncheck **Enable backup** option and then click on **Review + Create**, then **Create**.

1. Click the newly created file share and note the information available on the **az104-07-share** blade.

### Explore Storage Browser and upload a file

1. Return to your storage account and select **Storage browser**. The Azure Storage Browser is a portal tool that lets you quickly view all the storage services under your account.

1. Select **File shares** and verify your **az104-07-share** directory is present.

1. Select your **az104-07-share** directory and notice you can **+ Add directory**. This lets you create a folder structure. Provide name **az104-07-folder** and click on **OK**

    ![image](../images/lab4-23.png)

1. Open file explorer and in the Documents folder create a text document named **az104-07-file.txt**. The file can remain blank inside.

1. Navigate to **File shares** and select **az104-07-share**, then click into your **az104-07-folder** folder.

1. Select **Upload**. Browse to your new file **az104-07-file.txt**, and then click **Upload**.

    >**Note**: You can view file shares and manage those shares in the Storage Browser. There are currently no restrictions.

1. Verify that **az104-07-file.txt** appears in the list of files in the **az104-07-folder** folder.
    
### You have successfully completed the task

To confirm your completion and receive credit for this task, go to the **Lab Validation** tab in the navigation bar at the top of this guide and click the **Validate** button for **Verify File Upload in Azure File Share Folder**. If your task validation failed, read the validation information for more details, and try the steps in the task again.

### Restrict network access to the storage account

1. In the **Search resources, services, and docs (G+/)** box at the top of the portal, search for and select **Virtual networks**.

1. Select **+ Create**. Select your resource group **az104-07-rg1** and give the virtual network a **name**, `vnet1`.

1. Take the defaults for other parameters, select **Review + create**, and then **Create**.

1. Wait for the virtual network to deploy, and then select **Go to resource**.

1. Return to your **Storage account** named **strgaz104t07<inject key="DeploymentID" enableCopy="false" />**

1. In the **Security + networking** section, select the **Networking** blade.

1. Under **Firewalls and virtual networks** tab, for **Public network access** select **Enabled from selected virtual networks and IP addresses**. 

1. Select **Add existing virtual network** and select **vnet1** and **default** subnet.

1. You now need to enable the **Microsoft.Storage** service endpoint for the **default** subnet. Click **Enable** at the bottom of the blade and wait for the service endpoint to enable. Then, click **Add**. 

1. Be sure to **Save** your changes at the top of the tab before navigating away.

    >**Note:** The storage account should now only be accessed from the virtual network you just created. 

1. Select the **Storage browser** blade and **Refresh** the page. Then click on **Blob containers** and select **az104-07-container**.  

    >**Note:** You should receive a message *not authorized to perform this operation*, since you are not connecting from the virtual network. It may take a couple of minutes for this to take effect, so refresh the page if you do not receive this message.

     ![image](../images/lab4-24.png)

### You have successfully completed the task

To confirm your completion and receive credit for this task, go to the **Lab Validation** tab in the navigation bar at the top of this guide and click the **Validate** button for **VNet Exists & External Access Blocked**. If your task validation failed, read the validation information for more details, and try the steps in the task again.

## Learn more with self-paced training

+ [Optimize your cost with Azure Blob Storage](https://learn.microsoft.com/training/modules/optimize-your-cost-azure-blob-storage/). Learn how to optimize your cost with Azure Blob Storage.
+ [Control access to Azure Storage with shared access signatures](https://learn.microsoft.com/training/modules/control-access-to-azure-storage-with-sas/). Grant access to data stored in your Azure Storage accounts securely by using shared access signatures.

## Review
In this lab, you have completed:
- Create and configure a storage account. 
- Create and configure secure blob storage.
- Create and configure secure Azure file storage.

## You have successfully completed the lab

To confirm your completion and receive credit for this lab, go to the **Lab Validation** tab in the navigation bar at the top of this guide and confirm that all four tasks have validated.
