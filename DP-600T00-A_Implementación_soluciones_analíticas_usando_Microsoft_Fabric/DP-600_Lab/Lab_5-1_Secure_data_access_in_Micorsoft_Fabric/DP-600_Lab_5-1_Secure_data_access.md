# Secure data access in Micorsoft Fabric

Microsoft Fabric has a multi-layer security model for managing data access. Security can be set for an entire workspace, for individual items, or through granular permissions in each Fabric engine. In this exercise, you secure data using workspace and item access controls and OneLake security roles.

Note: To complete the exercises in this lab, you’ll need two users: one user should be assigned the Workspace Admin role, and the other should have the Workspace Viewer role. To assign roles to workspaces see Give access to your workspace. If you don’t have access to a second account in the same organization, you can still do the exercise as a Workspace Admin and skip the steps done as a Workspace Viewer account, referring to the exercise’s screenshots to see what a Workspace Viewer account has access to.

This lab takes approximately 45 minutes to complete.

Tip: For related training content, see Secure data access in Microsoft Fabric.

## Create a workspace

Note: You need access to a Fabric paid or trial capacity to complete this exercise. For information about the free Fabric trial, see Fabric trial.

Navigate to the Microsoft Fabric home page at https://app.fabric.microsoft.com/home?experience=fabric in a browser and sign in with your Fabric credentials.
In the menu bar on the left, select Workspaces (the icon looks similar to 🗇).
Create a new workspace with a name of your choice, selecting a licensing mode that includes Fabric capacity (Trial, Premium, or Fabric).
When your new workspace opens, it should be empty.

![image](images)
Screenshot of an empty workspace in Fabric.

Note: When you create a workspace, you automatically become a member of the Workspace Admin role.

## Create a data warehouse

Next, create a data warehouse in the workspace you created:

Click + New Item. On the New item page, under the Store Data section, select Sample warehouse and create a new data warehouse with a name of your choice.

After a minute or so, a new warehouse will be created:

![image](images)
Screenshot of a new warehouse.

## Create a lakehouse

Next, create a lakehouse in the workspace you created.

In the menu bar on the left, select Workspaces (the icon looks similar to 🗇).
Select the workspace you created.
In the workspace, select the + New Item button and then select Lakehouse. Create a new Lakehouse with the name of your choice. Leave the Lakehouse schemas box selected.

After a minute or so, a new Lakehouse will be created:

![image](images)
Screenshot of a new lakehouse in Fabric.

Select the Start with sample data tile. After a minute or so, the lakehouse will be populated with sample public holidays data.


## Apply workspace access controls

Workspace roles are used to control access to workspaces and the content within them. Workspace roles can be assigned when users need to see all items in a workspace, when they need to manage workspace access, or create new Fabric items, or when they need specific permissions to view, modify or share content in the workspace.

In this exercise, you add a user to a workspace role, apply permissions, and see what is viewable when each set of permissions is applied. You open two browsers and sign-in as different users. In one browser, you’ll be a Workspace Admin and in the other, you’ll sign-in as a second, less privileged user. In one browser, the Workspace Admin changes permissions for the second user and in the second browser, you’re able to see the effects of changing permissions.

In the menu bar on the left, select Workspaces (the icon looks similar to 🗇).
Next select the workspace you created.
Select Manage access on the top of the screen.
Note: You’ll see the user you’re logged in as, who is a member of the Workspace Admin role because you created the workspace. No other users are assigned access to the workspace yet.

Next, you’ll see what a user without permissions on the workspace can view. In your browser, open an InPrivate window. In the Microsoft Edge browser, select the ellipse at the top right corner and select New InPrivate Window.
Enter https://app.fabric.microsoft.com and sign-in as the second user you’re using for testing.
On the bottom left corner of your screen, select Microsoft Fabric and then select Data Warehouse. Next select Workspaces (the icon looks similar to 🗇).
Note: The second user doesn’t have access to the workspace, so it’s not viewable.

Next, you assign the Workspace Viewer role to the second user and see that the role grants read access to the warehouse in the workspace.
Return to the browser window where you’re logged in as the Workspace Admin. Ensure you’re still on the page that shows the workspace you created. It should have your new workspace items, and the sample warehouse and lakehouse, listed at the bottom of the page.
Select Manage access at the top right of the screen.
Select Add people or groups. Enter the email of the second user you’re testing with. Select Add to assign the user to the workspace Viewer role.

Return to the InPrivate browser window where you’re logged in as the second user and select refresh button on the browser to refresh session permissions assigned to the second user.
Select the Workspaces icon on the left menu bar (the icon looks similar to 🗇) and select the workspace name you created as the Workspace Admin user. The second user can now see all of the items in the workspace because they were assigned the Workspace Viewer role.

![image](images)
Screenshot of workspace items in Fabric.

Select the warehouse and open it.
Select the Date table and wait for the rows to be loaded. You can see the rows because as a member of the Workspace Viewer role, you have CONNECT and ReadData permission on tables in the warehouse. For more information on permissions granted to the Workspace Viewer role, see Workspace roles.

Next, select the Workspaces icon on the left menu bar, then select the lakehouse.
When the lakehouse opens, click on the dropdown box at the top right corner of the screen that says Lakehouse and select SQL analytics endpoint.

Select the publicholidays table and wait for the data to be displayed. Data in the lakehouse table is readable from the SQL analytics endpoint because the user is a member of the Workspace Viewer role that grants read permissions on the SQL analytics endpoint.

## Apply item access control

Item permissions control access to individual Fabric items within a workspace, like warehouses, lakehouses and semantic models. In this exercise, you remove the Workspace Viewer permissions applied in the previous exercise and then apply item level permissions on the warehouse so a less privileged user can only view the warehouse data, not the lakehouse data.

Return to the browser window where you’re logged in as the Workspace Admin. Select Workspaces from the left navigation pane.
Select the workspace that you created to open it.
Select Manage access from the top of the screen.
Select the word Viewer under the name of the second user. On the menu that appears, select Remove.

![041_16-evaluate-optimized-measure](images/041_16-evaluate-optimized-measure.png)
Screenshot of workspace access dropdown in Fabric.

Close the Manage access section.
In the workspace, hover over the name of your warehouse and an ellipse (…) will appear. Select the ellipse and select Manage permissions.
Select Add user and enter the name of the second user.
In the box that appears, under Additional permissions, select ReadData and clear all other checkboxes.

![061_19-grant-people-access](images/061_19-grant-people-access.png)
Screenshot of warehouse permissions being granted in Fabric.

Select Grant.
Return to the browser window where you’re logged in as the second user. Navigate to the workspace icon again and refresh the browser view.
The second user no longer has access to everything in the workspace and instead has access to only the warehouse. You can no longer browse workspaces on the left navigation pane to find the warehouse. Select OneLake catalog on the left navigation menu to find the warehouse:

On the screen that appears, next to All items, select Type: Data items, then select All types.

![image](images)
Screenshot of OneLake catalog.

Select the warehouse, then select Open.
Select the Date table to view table data. The rows are viewable because the user has ReadData permission on the warehouse, which was granted through item-level permissions.
Apply OneLake security in a lakehouse
OneLake security lets you create custom roles within a lakehouse and grant granular access to specific tables and folders. You can also add row or column constraints to roles to further limit data access.

In this exercise, you grant an item permission and create an OneLake security role to control access to data in a lakehouse.

Stay in the browser where you’re logged in as the second user.
Select OneLake catalog on the left navigation bar. The second user doesn’t see the lakehouse.
Return to the browser where you’re logged in as the Workspace Admin.
Select Workspaces on the left menu and select your workspace. Hover over the name of the lakehouse.
Select the ellipse (…) and select Manage permissions.

![image](images)
Screenshot of setting permissions on a lakehouse in Fabric.

On the screen that appears, select Add user.
Assign the second user to the lakehouse and ensure none of the permission checkboxes on the Grant People Access window are checked.

![image](images)
Screenshot of the Grant People Access window for a lakehouse with no permissions selected.

Select Grant. The second user now has Read permission on the lakehouse, which allows access to metadata but not the underlying data. Next, you’ll validate this behavior.
Return to the browser where you’re logged in as the second user. Refresh the browser.
Select OneLake catalog in the left navigation pane.
Select the ellipsis (…) next to the lakehouse name and select Open.
Try to expand Tables to see the publicholidays table. You’ll see the lakehouse but won’t be able to view data in publicholidays table.

![image](images)
Screenshot of lakehouse unable to load data.

Next, you’ll grant the second user access to the publicholidays table using OneLake security roles.
Return to the browser where you’re logged in as the workspace administrator.
Select Workspaces from the left navigation bar.
Select your workspace name.
Select the lakehouse to open it.
When the lakehouse opens, select Manage OneLake security.

![image](images)
Screenshot of a Fabric lakehouse Security tab showing the Manage OneLake security option.

On the OneLake security screen that appears, select + New.

Create a new role named publicholidays. Under Select Grant permissions, leave the box unchecked. Select Next.
On the New role Data screen that appears, under Add data to your role, select Selected data.
Under Data preview, select Edit. In the data browser, expand Tables, and dbo, then select the checkbox next to publicholidays.

![image](images)
Screenshot of OneLake security role data browser with Tables expanded and publicholidays table selected

Select Add data to confirm. Then select Next from the New role Data screen.

In the New role Member section, enter the email address of your second user, select the checkbox, then select Create.

![image](images)
Screenshot of OneLake security role data browser with Tables expanded and publicholidays table selected

Return to the browser where you’re logged in as the second user. Ensure you’re still on the page where the lakehouse is open. Refresh the browser.
Expand Tables in the lakehouse explorer and select the publicholidays table. Wait for the data to load. The user can now view data in this table because they’re a member of the publicholidays OneLake security role, which grants Read permission to this specific table only.


## Clean up resources

In this exercise, you secured data using workspace access controls, item access controls, and OneLake security roles.

In the left navigation bar, select the icon for your workspace to view all of the items it contains.
In the menu on the top toolbar, select Workspace settings.
In the General section, select Remove this workspace.

![imagen](images)

<br>

---

[Up](#secure-data-access-in-microsoft-fabric)
