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
