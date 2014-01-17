Smartsheet Bulk User Add Utility
===

License and Warranty
--------------------
Copyright 2014 Smartsheet.com

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.


Overview
--------

This command-line utility enables administrators of Team and Enterprise accounts to programmatically add users via the Smartsheet API.


Revision History
--------

* 1.0 - June 16 2013 - Initial build
* 1.1 - December 16 2014 - Improved logging and error handling


Dependencies
---

* Tested with Ruby 1.9.3 only.
* Ruby gems: httparty, activesupport, json. 
* Smartsheet Team or Enterprise plan.
* Smartsheet API access token that belongs to an account administrator.


Configuration
------

###User input list

Provide a CSV formatted list of users to be added to your Smartsheet account.  You can use the included user-list.csv sample file to get started.  The following columns
are expected, in this order:

* Email (required)
* FirstName (first name, empty otherwise)
* LastName (last name, empty otherwise)
* Admin ("yes" if admin, empty otherwise)
* Licensed ("yes" if licensed user, empty otherwise)
* ResourceMananger ("yes" if resource manager, empty otherwise)


###Config variables

The script contains several configuration variables that must be set:

* SS_TOKEN - Smartsheet API access token belonging to an account administrator.  See the [Smartsheet API docs](http://smartsheet.com/developers) for help on how to generate acess tokens.

* CSV_FILE - name of the user input list you have created.

* EMAIL_DOMAINS - zero or more email domains controlled by your organization.  See the detailed discussion of EMAIL_DOMAINS below.

By default, Smartsheet's has an opt-in account membership model - users cannot be added to a Smartsheet account without their explicit consent.  It means that when you attempt to add a user to your account, an email invitation is sent to the user's email address asking him/her to join.

Let's say that you own example.com, and you want to provision Smartsheet logins for 10k members of example.com seamlessly - without requiring them to take an action.  Contact our Smartsheet account representative to add a domain record to your account - be prepared to provide proof of domain ownership.  Once the domain record is in place, add the domain to the EMAIL_DOMAINS list, and the members of example.com can be seamlessly provisioned via this utility.

There are several exception to this rule:

* You are attempting to add the user as a licensed sheet creator, and you have already exhausted the number of licensed allotted for your account
* You are attempting to add the user as a regular member (rather than a licensed sheet creator), and the user already owns one or more sheets - in that case, the user must be added as a licensed sheet creator
* The user is already a member of your account
* The user does not match any of your registered email domains
* The user is an existing Smartsheet user and belongs to another trial account
* The user is an existing Smartsheet user and belongs to another paid account


Usage
---

###Rate limit
Rate limit: The Smartsheet API enforces a rate limit of 300 calls per minute per access token (we reserve the right to change this rate, so please check the Smartsheet API docs to confirm).  The script is designed to gracefully handle the rate limit and will
sleep for 60 seconds before retrying whenever a rate limit error is encountered.

###Errors
If a non-fatal error is encountered (e.g., the user already belongs to another Smartsheet account), the utility will log the error and continue.

The utility is currently configured to skip any users that don't match your domain records.  You can edit the code to override that, but keep in mind that these users will receive email invitations to join your account.  See the discussion of EMAIL_DOMAINS for more information.

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/25c830f239e48c7b8b5584b1f4afaab6 "githalytics.com")](http://githalytics.com/smartsheet-platform/bulk-user-add)
