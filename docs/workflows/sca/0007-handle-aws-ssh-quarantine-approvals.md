---
layout: page
title: Handle AWS SSH Quarantine Approvals
permalink: /workflows/sca/0007-handle-aws-ssh-quarantine-approvals
parent: Secure Cloud Analytics
grand_parent: Workflows
---

# Handle AWS SSH Quarantine Approvals
<div markdown="1">
Workflow #0007
{: .label }
</div>

This workflow is triggered when an Approval Task generated by [Quarantine AWS Instances from Alerts]({{ site.baseurl }}/workflows/sca/0006-quarantine-aws-from-alert) is approved, denied, or expires. If approved, SSH quarantine restrictions are removed from the AWS security group.

**Note:** This workflow is designed to respond to approval tasks generated by [this workflow]({{ site.baseurl }}/workflows/sca/0006-quarantine-aws-from-alert)!

[<i class="fab fa-github mr-1"></i> Workflow Folder]({{ site.github.repository_url }}/tree/Main/Workflows/0007-SCA-HandleAWSSSHQuarantineApprovals__definition_workflow_01KB7IIMZN9BP0qtnpiqeNnkg14mgEjiILo/){: .btn-cisco-sky-blue .mr-2 } [JSON]({{ site.github.repository_url }}/tree/Main/Workflows/0007-SCA-HandleAWSSSHQuarantineApprovals__definition_workflow_01KB7IIMZN9BP0qtnpiqeNnkg14mgEjiILo/definition_workflow_01KB7IIMZN9BP0qtnpiqeNnkg14mgEjiILo.json){: .btn-cisco-outline }

---

## Requirements
* An Amazon Web Services account with instances monitored by SCA
* (Optional) Webex Teams

---

## Workflow Steps
1. (Optional) Add global variables to local variables
1. Extract the AWS instance ID from the Approval Task
1. If a Teams room name was provided, translate it into a room ID
1. Make sure we got an instance ID (if not, post an error to webex)
1. Check the approval result. If the user selected to leave the instance quarantined or the task expired, do nothing. If they want to remove quarantine:
	* Get information about the instance from AWS and extract its security group
	* Restore normal SSH access
	* Send a Webex Teams notification

---

## Configuration
* Set your AWS region in the `AWS Region` local variable

### Using Webex?
* Make sure your bot is added to the room you want to post messages to
* Add your Webex Bot Token to the `Webex Bot Token` local variable (or, if you have a token in a global variable already, set the local variable to the global's value using the `Fetch Global Variables` group at the beginning of the workflow)
* Provide either a `Webex Teams Room Name` or `Webex Teams Room ID` in their respective local variable (only one of the two is required)

### Not Using Webex?
* Check the `Skip Activity Execution` box for all of the Webex activities

---

## Targets
Target Group: `Default TargetGroup`

| Target Name | Type | Details | Account Keys | Notes |
|:------------|:-----|:--------|:-------------|:------|
| Amazon Web Services | AWS Endpoint | _Region:_ `Your Region`<br /> | Your AWS Account Key | |
| Webex Teams | HTTP Endpoint | _Protocol:_ `HTTPS`<br />_Host:_ `webexapis.com`<br />_Path:_ None | None | |

---

## Account Keys

| Account Key Name | Type | Details | Notes |
|:-----------------|:-----|:--------|:------|
| Your AWS Account Key | AWS Credentials | _Access Key:_ AWS API Access Key<br />_Secret Key:_ AWS API Secret Key | |
