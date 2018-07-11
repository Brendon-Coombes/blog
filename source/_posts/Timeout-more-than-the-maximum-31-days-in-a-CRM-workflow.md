---
title: Timeout more than the maximum 31 days in a CRM workflow
categories:
 - [Dynamics365, Tips]
date: 2016-10-19 15:32:26
tags:
 - Dynamics365
 - Workflows
---

When you are creating a workflow in CRM, you often have the need to make it wait for a certain time before you want to continue with the rest of the steps in the workflow.

You can do this by adding a Wait Condition. There are some limitations to the Wait Condition however. You can only wait  1 - 59 Minutes, 1 - 23 Hours, 1 - 31 Days, or 1 - 24 Months. You can get what you need most of the time from these values, but what happens when you can't? What if you need to wait 40 Days exactly? Or 6 Weeks exactly, or another value that cannot be made from the given options?
<!-- more --> 

{% asset_img "crm-dynamic-values-available.png" "CRM dynamic values available"%}

You can edit the HTML on the page directly to get around this. I will show you how to do this simply using the Developer Tools in your browser. You normally get this by pressing F12. I will be showing you how to do this on Google Chrome, but any modern browser will be able to do the same thing.

Edit your Workflow Wait Condition, and get it up to the step where you are ready to select the wait time period.

{% asset_img "beforewaittime.png" "Edit wait condition"%}

When you are at this stage, open your browsers Developer Tools. (Normally pressing F12 will open this). And use the "Select Element" tool. 

{% asset_img "selectelementtool.png" "Developer Tools: Select Element Tool"%}

Click the drop down box that you want a different value in. When you have clicked it; the developer tools will highlight the HTML for the drop down box in the main panel. The HTML tag for a drop down box is a "select" tag. When you expand this tag using the triangle to the left of it, you will see all of the available options for that drop down.

{% asset_img "selecttaghtmlexpanded.png" "Select Tag - expanded"%}

For this example I am going to use Days and add the value 40 to the options.

Select the last option in the list. In this case that is "31 Days". Right-click on it and click the option to "Edit as HTML". You will then get a text box containing the option you selected that you can edit. We are going to enter the new value into this text box by copying the existing option and editing it to what we need.

{% asset_img "newoption.png" "Add new option"%}

You can now close the developer tools, and the option you just added will be available at the bottom of the drop down list for you to choose.

{% asset_img "newvalueindropdown.png" "New value is visible in drop-down"%}

You now can use this value in your timeout and continue with your workflow development as normal. There is nothing more that you need to do in order to be able to save the value. It just works from this point as if the value has always existed in the drop down to begin with.

{% asset_img "workflowstep.png" "Workflow saved step"%}

I have tested this on Dynamics CRM 2016 Online. I am fairly certain it will work with most versions of Dynamics CRM though. I am assuming that this functionality is using the AddDays method on the current DateTime and there is no validation other than it being a valid integer. I have yet to play around with it and see what else you are able to do.