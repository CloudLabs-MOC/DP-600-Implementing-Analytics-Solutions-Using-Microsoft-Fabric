# Module 07: Enforce model security

## Lab scenario

In this lab, you will update a pre-developed data model to enforce security. Specifically, salespeople at the Adventure Works company should only be able to see sales data related to their assigned sales region.

## Lab objectives

In this lab, you will perform:

- Create static and dynamic roles
- Validate roles
- Map security principals to dataset roles

## Estimated timing: 60 minutes

## Architecture Diagram

![](Images/lab9-archy.png)

## Exercise 0: Set up the prerequisites

### Task 1: Set up Power BI Desktop

In this task, you will set up Power BI Desktop.

1. To open File Explorer, on the taskbar, select the **File Explorer** shortcut.

	![](Images/dp9-1new.png)

1. Go to the **C:\LabFiles\DP-600-Implementing-Analytics-Solutions-Using-Microsoft-Fabric\Allfiles\LabFiles\09\Starter** folder.

1. To open a pre-developed Power BI Desktop file, double-click the **Sales Analysis - Enforce model security.pbix** file.
	
	![](Images/dp9-2.png)

1. At the top-right corner of Power BI Desktop, if you're not already signed in, select **Sign In**. Use the lab credentials in the Environment details tab to complete the sign-in process.

	![](Images/DP500-16-6new.png)
	
1. Enter the Lab username in the **Enter your email address (1)** and click on **Continue (2)** 
    * Email/Username: <inject key="AzureAdUserEmail"></inject>

      ![](Images/dp7-1.png)

      >**Note:** When prompted, on the **Let's get you signed in**, select **Work or school account**, and select **continue** on the pop-up.
	
1. Complete the sign up process by entering the **Email**, and select **Next**. 
   * Email/Username: <inject key="AzureAdUserEmail"></inject>

      ![](Images/dp-up1.png)

1. Enter the Password provided in the Environment Details tab and click on **Sign-in**

   * Password: <inject key="AzureAdUserPassword"></inject>

        ![](Images/dp-up(4).png)

    >**Note:** On the **Stay Signed in to all your apps**, select **No, sign in to this app only**.
1. At top-right corner, select the profile icon, select **Power BI service**.

   ![](Images/dp7-3.png)

1. Enter the Lab username:- * Email/Username: <inject key="AzureAdUserEmail"></inject>.

1. Do any remaining tasks to complete the trial setup.

   >**Note**: The Power BI web browser experience is known as the **Power BI service**.

### Task 2: Review the data model

In this task, you will review the data model.

1. Navigate back to Power BI Desktop.

1. In Power BI Desktop, at the left, switch to the **Model** view.

   ![](Images/dp9-6new.png)

1. Use the model diagram to review the model design.

   ![](Images/dp500_09-10.png)

   >**Note**: The model comprises six dimension tables and one fact table. The **Sales** fact table stores sales order details. It's a classic star schema design.

1. Expand the **Sales Territory** table.

   ![](Images/dp500_09-11.png)

1. Notice that the table includes a **Region** column.

   ![](Images/dp7-4.png)

   >**Note**: The **Region** column stores the Adventure Works sales regions. At this organization, salespeople are only allowed to see data related to their assigned sales region. In this lab, you will implement two different row-level security techniques to enforce data permissions.

## Exercise 1: Create static roles

In this exercise, you will create and validate static roles, and then see how you would map security principals to the dataset roles.

### Task 1: Create static roles

In this task, you will create two static roles.

1. Switch to **Report** view.

   ![](Images/dp9-7.png)

2. In the stacked column chart visual, in the legend, notice (for now) that it's possible to see many regions.

   ![](Images/dp9-8.png)

   >**Note**: For now, the chart looks overly busy. That's because all regions are visible. When the solution enforces row-level security, the report consumer will see only one region.

3. To add a security role, on the **Modeling** ribbon tab, from inside the **Security** group, select **Manage roles**.

   ![](Images/dp9-9.png)

4. In the **Manage roles** window, select **Create**.

   ![](Images/dp7-5.png)

5. To name the role, replace the selected text with **Australia**, and then press **Enter**.

   ![](Images/dp7-6.png)

6. In the **Tables** list, for the **Sales Territory** table, select the **ellipsis (1)**, and then select **Add filter (2)** > **[Region] (3)**.

   ![](Images/dp7-7.png)

7. In the **Table filter DAX expression** box, replace **Value** with **Australia (1)**.

   ![](Images/dp9-13.png)

   >**Note**: This expression filters the **Region** column by the value **Australia**.

8. To create another role, press **Create**.

   ![](Images/dp7-8.png)

9. Repeat the steps in this task to create a role named **Canada** that filters the **Region** column by **Canada**.

   ![](Images/dp9-14.png)

   >**Note**: In this lab, you'll create just the two roles. Consider, however, that in a real-world solution, a role must be created for each of the 11 Adventure Works regions.

10. Select **Save**.

    ![](Images/dp7-9.png)

### Task 2: Validate the static roles

In this task, you will validate one of the static roles.

1. On the **Modeling** ribbon tab, from inside the **Security** group, select **View as**.

   ![](Images/dp7-10.png)

2. In the **View as roles** window, select the **Australia (1)** role. Then Select **OK (2)**.

   ![](Images/dp7-11.png)

4. On the report page, notice that the stacked column chart visual shows only data for Australia.

   ![](Images/dp9-16.png)

5. Across the top of the report, notice the yellow banner that confirms the enforced role.

   ![](Images/dp500_09-26.png)

6. To stop viewing by using the role, at the right of the yellow banner, select **Stop viewing**.

   ![](Images/dp9-17.png)

### Task 3: Publish the report

In this task, you will publish the report.

1. Save the Power BI Desktop file.

   ![](Images/dp9-18.png)
 
2. To publish the report, on the **Home** ribbon tab, select **Publish**.

   ![](Images/dp9-19.png)

3. In the **Publish to Power BI** window, select your workspace that is **Fabric-<inject key="DeploymentID" enableCopy="false"/> (1)**, and then select **Select (2)**.

   ![](Images/dp7-12.1.png)
   
5. When the publishing succeeds, select **Got it**.

   ![](Images/dp9-20.png)

### Task 4: Configure row-level security (*Read-only*)

In this task, you will see how to configure row-level security in the Power BI service. 

This task relies on the existence of a **Salespeople_Australia** security group in the tenant you are working in. This security group does NOT automatically exist in the tenant. If you have permissions on your tenant, you can follow the steps below. If you are using a tenant provided to you in training, you will not have the appropriate permissions to create security groups. Please read through the tasks, but note that you will not be able to complete them in the absence of the existence of the security group. **After reading through, proceed to the Clean Up task.**

1. Switch to the Power BI service (web browser).

2. In the workspace landing page, notice the **Sales Analysis - Enforce model security** Semantic model.

   ![](Images/dp6003.png)

3. Hover the cursor over the Semantic model, and when the ellipsis appears, select the ellipsis, and then select **Security**.

   ![](Images/dp500_09-33.png)
	
   >**Note**: The **Security** option supports mapping Microsoft Azure Active Directory (Azure AD) security principals, which includes security groups and users.

4. At the left, notice the list of roles, and that **Australia** is selected.

   ![](Images/dp500_09-34.png)

5. In the **Members** box, commence entering **Salespeople_Australia**.

   ![](Images/dp500_09-35.png)

   >**Note**: Steps 5 through 8 are for demonstration purposes only, as they rely on the creation or existence of a Salespeople_Australia security group. If you have permissions and the knowledge to create security groups, please feel free to proceed. Otherwise, continue to the Clean Up task.

6. Select **Add**.

   ![](Images/dp500_09-36.png)

7. To complete the role mapping, select **Save**.

   ![](Images/dp500_09-37.png)

   >**Note**: Now all members of the **Salespeople_Australia** security group are mapped to the **Australia** role, which restricts data access to view only Australian sales.
 
   >**Note**: In a real-world solution, each role should be mapped to a security group.
 
   >**Note**: This design approach is simple and effective when security groups exist for each region. However, there are disadvantages: it requires more effort to create and set up. It also requires updating and republishing the dataset when new regions are onboarded.

   >**Note**: In the next exercise, you will create a dynamic role that is data-driven. This design approach can help address these disadvantages.

8. To return to the workspace landing page, in the **Navigation** pane, select the workspace.

## Exercise 2: Create a dynamic role

In this exercise, you will add a table to the model, create and validate a dynamic role, and then map a security principal to the dataset role.

### Task 1: Add the Salesperson table

In this task, you will add the **Salesperson** table to the model. Please switch to PowerBI Desktop.

1. Switch to **Model** view.

   ![](Images/dp7-13.png)

2. On the **Home** ribbon tab, from inside the **Queries** group, select the **Transform data** icon. And Then select **Transform data** from the drop-down.

   ![](Images/dp9-23.png)

   ![](Images/dp7-14.png)

   >**Note**: If you are prompted to specify how to connect, **Edit Credentials** and specify how to sign-in.

   ![](Images/dp9-24.png)

   >**Note**: Select **Connect**.

   ![](Images/dp9-25.png)
	 
   >**Note**: If you are prompted for Encryption Support, click on **OK**
	
   ![](Images/dp500_09-42.png)

4. In the **Power Query Editor** window, in the **Queries** pane (located at the left), right-click the **Customer (1)** query, and then select **Duplicate (2)**.

   ![](Images/dp7-15.png)

   >**Note**: Because the **Customer** query already includes steps to connect the data warehouse, duplicating it is an efficient way to commence the development of a new query.

5. In the **Query Settings** pane (located at the right), in the **Name** box, replace the text with **Salesperson**.

   ![](Images/dp7-16.png)

6. In the **Applied Steps** list, right-click the **Removed Other Columns (1)** step (third step), and then select **Delete Until End (2)**.

   ![](Images/dp7-17.png)

7. When prompted to confirm deletion of the step, select **Delete**.

   ![](Images/dp9-29.png)

8. To source data from a different data warehouse table, in the **Applied Steps** list, in the **Navigation** step (second step), select the gear icon (located at the right).

   ![](Images/dp7-18.png)

   >**Note:** If you are prompted to specify how to connect, Click on **Connect** followed by **OK** on the Encryption Support Pop-up.

10. In the **Navigation** window, select the **DimEmployee (1)** table.Then Select **OK (2)**.

    ![](Images/dp7-19.png)

12. To remove unnecessary columns, on the **Home (1)** ribbon tab, from inside the **Manage Columns** group, select the **Choose Columns (2)** icon and slect **Choose Columns (3)** from the drop-down.

     ![](Images/dp7-20.png)

13. In the **Choose Columns** window, uncheck the **(Select All Columns)** item.

     ![](Images/dp7-21.png)

14. Check the following three columns, and select **OK**:

	- EmployeeKey

	- SalesTerritoryKey

	- EmailAddress

	   ![](Images/dp9-33.png)

15. To rename the **EmailAddress** column, double-click the **EmailAddress** column header.

    ![](Images/dp7-22.png)

17. Replace the text with **UPN**, and then press **Enter**.

    >**Note**: UPN is an acronym for User Principal Name. The values in this column match the Azure AD account names.

    ![](Images/dp7-23.png)

18. To load the table to the model, on the **Home** ribbon tab, select the **Close &amp; Apply** icon.

    ![](Images/dp7-24.png)

19. When the table has added to the model, notice that a relationship to the **Sales Territory** table was automatically created.

    ![](Images/dp7-25.png)

### Task 2: Configure the relationship

In this task, you will configure properties of the new relationship.

1. Right-click the relationship between the **Salesperson** and **Sales Territory** tables, and then select **Properties**.

   ![](Images/dp7-26.png)

2. In the **Edit relationship** window, in the **Cross filter direction** dropdown list, select **Both (1)**. Check the **Apply security filter in both directions (2)** checkbox.

   ![](Images/dp9-37.png)

   >**Note**: Because there' a one-to-many relationship from the **Sales Territory** table to the **Salesperson** table, filters propagate only from the **Sales Territory** table to the **Salesperson** table. To force propagation in the other direction, the cross filter direction must be set to both.
	
   >**Note**: In case you encounter this error: `Table 'Sales Territory' is configured for row-level security, introducing constraints on how security filters are specified.` Uncheck the **Apply security filter in both directions** box.

   ![](Images/dp500-m09-note10a.png)
	
   ![](Images/dp500-m09-note11.png)

4. Select **OK**.

    ![](Images/dp7-27.png)

5. To hide the table, at the top-right of the **Salesperson** table, select the eye icon.

   ![](Images/dp7-28.png)

   >**Note**: The purpose of the **Salesperson** table is to enforce data permissions. When hidden, report authors and the Q&A experience won't see the table or its fields.
 
### Task 3: Create a dynamic role

In this task, you will create a dynamic role, which enforces permissions based on data in the model.

1. Switch to **Report (1)** view.

2. To add a security role, on the **Modeling** ribbon tab, from inside the **Security** group, select **Manage roles (2)**.

   ![](Images/dp9-39.png)

3. In the **Manage roles** window, select **Create**.

   ![](Images/create.png)

4. To name the role, replace the selected text with **Salespeople**.

   ![](Images/salespeople.png)

   >**Note**: This time, only one role needs to be created.

5. In order to add a filter to the **UPN** column of the **Salesperson** table, select the ellipsis (1), and then select Add filter (2) > [UPN] (3).

   ![](Images/dp7-29.png)

6. In the **Table filter DAX expression** box, replace **"Value"** with **USERPRINCIPALNAME() (1)**, and select **Save (2)**.

   ![](Images/dp7-30.png)

   >**Note**: This expression filters the **UPN** column by the USERPRINCIPALNAME function, which returns the user principal name (UPN) of the authenticated user.

7. Navigate back to **Managed Roles** under **Modelling** Tab. Now, being under the **Salespeople** role, add a filter to the **Region** column of the **Sales Territory** table.

   ![](Images/salespeople1.png)

8. In the **Table filter DAX expression** box, replace **"Value"** with **Northeast**.

   ![](Images/dp500_09-66.png)

   >**Note**: When the UPN filters the **Salesperson** table, it filters the **Sales Territory** table, which in turn filters the **Sales** table. This way, the authenticated user will only see sales data for their assigned region.

7. Select **Save**.

   ![](Images/dp7-31.png)

### Task 4: Validate the dynamic role

In this task, you will validate the dynamic role.

1. On the **Modeling** ribbon tab, from inside the **Security** group, select **View as**.

   ![](Images/dp9-44.png)

1. In the **View as roles** window, check **Other user (1)**, and then in the corresponding box, enter: **michael9@adventure-works.com (2)**. Check the **Salespeople (3)** role, and select **OK (4)**.

   ![](Images/dp7-32.png)
	
   >**Note**: For testing purposes, **Other user** is the value that will be returned by the USERPRINCIPALNAME function. Note that this salesperson is assigned to the **Northeast** region.

1. On the report page, notice that the stacked column chart visual shows only data for Northeast.

   ![](Images/dp9-47.png)

1. Across the top of the report, notice the yellow banner that confirms the enforced role.

   ![](Images/dp500_09-73.png)

1. To stop viewing by using the role, at the right of the yellow banner, select **Stop viewing**.

   ![](Images/dp500_09-74.png)

### Task 5: Finalize the design (*Read-only*)

In this task, you will finalize the design by publishing the report and mapping a security group to the role.

*The steps in this task are deliberately brief. For full step details, refer to the task steps of the previous exercise.*

1. Save the Power BI Desktop file.

    ![](Images/dp500_09-75.png)

2. Publish the report to the workspace you created at the beginning of the lab.

3. When the publishing succeeds, select Got it.

4. Close Power BI Desktop.

5. Switch to the Power BI service (web browser).

6. Go to the security settings for the **Sales Analysis - Enforce model security** dataset.

7. Map the **Salespeople** security group the **Salespeople** role.

   ![](Images/dp500_09-76.png)

   >**Note**: Now all members of the **Salespeople** security group are mapped to the **Salespeople** role. Providing the authenticated user is represented by a row in the **Salesperson** table, the assigned sales territory will be used to filter the sales table.

   >**Note**: This design approach is simple and effective when the data model stores the user principal name values. When salespeople are added or removed, or are assigned to different sales territories, this design approach will simply work.

### Review
In this lab, you have completed:
- Create static and dynamic roles
- Validate roles
- Map security principals to dataset roles
  
## You have successfully completed this lab, please proceed with the upcoming modules.
