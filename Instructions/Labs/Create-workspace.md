## Create a Fabric workspace

#### Task 1.1: Assign Fabric Administrator Role

1. In the Azure portal, type **Microsoft Entra ID (1)** in the search box and select **Microsoft Entra ID (2)** from the results.

   ![Navigate-To-AAD](./Images/ws/entra01.png)

2. Navigate to **Roles and administrators (1)**.

   ![Roles-and-Administrator](./Images/ws/entraa02.png)

3. In the **Roles and administrators** page, type **Fabric Administrators (1)** in the search box and select **Fabric Administrators (2)** from the results.

   ![search-fabric-admin](./Images/ws/entra02.png)

4. This will take you to the **Fabric Administrator | Assignments** page where you will have to assign yourself the **Fabric Administrator role**. Now, click on **+ Add assignments (1)**.

   ![click-add-assignments](./Images/ws/04.png)

5. Make sure to **check the box (1)** next to your username, confirm if it is **Selected (2)** and click on **Add (3)**.

   ![check-and-add-role](./Images/ws/05.png)

6. You can confirm the **Fabric Administrator** role has been added successfully by **Refresh (1)** Fabric Administrators | Assignments page. After **confirming (2)** it has been added successfully, navigate back to **Home (3)**.

   ![check-and-navigate-back-to-home](./Images/ws/06.png)

----

#### Task 1.2: Sign up for Microsoft Fabric Trial

1. Copy the **Power BI homepage link**, and open this link inside the VM in a new tab.

   ```
   https://powerbi.com
   ```

   >**Note**: In case a sign-up page asks for a phone number, you can enter a dummy phone number to proceed.

2. Select **Account manager (1)**, and click on **Start trial (2)**.

   ![Account-manager-start](./Images/ws/07.png)

3. A new prompt will appear asking you to **Upgrade to a free Microsoft Fabric trial**, click on **Start trial (1)**.

   ![Start-trial](./Images/ws/08.png)

4. Once your trial capacity is ready, you receive a confirmation message. Select **Got it (1)** to begin working in Fabric.

   ![Got-it](./Images/ws/09.png)

6. Now, open **Account manager (1)** again, and verify **Trial Status (2)**.

   ![Verify-trial-status](./Images/ws/10.png)

----

#### Task 1.3: Create a workspace

Here, you create a Fabric workspace. The workspace contains all the items needed for this lakehouse tutorial, which includes lakehouse, dataflows, Data Factory pipelines, notebooks, Power BI datasets, and reports.

1.  Now, select **Workspaces** and click on **+ New workspace**.

    ![New Workspace](./Images/ws/11.png)

2. Fill out the **Create a workspace** form with the following details:

   - **Name:** Enter **Fabric-<inject key="DeploymentID" enableCopy="false"/>**

      ![name-and-desc-of-workspc](./Images/ws/12.png)

   - **Advanced:** Expand it and Under **License mode**, select **Trial capacity (1)** and click on **Apply (2)** to create and open the workspace.

      ![advanced-and-apply](./Images/ws/13.png)

Congratulations! You have successfully learned to create a Fabric workspace.

----

*Continue to page 3 ...*
