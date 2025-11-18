### Prevent the deletion of account record if it has more than 2 contacts
``` apex
trigger preventDeletion on Account (before delete) {

    // Collect Account IDs being deleted
    Set<Id> accIds = new Set<Id>();
    for (Account acc : Trigger.old) {
        accIds.add(acc.Id);
    }

    // Query accounts and their contacts
    List<Account> accList = [SELECT Id, (SELECT Id FROM Contacts)FROM AccountWHERE Id IN :accIds];

    // Check number of contacts
    for (Account acc : accList) {
        if (acc.Contacts.size() > 2) {
            acc.addError('You cannot delete this Account because it has more than 2 contacts.');
        }
    }
}

```
