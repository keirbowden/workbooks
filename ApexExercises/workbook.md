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

Hint: You create validation rules via the object in question, as shown in the screenshot below:

![Validation Rule Menu Option](https://lh5.googleusercontent.com/-axIv59aJau8/VGtAS6-PaeI/AAAAAAAAA_Q/XmkuqWeSck0/w228-h305-no/Screen%2BShot%2B2014-11-18%2Bat%2B12.32.43.png)

You can read more about validation rules at: https://help.salesforce.com/HTViewHelpDoc?id=fields_about_field_validation.htm

###Configuration Exercise 2: Improve the Validation Rule###

Salesforce supports two distinct types of contacts - public (which are associated with an account) and private (which are not associated with an account and are only visible to the record owner and the system administrator).  Users have complained that they cannot add private contacts without specifying one of the email and phone field.

Rework the validation rule from Configuration Exercise 1 to allow private contacts to be saved without either of these fields, but that still blocks the save of a public contact unless at least one is present. 

Hint: A contact is associated with an account if the AccountId field is populated.

###Configuration Exercise 3: Create an Automated Email Alert###

The Sales manager would like to be notified when a high value (with an amount over 1 million) opportunity is created in the system.

Create a workflow rule that sends an email alert when a high value opportunity is created.  Note: the email alert should only be sent once, when the record is first inserted.

Hint: You will need to create an additional user to represent the Sales Manager.

Hint: Access the workflow functionality from the setup menu as follows:

![Workflow Menu Option] (https://lh6.googleusercontent.com/1KWSLC8lJfAZeWjTwh4V6EaJ86WZPjACf8VxhpGjlt8=w160-h207-p-no)

Hint: You will also need to create an email template for use by the workflow action, via the following setup menu option:

![Email Template Option](https://lh3.googleusercontent.com/vav77vUsJawQnrAzqjSCh5mZYKogKHnUciWEoBE1GJI=w194-h207-p-no)

###Exercise Set 2: Apex###

###Apex Exercise 1: Create an Apex Trigger###

Users would like to see in the account list view how many contacts are associated with an account. 

Create an Apex trigger that reacts to actions on the contact sobject and updates the count of contacts on the account.

Note: Private contacts (not associated with an account) should be unaffected by this trigger.

Hint: A contact is associated with an account if the AccountId field is populated.

Hint: The number of contacts will change when a contact is added, deleted, updated to be associated with a different account or undeleted from the recycle bin.

Hint: To author an Apex trigger, access the Triggers option from the submenu of the sobject in question:

![Apex Trigger Menu](https://lh4.googleusercontent.com/oQkq6ZRJImCZYg4WBN83996fowsn56nvXux5IKBv4FM=w150-h207-p-no)

Hint: Here's an example of a simple trigger to remind you of the syntax:

  ```
  trigger bg_account_bu on Account (before update) 
  {
	  for (Account acc : trigger.new)
	  {
  		double version=1;
	  	if (null!=acc.Version__c)
		  {
  			version=acc.Version__c+1;
	  	}
		  acc.Version__c=version;
	  }
  }
  ```

###Apex Exercise 2: Unit Test###

In order to promote the trigger to production, at least 75% code coverage is required.

Write a unit test for the trigger that covers at least the following scenarios:

1. A contact associated with an account is created
2. A contact associated with an account is updated to be associated with a different account
3. A contact associated with an account is deleted
4. A private contact (not associated to an account) is created
5. A private contact (not associated with an account) is deleted
6. A private contact (not associated with an account) is updated to associate it with an account

Hint: In order to test the database actions, you will need to insert/update/delete the records in question.

###Apex Exercise 3: Bulk Triggers###

Triggers need to be able to process up to 200 records at a time without breaching governor limits. 

Rework the trigger created in exercise 1 to be able to process actions on multiple contact records.

Hint: The trigger will need to build a list of affected accounts and then retrieve the related contacts for all of these accounts through a single SOQL query.

###Exercise Set 3: Visualforce###

###Apex Exercise 1: Standard Controller###

Create a single Visualforce page to display the details of a contact and selected (you choose!) fields from the parent account. You can layout this information using simple HTML elements such as tables.

Hint: To access fields from a parent object use the dot notation to follow the relationship (e.g. Contact.Account.Name)

Hint: Here's an exceptionally simply Visualforce page to demonstrate accessing fields through a standard controller:

```
  <apex:page StandardController=“Account”>  
    You are viewing <apex:outputField value=“{!Account.Name}” />
  </apex:page>
```

###Apex Exercise 2: Extension Controller###

Create a single Visualforce page to edit a contact and selected fields from the parent account.  As this needs to save changes to multiple records, an extension controller is required to manage the additional record.

There is an introduction to extension controllers at : http://www.salesforce.com/us/developer/docs/pages/Content/pages_controller_extension.htm

Hint: An extension controller should always delegate to the standard controller wherever possible, so the extension controller should only manage one of these records.

###Apex Exercise 3: Unit Test###

Create a unit test for the extension controller created in exercise 2.

Hint: Testing a Visualforce controller does not involve any interaction with the page, instead you would set up the internal state of the controller to reflect the user actions you are simulating and then execute methods to verify behaviour.

Hint: Here's an example unit test for a custom controller to give you a feel for the structure of a test class
  ```
@isTest
private class WrapperExampleController_Test {

    private static testMethod void testController()
    {
    	// create a test account
    	Account acc1=new Account(Name='Unit Test 1', 
    	                         BillingStreet='Unit Test Street',
    	                         BillingCity='Unit Testville',
    	                         BillingPostalCode='UTEST1 1UT');
    	insert acc1;
    	
    	// instantiate the controller
    	WrapperExampleController ctrl=new WrapperExampleController();
    	
    	// check that there is at least one account
    	Integer accSize=ctrl.getAccounts().size();
    	System.assert(accSize>=1);
    	
    	// the number of rows should be 4 - one for each field
    	List<WrapperExampleController.RowWrapper> rows=ctrl.getRowWrappers();
    	System.assertEquals(4, rows.size());
    }
}

  ```

Hint: To instantiate an extension controller, you need to pass it an instance of the standard controller, wrapping the standard object. E.g. <code>MyController ctrl=new MyController(new ApexPages.StandardController(new Contact()));</code>

##Tell me more##
In this workbook you have carried out simple business process automation through configuration and created Apex triggers and Visualforce pages.  This has only scratched the surface of development with the Salesforce1 platform - here are some resources to help you gain a deeper understanding and start building up your skills as a Salesforce developer:

###Salesforce Workbooks###

The Salesforce workbooks guide you through key functionality through a series of tutorials.

* Force.com Workbook - https://developer.salesforce.com/docs/atlas.en-us.workbook.meta/workbook/
* Apex Workbook - https://developer.salesforce.com/docs/atlas.en-us.apex_workbook.meta/apex_workbook/
* Visualforce Workbook - https://developer.salesforce.com/docs/atlas.en-us.workbook_vf.meta/workbook_vf/

Other workbooks are available - https://developer.salesforce.com/docs?filter_text=workbook&service=All+Services&select_type=&version=32.0&lang=en-us&sort=title

###Trailhead###

Trailhead (https://developer.salesforce.com/trailhead) is an interactive learning system for Salesforce developers, provided by Salesforce. At the time of writing (November 2014) the following trails are available :

* Getting started with Force.com - https://developer.salesforce.com/en/trailhead/trail/force_com_introduction
* Intro to Visual App Development (no code!) - https://developer.salesforce.com/en/trailhead/trail/force_com_declarative_beginner
* Intro to Programmatic App Development - https://developer.salesforce.com/en/trailhead/trail/force_com_programmatic_beginner

###Developer Guides###

* Force.com Apex Code Developers Guide - https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/
* Visualforce Developer's Guide - https://developer.salesforce.com/docs/atlas.en-us.pages.meta/pages/
