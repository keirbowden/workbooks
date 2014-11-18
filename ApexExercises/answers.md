Introduction to Force.com/Apex Workshop Exercises - Answers
===================================================================

## Exercise Set 1: Automating Business Processes through Configuration ##

###Configuration Exercise 1: Create a Validation Rule###
  
![Validation Rule](https://lh3.googleusercontent.com/-qmop3FyDdhs/VGtbPmoKEEI/AAAAAAAABAs/lzC9YbGLmHI/w576-h628-no/Screen%2BShot%2B2014-11-18%2Bat%2B14.43.21.png)
  
###Configuration Exercise 2: Improve the Validation Rule###
  
![Improved Validation Rule](https://lh3.googleusercontent.com/-vEBVOvdMW80/VGtdvHaNTDI/AAAAAAAABBc/7F4TEmwfAuk/w577-h628-no/Screen%2BShot%2B2014-11-18%2Bat%2B14.54.05.png)

###Configuration Exercise 3: Create an Automated Email Alert###

####Email Template####

![Email Template](https://lh5.googleusercontent.com/-GRk-qMTb1yQ/VGthaaZbhUI/AAAAAAAABBs/AV9FPWkeruQ/w689-h478-no/Screen%2BShot%2B2014-11-18%2Bat%2B15.09.02.png)

***

####Email Alert####

![Email Alert Action](https://lh5.googleusercontent.com/-UIWsyWlVu2M/VGtheRY1aEI/AAAAAAAABCA/grsJE7q9_ac/w698-h229-no/Screen%2BShot%2B2014-11-18%2Bat%2B15.09.37.png)

***

####Workflow Rule####

![Workflow Rule](https://lh3.googleusercontent.com/-IzLi3jA9Tus/VGtheQkYXzI/AAAAAAAABCE/0TxXFmJIbmI/w673-h199-no/Screen%2BShot%2B2014-11-18%2Bat%2B15.09.53.png)

###Exercise Set 2: Apex###

###Apex Exercise 1: Create an Apex Trigger###

    trigger bg_contact_count on Contact (after insert) 
    {
      Id accountId=Trigger.new[0].AccountId;
    
      if (null!=accountId)
      {
          Account acc=[select id, Contact_Count__c,
                         (select id from Contacts) 
                       from Account 
                       where id=:accountId];
                       
          acc.Contact_Count__c=acc.contacts.size();
          update acc;
      }
    }

###Apex Exercise 2: Unit Test###

    @isTest
    private class TestContactTrigger
    {
        private static testMethod void TestAddPublic()
        {
            Account acc=new Account(Name='Unit Test');
            insert acc;
        
            Contact cont=new Contact(AccountId=acc.Id,
                                     FirstName='Keir',
                                     LastName='Bowden',
                                     Email='keirbowden@brightgen.com');
            insert cont;
        
            Account accFromDB=[select id, Contact_Count__c from Account where id=:acc.Id];
            System.assertEquals(accFromDB.Contact_Count__c, 1);
        }

        private static testMethod void TestAddPrivate()
        {
            Contact cont=new Contact(FirstName='Keir',
                                     LastName='Bowden');
            insert cont;
        }
    }
    
###Apex Exercise 3: Bulk Triggers###

    trigger bg_contact_count on Contact (after insert) 
    {
        Set<Id> accIds=new Set<Id>();
        for (Contact cont : trigger.new)
        {
            if (null!=contact.AccountId)
            {
                accIds.add(cont.AccountId);
            }
        }
    
        List<Account> accs=[select id, Contact_Count__c,
                             (select id from Contacts) 
                            from Account 
                            where id IN :accIds];

        for (Account acc : accs)
        {                     
            acc.Contact_Count__c=acc.contacts.size();
        }
    
        update accs;
    }

###Exercise Set 3: Visualforce###

###Apex Exercise 1: Standard Controller###

    <apex:page standardcontroller="Contact">
      <h1>Contact : <apex:outputField value="{!Contact.Name}"/></h1>
      <table style="width:50%">
        <tr>
          <td>Email:</td>
          <td><apex:outputField value="{!Contact.Email}"/></td>
          <td>Phone:</td>
          <td><apex:outputField value="{!Contact.Phone}"/></td>
        </tr>
        <tr colspan="4">
          <td><strong>Account Fields</strong></td>
        </tr>
        <tr>
          <td>Name:</td>
          <td><apex:outputField value="{!Contact.Account.Name}"/></td>
          <td>Industry:</td>
          <td><apex:outputField value="{!Contact.Account.Industry}"/></td>
        </tr>
      </table>
    </apex:page>
    
###Apex Exercise 2: Extension Controller###

####Controller####

    public class AccountContactEditExt
    {
        private ApexPages.StandardController std;
        public Account acc {get; set;}
    
        public AccountContactEditExt(ApexPages.StandardController stdCtrl)
        {
            std=stdCtrl;
            Contact cont=(Contact) stdCtrl.getRecord();
            acc=[select id, Name, Industry from Account where id=:cont.AccountId];
        }
    
        public PageReference save()
        {
            upsert acc;
            return std.save();
        }
    }

####Visualforce Page####
  
    <apex:page standardcontroller="Contact" extensions="AccountContactEditExt">
      <apex:outputField value="{!Contact.AccountId}" rendered="false"/>
      <apex:form >
          <h1>Contact : <apex:inputField value="{!Contact.Name}"/></h1>
          <table style="width:50%">
            <tr>
              <td>Email:</td>
              <td><apex:inputField value="{!Contact.Email}"/></td>
              <td>Phone:</td>
              <td><apex:inputField value="{!Contact.Phone}"/></td>
            </tr>
            <tr colspan="4">
              <td><strong>Account Fields</strong></td>
            </tr>
            <tr>
              <td>Name:</td>
              <td><apex:inputField value="{!acc.Name}"/></td>
              <td>Industry:</td>
              <td><apex:inputField value="{!acc.Industry}"/></td>
            </tr>
          </table>
          <apex:commandButton action="{!save}" value="Save"/>&nbsp;
          <apex:commandButton action="{!cancel}" value="Cancel"/>
      </apex:form>
    </apex:page>
    
