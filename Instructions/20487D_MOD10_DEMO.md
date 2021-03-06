# Module 10: Scaling services

1. Wherever you see a path to file starting at [Repository Root], replace it with the absolute path to the directory in which the 20487 repository resides.
   e.g. - you cloned or extracted the 20487 repository to C:\Users\John Doe\Downloads\20487, then the following path: [Repository Root]\AllFiles\20487D\Mod01 will become C:\Users\John Doe\Downloads\20487\AllFiles\20487D\Mod01
2. Wherever you see **{YourInitials}**, replace it with your actual initials.(for example, the initials for John Do will be jd).
3. Before performing the demonstration, you should allow some time for the provisioning of the different Azure resources required for the demonstration. It is recommended to review the demonstrations before the actual class and identify the resources and then prepare them beforehand to save classroom time.

### Preparation Steps

1. Open **PowerShell** as **Administrator**.
2. In the **User Account Control** modal, click **Yes**.
3. Run the following command: **Install-Module azurerm -AllowClobber -MinimumVersion 5.4.1**.
4. Navigate to **[repository root]\AllFiles\Mod10\DemoFiles\Setup1**
5. Run the following command:
   ```batch
    .\createAzureServices.ps1
   ```
6. You will be asked to supply a **Subscription ID**, which you can get by performing the following steps:
   1. Open a browser and navigate to **http://portal.azure.com**. If a page appears, asking for your email address, type your email address, and then click **Continue**. Wait for the sign-in page to appear, enter your email address and password, and then click **Sign In**.
   2. In the search text box on the top bar, type **Cost** and then in results click **Cost Management + Billing(Preview)**. The **Cost Management + Billing** window should open.
   3. Under **BILLING ACCOUNT**, click **Subscriptions**.
   4. Under **My subscriptions**, you should have at least one subscription. Click on the subscription that you want to use.
   5. Copy the value from **Subscription ID**, and then paste it at the **PowerShell** prompt.
7. In the **Sign in** window that appears, enter your details, and then sign in.
8. In the **Administrator: Windows PowerShell** window, follow the on-screen instructions. Wait for the deployment to complete successfully.
9. Write down the name of the Azure App Service that is created.

# Lesson 1: Introduction to Scalability

### Demonstration: Scaling out with Azure Web Apps

#### Demonstration Steps

1.  Open **Command Line**.
2.  Run the following command to change directory to the starter project:
    ```bash
    cd [Repository Root]\AllFiles\Mod10\DemoFiles\Code\BlueYonder.Hotels.Service\RunCPU
    ```
3.  Run the following command to run the project:
    ```base
    dotnet run
    ```
4.  Wait 5 minutes, and write down the time took to Signing 180000 resarvations.
5.  Open a browser and navigate to **http://portal.azure.com**.
6.  Click on **App Services** on the left menu panel, to display all the **App Services**.
7.  Click on **blueyonderMod10Demo1{YourInitials}** service.
8.  Click on **Scale out (App Service plan)** in **Settings** section.
9.  Click on **Configure** tab.
10. To increase the instances, locate the **Override condition** scale, and increase it to **2**.
11. Click on **Save**.
12. Wait until all changes saved.
13. Switch to **Command Line**.
14. Run the following command to run the project:
    ```base
    dotnet run
    ```
15. Wait 5 minutes, and write down the time took to Signing 180000 resarvations.
    > Note: Now the time is smaller, we have 2 instances thanks to the scaling out.

# Lesson 2: Automatic Scaling

### Demonstration: Configuring auto-scaling for Azure Web Apps

#### Demonstration Steps

1. Redo the preperation steps above, to deploy the azure webapp.
2. Open a browser and navigate to **http://portal.azure.com**.
3. Click on **blueyonderMod10Demo1{YourInitials}** service.
4. Click on **Application Insights** in **Settings** section.
5. Select **Create new resource**.
6. Choose **ASP.Core** in **Runtime/Framework**.
7. Click **OK**, and wait until all changes saved.
8. Click on **Scale out (App Service plan)** in **Settings** section.
9. Click on **Enable autoscale** button.
10. Type a autoscale name in **Autoscale setting name**.
11. Click on **+ Add a rule**, under **Rules** to add a rule:
    - Enter **80** in **Threshold**.
    - Enter **5** in **Duration**.
      Click **Add** and wait until all changes saved.
12. Click on **+ Add a rule**, under **Rules** to add another rule:
    - Choose **Less than** in **Operator**
    - Enter **20** in **Threshold**.
    - Enter **5** in **Duration**.
      Click **Add** and wait until all changes saved.
13. Type **2** in the **Maximum** text block of **Instance limits**.
14. Click **Save**, and wait until all changes saved.
15. Open **Command Line**.
16. Run the following command to change directory to the RunCPU project:
    ```bash
    cd [Repository Root]\AllFiles\Mod10\DemoFiles\Code\BlueYonder.Hotels.Service\RunCPU
    ```

17. Run the following command to run the project:
    ```base
    dotnet run
    ```
18. Wait 5 minutes, and switch to **Azure Portal**.
19. Click on **Run history** tab.
20. In the scaling table, under **OPERATION NAME**, check the value is **Autoscale scale up completed**
21. Wait another 5 minutes
22. In the scaling table, under **OPERATION NAME**, check the value is **Autoscale scale out completed**

# Lesson 3: Azure Application Gateway and Traffic Manager

### Demonstration: Using an Azure Web App behind Application Gateway

#### Demonstration Steps

1.  Open **PowerShell**.
2.  Run the following command to change directory to the Setup folder:
    ```bash
    cd [Repository Root]\AllFiles\Mod10\DemoFiles\Setup3
    ```
3.  Run the following command:
    ```batch
     .\createAzureServices.ps1
    ```
4.  Follow the on-screen instructions. Wait for the deployment to complete successfully.
5.  Open **Microsoft Edge** browser and navigate to **http://portal.azure.com**.
6.  Log in to the **Azure portal**.
7.  Click **Create a resource** found on the upper left-hand corner of the **Azure portal**.
8.  Select **Networking** and then select **Application Gateway** in the Featured list.
9.  Enter these values for the application gateway:
    - **myAppGateway** - for the name of the application gateway.
    - **myResourceGroupAG** - for the new resource group.
10. Accept the default values for the other settings and then click **OK**.
11. Click **Choose a virtual network**, choose **Create new**, and then enter these values for the virtual network:

- **myVNet** - for the name of the virtual network.
- **10.0.0.0/16** - for the virtual network address space.
- **myAGSubnet** - for the subnet name.
- **10.0.0.0/24** - for the subnet address space.

12. Click **OK** to create the virtual network and subnet.
13. Click **Choose a public IP address**, choose **Create new**, and then enter the name of the public IP address.
    > Note: In this example, the public IP address is named myAGPublicIPAddress.
14. Accept the default values for the other settings and then click OK.
15. Accept the default values for the listener configuration, leave the web application firewall disabled, and then click **OK**.
16. Click **OK** to create the virtual network, the public IP address, and the application gateway.
17. Wait until the deployment finishes successfully before moving on to the next section.
    > Note: It may take up to 30 minutes for the application gateway to be created.
18. Click **All resources** in the left-hand menu, and then click **myVNet** from the resources list.
19. Click **Subnets** and then click **+ Subnet**.
20. Enter **myBackendSubnet** for the name of the subnet and then click **OK**.
21. Switch to **PowerShell**.
22. Run the following command to change directory to the Setup folder:
    ```bash
    cd [Repository Root]\AllFiles\Mod10\DemoFiles\Setup3
    ```
23. Run the following command:
    ```batch
     .\addAzureServiceToGateway.ps1
    ```
24. Run the following command to add new reservation:
    ```batch
    Invoke-WebRequest -Uri http://13.80.47.113/api/reservation/sign -Method Post -ContentType 'application/json' -Body '{"ReservationId":215697,"Room":{"RoomId":689331,"Number":476,"Price":2588.0},"DateCreated":"2018-08-30T08:32:55.3930115Z","CheckIn":"2019-03-09T08:32:55.3909954Z","CheckOut":"2019-04-02T08:32:55.3909954Z","Guests":2,"Traveler":{"TravelerId":102583,"Name":"Donovan","Email":"donovan+877@hotmail.com"}}'
    ```
    > Note: To add new reservation, we invoke **POST** web request.
25. In the response, verified that the **StatusCode** is **200** and **StatusDescription** is **OK**.

### Demonstration: Using Traffic Manager with an Azure Web App in multiple regions

#### Demonstration Steps


1. Open **PowerShell** as **Administrator**.
2. In the **User Account Control** modal, click **Yes**.
3. Run the following command: **Install-Module azurerm -AllowClobber -MinimumVersion 5.4.1**.
4. Run the following command to change directory to **Setup** folder:
    ```bash
    cd [repository root]\AllFiles\Mod10\DemoFiles\Setup4
    ```
5. Run the following command to run the script taht create webapp in **West Europe** region:
    ```batch
     .\createAzureServicesWestEurope.ps1
    ```
6. In the **Sign in** window that appears, enter your details, and then sign in.
7. You will be asked to supply a **Subscription ID**, which you can get by performing the following steps:
    - Open a browser and navigate to **http://portal.azure.com**. If a page appears, asking for your email address, type your email address, and then click Continue. Wait for the sign-in page to appear, enter your email address and password, and then click Sign In.
    - In the search text box on the top bar, type **Cost** and then in results click **Cost Management + Billing(Preview)**. The **Cost Management + Billing** window should open.
    - Under **BILLING ACCOUNT**, click **Subscriptions**.
    - Under **My subscriptions**, you should have at least one subscription. Click on the subscription that you want to use.
    - Copy the value from **Subscription ID**, and then paste it at the **PowerShell** prompt. 
8.  In the **Administrator: Windows PowerShell** window, follow the on-screen instructions. Wait for the deployment to complete successfully.
9.  Write down the name of the Azure App Service that is created.
10. Run the following command to change directory to **Setup** folder:
    ```bash
    cd [repository root]\AllFiles\Mod10\DemoFiles\Setup4
    ```
11. Run the following command to run script taht create webapp in **West Us** region:
    ```batch
     .\createAzureServicesWestUS.ps1
    ```
12. Sign in with your **Subscription ID** and follow the on-screen instructions.
13. Redo **Task 1 steps 6-10**.
14. Close **PowerShell** window.
15. Open a browser and navigate to **http://portal.azure.com**
16. Click on **All resources** on the left pannel.
17. Click on **+Add**, and search **Traffic Manager profile**.
18. Click on **Create** button in the **Traffic Manager profile** blade.
19. In the **Traffic Manager profile**:
   - In the **Name**, type **blueyondermod10demoTM{YourInitials}**.
   - In **Routing method**, select the Priority routing method. 
   - In **Resource group**, select **Use existing** and then select **Mod10demo4Europe-RG**.
18. Click **Create**, and wait until it created successfully.

19. In **All resources**, Locate **blueyondermod10demoTM{YourInitials}** and click it.
20. Click on **Endpoints** in the **Settings** section.
21. Click on **+ Add** and enter following information to create primary end point:
   - In **Type**, choose **Azure endpoint**.
   - In **Name**, type **myPrimaryEndpoint**.
   - In **Target resource type**, choose **App Service**.
   - In **Target resource**, choose **blueyondermod10demo4Europe**.
   - In **Priority**, type **1**.
   - Click **Ok**, and wait until it created successfully.
22. Click on **+ Add** and enter following information to create secondary end point:
   - In **Type**, choose **Azure endpoint**.
   - In **Name**, type **mySecondaryEndpoint**.
   - In **Target resource type**, choose **App Service**.
   - In **Target resource**, choose **blueyondermod10demo4US**.
   - In **Priority**, type **2**.
   - Click **Ok**, and wait until it created successfully.

23. Open **Microsoft Edge** browser.
24. Navigate to:
    ```url
    http://blueyondermod10demoTM{YourInitials}.trafficmanager.net/api/destinations
    ```
    > Note: All requests are routed to the primary endpoint that is set to Priority 1.
25. Open **Command Line**.
26. Run the following command to find our trafficmanager:
    ```bash
    nslookup blueyondermod10demoTM{YourInitials}.trafficmanager.net
    ```
27. In the result, locate **Aliases:** property.
28. You should see it used **blueyondermod10demo4Europe** service.
     > Note: **blueyondermod10demo4Europe** is the primary endpoint.
29. Switch to **Azure Portal**.
30. In **All resources**, Locate **blueyondermod10demoTM{YourInitials}** and click it.
31. Click on **Endpoints**  in the **Settings** section.
32. Click **myPrimaryEndpoint**, change the **Status** to **Disabled**.
33. Click **Save**, and Wait until all changes saved.
34. Navigate to:
    ```url
    http://blueyondermod10demoTM{YourInitials}.trafficmanager.net/api/destinations
    ```
    > Note: All requests are routed to the secondary endpoint that is set to Priority 2.
35. Switch to **Command Line**.
36. Run the following command to find our trafficmanager:
    ```bash
    nslookup blueyondermod10demoTM{YourInitials}.trafficmanager.net
    ```
37. In the result, locate **Aliases:** property.
38. You should see it used **blueyondermod10demo4US** service.
     > Note: **blueyondermod10demo4US** is the secondary endpoint.
