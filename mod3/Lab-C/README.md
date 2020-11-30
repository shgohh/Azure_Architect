# Lab: Azure Automation

# Deploying Configuration Management solutions to Azure

## Exercise 1: Deploy compute resources

#### Task 1: Open the Azure portal

1. In the open browser window, navigate to the **Azure Portal** (<https://portal.azure.com>).

1. When prompted, authenticate with the user account account that has the owner role in the Azure subscription you will be using in this lab.

#### Task 2: Open Cloud Shell

1. At the top of the portal, click the **Cloud Shell** icon to open a new shell instance.

    > **Note**: The **Cloud Shell** icon is a symbol that is constructed of the combination of the *greater than* and *underscore* characters.

1. If this is your first time opening the **Cloud Shell** using your subscription, you will see a wizard to configure **Cloud Shell** for first-time usage. When prompted, in the **Welcome to Azure Cloud Shell** pane, click **Bash (Linux)**.

    > **Note**: If you do not see the configuration options for **Cloud Shell**, this is most likely because you are using an existing subscription with this course's labs. If so, proceed directly to the next task.

1. Wait for the **Cloud Shell** to finish its setup procedures before you proceed to the next task.

#### Task 3: Deploy a Linux VM

1. In the **Cloud Shell** pane, click the **Upload/Download files** icon and, in the drop-down menu, click **Upload**.

1. In the **Open** dialog box, navigate to the **\\allfiles\\AZ-301T02\\Module_03\\LabFiles\\Starter\\** folder, select the **linux-template.json** file, and click **Open**. 

1. At the **Cloud Shell** command prompt, type in the following command and press **Enter** to create a variable which value designates the name of the resource group that will contain the virtual virtual machine:

    ```sh
    RESOURCE_GROUP='AADesignLab1202-RG'
    ```

1. At the **Cloud Shell** command prompt, type in the following command and press **Enter** to create a variable which value designates the Azure region you will use for the deployment :

    ```sh
    LOCATION='southeastasia'
    ```

1. At the **Cloud Shell** command prompt, type in the following command and press **Enter** to create a new resource group:

    ```sh
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```

1. At the **Cloud Shell** command prompt, type in the following command and press **Enter** to deploy the Azure Resource Manager template with the specified parameters file:

    ```sh
    az deployment group create --resource-group $RESOURCE_GROUP --template-file ~/linux-template.json --parameters password=Pa55w.rd1234
    ```

1. Do not wait for the deployment to complete before you proceed to the next task.

#### Task 4: Deploy an Azure Automation account

1. In the upper left corner of the Azure portal, click **Create a resource**.

1. At the top of the **New** blade, in the **Search the Marketplace** text box, type **Automation** and press **Enter**.

1. On the **Everything** blade, in the search results, click **Automation**.

1. On the **Automation** blade, click **Create**.

1. On the **Add Automation Account** blade, perform the following tasks:

    - In the **Name** text box, type **LinuxAutomation**.

    - Leave the **Subscription** drop-down list entry set to its default value.

    - In the **Resource group** section, select the **Create new** option and then, in the text box, type **AADesignLab1203-RG**.

    - In the **Location** drop-down list, select Southeast Asia.

    - In the **Create Azure Run As account** section, ensure that **Yes** option is selected.

    - Click the **Create** button.

1. Wait for the provisioning to complete before you proceed to the next task.

> **Review**: In this exercise, you created a Linux VM using an Azure Resource Manager template and provisioned an Azure Automation account from the Azure portal.


## Exercise 2: Configure Azure Automation DSC

#### Task 1: Import Linux PowerShell DSC modules

1. In the hub menu of the Azure portal, click **Resource groups**.

1. On the **Resource groups** blade, click **AADesignLab1203-RG**.

1. On the **AADesignLab1203-RG** blade, click the newly created Azure Automation account.

1. On the **LinuxAutomation** blade, in the **Shared Resources** section on the left side of the blade, click **Modules gallery**.

1. On the **LinuxAutomation | Modules gallery** blade, perform the following tasks:

    - In the **Search** text box, type **nx** and press **Enter**.

    - In the search results, click the **nx** module.

1. On the **nx** blade, click the **Import** button at the top of the blade.

1. On the **Import** blade, click the **OK** button.

1. Wait for the import process to finish before you proceed to the next task. A status message on the **nx Module** blade will indicate that the module was successfully imported.

    > **Note**: This process should take about 2 minutes.

#### Task 2: Create Linux DSC Configuration

1. Navigate back to the **LinuxAutomation** blade.

1. Back on the **LinuxAutomation** blade, in the **Configuration Management** section, click **State configuration (DSC)**.

1. On the **LinuxAutomation | State configuration (DSC)** blade, click the **Configurations** tab.

1. On the **LinuxAutomation | State configuration (DSC)** blade, click the **+ Add** button at the top of the pane.

1. On the **Import** blade, perform the following tasks:

    - Next to the **Configuration file** field, click the blue button with a folder icon.

    - In the **Choose File to Upload** dialog box, navigate to the **\\allfiles\\AZ-301T02\\Module_03\\LabFiles\\Starter\\** folder.

    - Select the **lampserver.ps1** file.

    - Click the **Open** button to close the dialog and return to the **Import** blade.

    - In the **Name** text box, accept the default entry **lampserver**.

    - In the **Description** text box, type **LAMP Server configuration using PHP and MySQL**.

    - Click the **OK** button.

1. Back in the **DSC configurations** pane, click **Refresh** and then click the newly created **lampserver** configuration.

1. On the **lampserver Configuration** blade, click the **Compile** button at the top of the blade. In the confirmation dialog box, click **Yes** to proceed with compiling the configuration.

1. Wait for the compilation task to finish. To determine the status of the compilation task, review the **Status** column of the **Compilation jobs** section of the **lampserver Configuration** blade.

    > **Note**: You may need to close and re-open the blade to see the latest compilation status. This blade does not refresh automatically.

#### Task 3: Onboard Linux VM

1. Navigate back to the **LinuxAutomation - State Configuration (DSC)** blade.

1. Back on the **LinuxAutomation | State Configuration (DSC)** blade, click the **Nodes** tab.

1. On the **LinuxAutomation | State configuration (DSC)** blade, click the **+ Add** button at the top of the pane.

1. On the **Virtual Machines** blade, click the entry representing the Linux virtual machine you deployed in the previous exercise.

1. On the virtual machine blade, click **+ Connect**.

1. On the **Registration** blade, perform the following tasks:

    - Leave the **Registration key** setting with its default value.

    - In the **Node configuration name** drop-down list, select the **lampserver.localhost** entry.

    - Leave all remaining settings with their default values.

    - Click the **OK** button.

1. Wait for the connection process to complete before you proceed to the next step.

1. Navigate back to the **LinuxAutomation | State Configuration (DSC)** blade.

1. On the **LinuxAutomation | State configuration (DSC)** blade, select in the **NODE** section the virtual machine you deployed in the previous exercise.

    > **Note**: You may need to refresh the blade.

1. On the virtual machine blade, click **Assign node configuration**.

1. On the Assign Node Configuration blade, select the node configuration **lampserver.host** and click the **OK** button.

1. Back on the **LinuxAutomation | State Configuration (DSC)** blade, click the **Refresh** button.

1. In the list of DSC nodes, verify that the Linux virtual machine has the **Compliant** status.

    > **Note**: You may need to wait for up to 30 minutes for the new status to be updated.
    
> **Review**: In this exercise, you created a PowerShell DSC configuration and applied the configuration to a Linux virtual machine.

#### Task 4: Validate Linux VM onboarding

1. In the Azure portal, open a Bash session in the **Cloud Shell**.

1. In the Bash session of the Cloud Shell run the following to identify installed packages (provide the password **Pa55w.rd1234** when prompted):

    ```sh
    RESOURCE_GROUP='AADesignLab1202-RG'
    PUBLIC_IP=$(az network public-ip list --resource-group $RESOURCE_GROUP --query "[0].ipAddress" --output tsv)
    ssh Student@$PUBLIC_IP
    sudo yum history
    ps -aux | grep httpd
    ps -aux | grep maria
    exit
    ```
1. Close the **Cloud Shell** pane.
    
## Exercise 3: Remove lab resources

#### Task 1: Open Cloud Shell

1. At the top of the portal, click the **Cloud Shell** icon to open the Cloud Shell pane.

1. At the **Cloud Shell** command prompt at the bottom of the portal, type in the following command and press **Enter** to list all resource groups you created in this lab:

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab12')]".name --output tsv
    ```

1. Verify that the output contains only the resource groups you created in this lab. These groups will be deleted in the next task.

#### Task 2: Delete resource groups

1. At the **Cloud Shell** command prompt, type in the following command and press **Enter** to delete the resource groups you created in this lab

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab12')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

1. Close the **Cloud Shell** prompt at the bottom of the portal.


> **Review**: In this exercise, you removed the resources used in this lab.
