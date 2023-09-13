# Lightning Penalties

In order to create new state, both parties must revoke the old state. So, as long as your local data is reliable, your counterparty should not be able to create a new state and revoke old state without your cooperation. 

the `to_self_delay` determines how long a party can safely be offline before their counterparty can steal funds successfully. This is because when the counterparty claims `to_local` funds, those funds go to a script which allows the party `to_self_delay` relative time to revoke that state if it is old. 

If a party is out of sync with the blockchain, they are susceptible to potential broadcasting bad state. 

