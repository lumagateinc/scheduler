
![Documentation Home](https://github.com/lumagateinc/scheduler/blob/master/images/header_img.png)

# Resource Scheduler<!-- omit in toc -->

The Resource Scheduler for Microsoft Azure provides a quick and easy way to create group schedules to stop and start Azure VMs on the schedule (days and times) you specify.

> **NOTE**: While additional resource types may be added in the future, "resources" in the current release refers to Azure VMs.

## Table of Contents<!-- omit in toc -->

- [Scope of a Resource Scheduler instance](#scope-of-a-resource-scheduler-instance)</br>
- [Install and Configure](#install-and-configure)</br>
  - [Installation](#installation)</br>
  - [Grant Permissions](#grant-permissions)</br>
  - [Connect Subscriptions](#connect-subscriptions)</br>
  - [Configure Time Zone](#configure-time-zone)</br>
- [Managing Schedules](#managing-schedules)</br>
  - [Schedule Resources Directly](#schedule-resources-directly)</br>
  - [Schedule Resources by Tag](#schedule-resources-by-tag)</br>
- [Troubleshooting and Support](#troubleshooting-and-support)</br>
  - [Viewing Resource Logs](#viewing-resource-logs)</br>
  - [Request Support](#request-support)</br>
- [Resource Scheduler Licensing](#resource-scheduler-licensing)</br>
- [Frequently Asked Questions](#frequently-asked-questions)</br>

## Scope of a Resource Scheduler instance<!-- omit in toc -->

A Resource Scheduler instance is associated to a single Azure Active Directory (AD) tenant. A Resource Scheduler instance can manage schedules for starting and stopping VMs in ALL Azure subscriptions associated with an Azure AD tenant. In other words:

- Resource Scheduler has a 1-1 relationship with your Azure AD tenant.
- Resource Scheduler has a 1-many relationship with your Azure subscriptions associated to that Azure AD tenant.

[back to ToC](#table-of-contents)

## Install and Configure<!-- omit in toc -->

This section covers the initial installation and configuration of the Resource Scheduler.

### Installation<!-- omit in toc -->

1. Browse to the Azure portal at https://portal.azure.com. Login using an account with Global Administrator rights.
2. In the search box at the top of the browser window, type "Marketplace". Select the Marketplace icon to go to the Azure Marketplace.
3. In the Marketplace search box, type "Resource Scheduler".
4. Click the "Resource Scheduler" tile in the search results.
5. To install the Resource Scheduler, click the **Create** button.

![marketplace](https://github.com/lumagateinc/scheduler/blob/master/images/marketplace.png)

**FIGURE 1**. Resource Scheduler in Azure Marketplace

**Video demo:**
Step-by-step demo of this task [HERE](http://example.com/link "title").

[back to ToC](#table-of-contents)

### Grant Permissions<!-- omit in toc -->

The Resource Scheduler includes custom roles based on Azure role-based access control. Roles include:

- **Administrator**. Enables a user to manage schedules for resources to which they have access, as well as to schedule. Additionally, this role can add additional subscriptions to the Resource Scheduler instance.
- **Schedule Manager**. Enables a user to manage schedules for resources to which they have access, as well as to schedule.
- **Auditor**. This role has read-only access to resources, schedules, logs, and subscriptions connected to the Resource Scheduler instance.

> **IMPORTANT NOTE:** Since the **Scheduler Manager** role can schedule based on Tags, a member of this role may be able to schedule resources they cannot see. This issue also exists when using tags with script or runbook-based scheduling, because Azure does not support RBAC for tags.

*To assign a Resource Scheduler role to a user or group, perform the following steps:*

1. In the Azure portal or Office 365 Admin Center, select **Azure Active Directory**.
2. Then, select Enterprise Applications. From the list, find and select Resource Scheduler (shown in Figure 2 below).
3. Click **Add user > Users and Groups**. Then, select the user or group you wish to add the role. Click **Select** to save your changes.
4. Next, click **Select Role**, and choose the role you would like to assign to the selected user or group (Administrator, Auditor, or Schedule Manager). Click **Select** to save your changes.

![entapps](https://github.com/lumagateinc/scheduler/blob/master/images/ent_apps.png)

**FIGURE 2**. Enterprise Apps list in Azure Active Directory

Menu appearance will vary by role assignment. Member of the **Administrator** role will see the Subscriptions and Settings menus, as shown in Figure 3.

![menus](https://github.com/lumagateinc/scheduler/blob/master/images/menus.png)

**FIGURE 3**. Enterprise Apps list in Azure Active Directory

**Video demo:**
Step-by-step demo of this task [HERE](http://example.com/link "title").

[back to ToC](#table-of-contents)

### Connect Subscriptions<!-- omit in toc -->

You can associate multiple subscriptions to a single Resource Scheduler instance. The only requirement is that the subscriptions are associated to the same Azure AD tenant as the Resource Scheduler instance.

*To connect a new subscription to the Resource Scheduler instance, perform the following steps:*

1. From the left menu, select **Subscriptions**.
2. From the **Available Subscriptions**, find the subscription you want to add.
3. Click the blue add ![add](https://github.com/lumagateinc/scheduler/blob/master/images/add.png) symbol next to the right of the subscription. Click **Connect** to confirm the change.

*To **disconnect** a subscription to the Resource Scheduler instance, perform the following steps:*

1. From the left menu, select **Subscriptions**.
2. From the **Connected Subscriptions**, find the subscription you want to remove.
3. Click the red disconnect ![delete](https://github.com/lumagateinc/scheduler/blob/master/images/disconnect.png) symbol next to the right of the subscription. Click **Disconnect** to confirm the change.

**Video demo:**
Step-by-step demo of this task [HERE](http://example.com/link "title").

[back to ToC](#table-of-contents)

### Configure Time Zone<!-- omit in toc -->

The time zone settings determines the time zone by which schedules will be set and displayed in the Resource Scheduler portal. Only a user in the Resource Scheduler Administrator role can change this setting.

*To connect a new subscription to the Resource Scheduler instance, perform the following steps:*

1. From the left menu, select **Settings**.
2. Under **Timezone**, select the desired time zone.
3. Click **Save** to save your changes.

**IMPORTANT!** If you change the Timezone setting after configuring schedules, it will change the time by which all schedules are evaluated! This should be clear from the time zone notices throughout the Resource Scheduler portal, but we wanted to mention it again here! :grin:

**Video demo:**
Step-by-step demo of this task [HERE](http://example.com/link "title").

[back to ToC](#table-of-contents)

## Managing Schedules<!-- omit in toc -->

This section describes how to schedule resources for start and stop at the days and times you specify. There are two approaches for scheduling resources:

1. **Scheduling resources directly**. With directly scheduling, you associate VMs to a schedule one at-a-time, using a simple search interface. *This is great for smaller environments or schedules that affect a small number of VMs.* See ["Schedule Resources Directly"](#schedule-resources-directly) for configuration steps.
2. **Scheduling by tag**. This option will automatically associate the schedule to all Azure VMs with the tag you specify. *This is the preferred option for bulk scheduling and large environments.* See ["Schedule Resources by Tag"](#schedule-resources-by-tag) for configuration steps.

> **A note on multiple schedules**. You can assign multiple schedules through direct assignment or using tags.

### Schedule Resources Directly<!-- omit in toc -->

Associating resources to schedules directly is the preferred method for managing small numbers of VMs, as explained in [Managing Schedules](#managing-schedules) above.

*To add VMs to a schedule, perform the following steps:*

1. From the left menu, select **Schedules**.
2. Click the plus (+) sign by **Schedules**, shown in Figure 4 below. This will bring up the schedule form.
3. Complete the values in the schedule form.
4. Add VMs in the Resources field, using the search and list controls, shown in Figure 5 below.
5. Click **Save** to save your changes.

![schedule](https://github.com/lumagateinc/scheduler/blob/master/images/schedules.png)

**FIGURE 4**. Schedule menu in Resource Scheduler

![schedule](https://github.com/lumagateinc/scheduler/blob/master/images/sched_res.png)

**FIGURE 5**. Adding VM resources directly to a schedule

**Video demo:**
Step-by-step demo of this task [HERE](http://example.com/link "title").

[back to ToC](#table-of-contents)

### Schedule Resources by Tag<!-- omit in toc -->

Associating schedules to resources with Azure tags is the preferred method for managing large numbers of resources as explained in [Managing Schedules](#managing-schedules) above. Resource Scheduler will recognize tags on the resource group containing the VM or the VM resource itself. If you are not familiar with tags in Azure, see ["Use tags to organize your Azure resources"](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources#portal). 

*To associate a tag to a schedule, perform the following steps:*

**First, you will create the schedule:**

1. From the left menu, select **Schedules**.
2. Click the plus (+) sign by **Schedules**. This will bring up the schedule form.
3. Complete the values in the schedule form. Leave the **Resources** field blank.
4. Click **Save** to save your changes.

**Next, you will associate the schedule to tags:**

1. From the left menu, select **Tags**.
2. In the **Available Tags** list, find the tag name associated to the resources you wish to schedule.
3. To the right of your tag, click the orange Schedule tag symbol, shown in Figure 6. This will bring up the schedule form.
4. In the **Schedule Trigger Values** field, add one or more tag values that will trigger schedule actions. *The tag values are provided to you in the dropdown list, shown in Figure 7 below.*
5. In the** Attached Schedules** field, select the desired schedule or schedules from the dropdown list. 
6. Click **Save** to save your changes.

![availtags](https://github.com/lumagateinc/scheduler/blob/master/images/avail_tags.png)

**FIGURE 6**. Adding VM resources directly to a schedule

![schedtags](https://github.com/lumagateinc/scheduler/blob/master/images/sched_tag.png)

**FIGURE 7**. Adding VM resources directly to a schedule


**Video demo:**
Step-by-step demo of this task [HERE](http://example.com/link "title").

[back to ToC](#table-of-contents)

## Troubleshooting and Support<!-- omit in toc -->

This section details where to view logs related to Resource Scheduler operation, as well as how to ask a question or request support.

### Viewing Resource Logs<!-- omit in toc -->

*To view logs related to a resource, perform the following steps:*

1. From the left menu, select **Schedules**.
2. Click the **Expand details** icon to the right of the resource (VM) in question.
3. Select the **Logs** tab.

Events are listed in descending order (newest event at the top).

**Video demo:**
Step-by-step demo of this task [HERE](http://example.com/link "title").

[back to ToC](#table-of-contents)

### Request Support<!-- omit in toc -->

*To request support, perform the following steps:*

**STEP 1: Check the FAQs**

Begin by checking our FAQs to see if your question is answered there. If it is not, proceed to STEP 2.

**STEP 2: Screenshot your Claims**

Visit **[https://\<tenant\>.azurewebsites.net/claims](https://\<tenant\>.azurewebsites.net/claims)** and capture a screenshot of the claims associated with your account. If you are logging a request for another user, ask them to capture this data and forward to you.

> **IMPORTANT!** Be sure to complete this step. Your support analyst may ask for this information.

**STEP 3: Log a ticket**

To log a ticket, visit https://lumagate.us/support and click the "CONTACT US" button. In the form provided, select "Resource Scheduler" in the **Product** dropdown. Complete the required fields in the form and click **Submit**. A support ticket will be logged and routed automatically. You will receive an e-mail confirmation that your request was received.

[back to ToC](#table-of-contents)


## Resource Scheduler Licensing<!-- omit in toc -->

Resource scheduler is licensed according to the number of resources (VMs) you need to schedule. For pricing information, see ["Resource Scheduler Licensing"](https://lumagate.us/azure/pricing) on the Lumagate website.

[back to ToC](#table-of-contents)

## Frequently Asked Questions<!-- omit in toc -->

Frequently asked questions are maintained on our [FAQs Page](https://github.com/lumagateinc/scheduler/blob/master/FAQs.md)

[back to ToC](#table-of-contents)