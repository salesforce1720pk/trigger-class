#### Wehnver account is created with industry Banking ,create a contact with last name as the account's name and contact's phone number as account's Phone number
``` apex
trigger accountTrigger on Account(after insert) {

    List<Contact> conList = new List<Contact>();

    for (Account acc : Trigger.new) {

        // Condition: Only if industry is Banking
        if (acc.Industry == 'Banking') {

            Contact con = new Contact();
            con.LastName = acc.Name;
            con.AccountId = acc.Id;     // Required
            con.Phone = acc.Phone;      // Set phone (null is allowed)

            conList.add(con);
        }
    }

    if (!conList.isEmpty()) {
        insert conList;
    }
}

```
