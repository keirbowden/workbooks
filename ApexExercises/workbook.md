Introduction to Force.com/Apex Workshop Exercises
===================================================================

##About the Workbook##

This workbook contains exercises for configuring business processes, creating Apex triggers and creating Visualforce pages. No prior experience with Salesforce is necessary, but it is assumed that readers are familiar with object-oriented programming languages and have received an overview of the Force.com platform.

##Audience##
These tutorials are intended for developers who are new to the Salesforce1 platform.

##Sign Up for Developer Edition##

This workbook is designed to be used with a Developer Edition organization, or DE org for short. DE orgs are multipurpose
environments with all of the features and permissions that allow you to develop, package, test, and install apps.

1. In your browser, go to https://developer.salesforce.com/signup.
2. Fill in the fields about you and your company.
3. In the Email Address field, make sure to use a public address you can easily check from a web browser.
4. Type a unique Username. Note that this field is also in the form of an email address, but it does not have to be the same
as your email address, and in fact, it’s usually better if they aren’t the same. Your username is your login and your identity
on developer.salesforce.com, so you’re often better served by choosing a username such as
firstname@lastname.com.
5. Read and then select the checkbox for the Master Subscription Agreement and then click Submit Registration.
6. In a moment you’ll receive an email with a login link. Click the link and change your password.

## Exercise Set 1: Automating Business Processes through Configuration ##

In this set of exercises, you will creat automatic validation and post-save processing of Salesforce records entirely through configuration.  Before you start, please create a free Force.com Developer Edition Environment, as indicated earlier in the “About the Workbook” section.

###Configuration Exercise 1: Create a Validation Rule###

The Sales Manager for a large enterprise has found that the marketing team are creating contacts that are missing both the email address and phone.

A validation rule is required that ensures at least one of these fields is populated, and blocks the save with an appropriate error message if this is not the case.

You can read more about validation rules at: `https://help.salesforce.com/HTViewHelpDoc?id=fields_about_field_validation.htm`

###Configuration Exercise 2: Improve the Validation Rule###

Salesforce supports two distinct types of contacts - public (which are associated with an account) and private (which are not associated with an account and are only visible to the record owner and the system administrator).  Users have complained that they cannot add private contacts without specifying one of the email and phone field.

Rework the validation rule from Configuration Exercise 1 to allow private contacts to be saved without either of these fields, but that still blocks the save of a public contact unless at least one is present. 

Hint: A contact is associated with an account if the AccountId field is populated.

###Configuration Exercise 3: Create an Automated Email Alert###

The Sales manager would like to be notified when a high value (with an amount over 1 million) opportunity is created in the system.

Create a workflow rule that sends an email alert when a high value opportunity is created.  Note: the email alert should only be sent once, when the record is first inserted.

Hint: You will need to create an additional user to represent the Sales Manager.

###Exercise Set 2: Apex###

###Apex Exercise 1: Trigger###

Users would like to see in the account list view how many contacts are associated with an account. 

Create an Apex trigger that reacts to actions on the contact sobject and updates the count of contacts on the account.

Note: Private contacts (not associated with an account) should be unaffected by this trigger.

Hint: A contact is associated with an account if the AccountId field is populated.

Hint: The number of contacts will change when a contact is added, deleted, updated to be associated with a different account or undeleted from the recycle bin.

###Apex Exercise 2: Unit Test###

In order to promote the trigger to production, at least 75% code coverage is required.

Write a unit test for the trigger that covers at least the following scenarios:

1. A contact associated with an account is created
2. A contact associated with an account is updated to be associated with a different account
3. A contact associated with an account is deleted
4. A private contact (not associated to an account) is created
5. A private contact (not associated with an account) is deleted
6. A private contact (not associated with an account) is updated to associate it with an account

###Apex Exercise 3: Bulk Triggers###

Triggers need to be able to process up to 200 records at a time without breaching governor limits. 

Rework the trigger created in exercise 1 to be able to process actions on multiple contact records.

Hint: The trigger will need to build a list of affected accounts and then retrieve the related contacts for all of these accounts through a single SOQL query.

###Summary###
In this workbook you have used a couple of the features of the Bootstrap responsive web design framework - panels and the responsive grid. Bootstrap provides much more than this, including a rich set of reusable components and more than a dozen JQuery plugins to bring those components to life. As you may have noticed, when using Bootstrap in a Visualforce page, very few of the Visualforce standard components are used, and none of those components that provide layout or styling.

