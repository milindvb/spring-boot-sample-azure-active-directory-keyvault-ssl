Login to Virtual Machine

1.  Clone sample repo from Github:

git clone
<https://github.com/milindvb/spring-boot-sample-azure-active-directory-keyvault-ssl.git>

Note: This sample assumes that you have a Working Azure Subscription and
Tenant.

2.  Login to Azure portal and Select Azure Active directory

![](media\image1.png){width="6.229166666666667in"
height="3.0114293525809273in"}

3.  Copy the **Tenant ID**; you will use that value to configure
    your *application.properties* file later in this tutorial.

> ![](media\image2.png){width="5.53125in" height="3.605358705161855in"}

4.  From the portal menu, select **App registrations**, and then
    select **New Registration**

![](media\image3.png){width="6.5in" height="3.609027777777778in"}

5.  Specify your application, and then select **Register**.

![](media\image4.png){width="6.5in" height="6.098611111111111in"}

6.  When the page for your app registration appears, copy
    your **Application ID** and the **Tenant ID**; you will use these
    values to configure your *application.properties* file later in this
    tutorial.

![](media\image5.png){width="6.5in" height="2.698611111111111in"}

7.  Click **Certificates & secrets** in the left navigation pane. Then
    select **New client secret**.

> ![](media\image6.png){width="6.5in" height="4.06875in"}

8.  Add a **Description** and select duration in the **Expires** list.
    Click **Add**. The value for the key will be automatically filled
    in.

> ![](media\image7.png){width="6.5in" height="2.5548611111111112in"}

9.  Copy and save the value of the client secret to configure
    your *application.properties* file later in this tutorial. (You will
    not be able to retrieve this value later.)

> ![](media\image8.png){width="6.895833333333333in"
> height="3.5282206911636047in"}

10. Click **API permissions** Click **Microsoft Graph**, then select
    **Directory.AccessAsUser.All** and click **Update Permissions**

![](media\image9.png){width="7.5in" height="6.433333333333334in"}

11. Click **Grant admin consent for Azure Sample** and select **Yes**.

![](media\image10.png){width="7.5in" height="3.067361111111111in"}

Permissions will be updated as shown below.

![](media\image11.png){width="7.5in" height="2.08125in"}

12. From the main page for your app registration,
    select **Authentication**, and select **Add a platform**. Then
    select **Web applications**.

![](media\image12.png){width="7.5in" height="2.698611111111111in"}

13. Enter *https://\<VirtualMachineIP\>:8443/login/oauth2/code/azure* as
    a new **Redirect URI**, and then select **Configure**.

![](media\image13.png){width="7.5in" height="4.839583333333334in"}

14. Add a user account to your directory, and add that account to a
    group. From the **Overview** page of your Active Directory, select
    **Users**, and then select **New user**.

![](media\image14.png){width="7.5in" height="2.55625in"}

15. When the **User** panel is displayed, enter the **User
    name** and **Name**. Then select **Create**.

> ![](media\image15.png){width="6.6875in" height="8.181042213473315in"}

** **

16. From the **Overview** page of your Active Directory,
    select **Groups**, then **New group** that you will use for
    authorization in your application.

17. Select **No members selected**. (For the purposes of this tutorial,
    we\'ll create a group named *group1*.) Search for the user created
    in the previous step. Select **Select** to add the user to the
    group. Then select **Create** to create the new group.

![](media\image16.png){width="7.5in" height="4.58125in"}

18. Go back to the **Users** panel, select your test user, and
    select **Reset password**, and copy the username and password; you
    will use this when you log into your application later in this
    tutorial.

![](media\image17.png){width="7.5in" height="1.4958333333333333in"}

## **Create Keyvault and self signed Certificate**

1.  In Azure Portal, click in **Keyvaults** and then click
    **Generate/Import**

![](media\image18.png){width="7.5in" height="3.470138888888889in"}

2.  Add **Certificate Name**, **Subject** and click on **Create**

![](media\image19.png){width="7.5in" height="8.320833333333333in"}

3.  Enable Managed System Identity for VM

![](media\image20.png){width="7.5in" height="4.565972222222222in"}

4.  Open port 8443 in Network Security Group by selecting **Networking**
    and clicking on **Add inbound Port**

![](media\image21.png){width="7.5in" height="3.7444444444444445in"}

5.  Add Access Policy for Cert

![](media\image22.png){width="7.5in" height="5.083333333333333in"}

6.  In the **Add access policy** section under **Configure from template
    (optional)** choose **Secret Management** from the pull-down menu.

7.  Choose **Select Principal**, and in the search field enter the name
    of the VM you created earlier.  Select the VM in the result list and
    choose **Select**. Select **Add** and Select **Save**

![](media\image23.png){width="7.5in" height="4.563888888888889in"}

## 

## **Configure and compile your app**

1.  Navigate to the *src/main/resources* folder in your project, then
    open the *application.properties* file in a text editor.

![](media\image24.png){width="6.354166666666667in"
height="3.1041666666666665in"}

+----------------------------------+----------------------------------+
| **Parameter**                    | **Description**                  |
+==================================+==================================+
| azure.activedirectory.tenant-id  | Contains your Active             |
|                                  | Directory\'s **Directory         |
|                                  | ID** from earlier.               |
+----------------------------------+----------------------------------+
| azure.activedirectory.client-id  | Contains the **Application       |
|                                  | ID** from your app registration  |
|                                  |                                  |
|                                  | that you completed earlier.      |
+----------------------------------+----------------------------------+
| azu                              | Contains the **Value** from your |
| re.activedirectory.client-secret | app registration key             |
|                                  |                                  |
|                                  | that you completed earlier.      |
+----------------------------------+----------------------------------+
| azure.activedir                  | Contains a list of Active        |
| ectory.user-group.allowed-groups | Directory groups to use for      |
|                                  |                                  |
|                                  | authorization.                   |
+----------------------------------+----------------------------------+

2.  Save and close the *application.properties* file.

3.  Open a command prompt and change directory to the folder where your
    app\'s *pom.xml* file is located.

4.  Build your Spring Boot application with Maven and run it; for
    example:

> mvn clean package -DskipTests
>
> mvn spring-boot:run

5.  Open a browser and Navigate to <https://13.68.147.183:8443/group1>

6.  Login with the user and password created earlier

7.  You will be asked to change the password when you login the first
    time

8.  After login following page will be displayed

![](media\image25.png){width="5.958333333333333in" height="0.90625in"}

9.  The Page will show "Not Secure" because we are using a self-signed
    certificate for this example.

10. Click on "**Not Secure**" and select **Certificate**

11. You will see that the page is signed with the same cert we created
    in Keyvault

![](media\image26.png){width="5.764271653543307in"
height="7.119344925634295in"}
