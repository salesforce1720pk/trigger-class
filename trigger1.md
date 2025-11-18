### Write a trigger whenever opportunity stage is changed to closed won the related account's rating is updated to cold
``` apex
trigger OpportunityTrigger on Opportunity (after insert, after update) {

    Set<Id> accIds = new Set<Id>();

    for (Opportunity opp : Trigger.new) {
        Opportunity oldOpp = Trigger.isUpdate ? Trigger.oldMap.get(opp.Id) : null;

        if (opp.StageName == 'Closed Won' &&
            opp.AccountId != null &&
            (
                Trigger.isInsert ||
                (Trigger.isUpdate && oldOpp.StageName != 'Closed Won')
            )
        ) {
            accIds.add(opp.AccountId);
        }
    }

    if (!accIds.isEmpty()) {
        List<Account> accList = [
            SELECT Id, Rating
            FROM Account
            WHERE Id IN :accIds
        ];

        for (Account acc : accList) {
            acc.Rating = 'Cold';
        }

        update accList;
    }
}

```
