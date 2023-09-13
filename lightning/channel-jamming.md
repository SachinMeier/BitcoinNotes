# Channel Jamming

Denial Of Service using Failed Payments

## Strategies

1. Fast Jamming: endless stream of fast-failing HTLCs.
2. Slow Jamming: send and hold set of HTLCs and hold until point of onchain fialure, then succeed. 
    - Why not just force it onchain? 


## Categories of Mitigations

### 1. Monetary

- Upfront fees
- PoW
- Tokens
- Burn

Problems:
- Order of magnitude difference between smallest and largest payment duration
    - Can't charge for duration of payment

- CLTV delta is a security parameter and needs to be large enough

- No option to cancel an HTLC early. 
    - WHY NOT


### 2. Reputation

- Whitelists
- Reputation scores

Problems: usually gameable

- Definition of Bad behavior is always loopholable. 
    - Resolution time? 
    - Success rate? 
    - Amount 

Difficult to detect, false positives punish bad users


## Hybrid Mitigation

Monetary approach addresses fast jamming & Repuation approach addresses slow jamming.

Threshold separates the two.

Upfront Fees? 

### Reputation Requirements 

Privacy? 
- Preserve existing guarantees 
- Local reputation ONLY
    - No global reputation
    - information source is only from direct peers

Centralizing Force? 
- Nodes will drop payments with low reputation to protect their own reputation
- De-risk forwarding of unknown payments
    - HOW? Endorsement!
- Barriers to entry? How do low nodes 
    - New nodes get bootstrapping and quality service
- Reputation AAS from surveiling entities
    - NO FINANCIAL benefit based on reputation
    - NO Steady State impact 

### Local Resource Conservation

Local Reputation -----\
                      |-----> 
HTLC Endorsement -----/

Endorsement is binary. 

example: 
```
ALICE <> BOB <> CAROL <> DAVE
```

Alice sends HTLC to Bob (Endorsement = 1)

Bob endorses HTLC IFF Alice is in good repuation && Alice forwarded HTLC as endorsed. 

Carol sends UNendorsed HTLC to Dave because Bob doesn't have good repuation for her. 

Dave sees unendorsed HTLC from Carol, and forwards unendorsed HTLC even though Carol is in good reputation. 

!! Cost of acquiring reputation is GREATER THAN reward for using reputation to attack. 



Local Reputation (ATTACK example)

ALICE <----> BOB <--target of attack--> CAROL


Alice is attacking Bob's channel with Bob
cost of reputation = sent by Alice to BOb, because Attacker has to pay for reputation.

Fee Forwaded - Risk(HTLCs) > TargetChannel Revenue (over time period)

- Reputation should force attackers to mimic honest actions
- No advantage for malicious bootstrapping

Opportunity Cost: 
- <90 seconds = free
- rest is charged

IF HTLC Endorsed:
    SUCCESS -> fee - Opportunity Cost 

    FAILED -> -Opportunity Cost

IF HTLC UNendorsed:
    SUCCESS -> fee

    FAILED -> 0 

In Flight Risk: CLTV EXPIRY - Height_added * 10 * 60
    - Low impact for regular payments
    - up front accountability for many inflight payments
    - your reputation is docked while you 

### Resource Bucketing

Define "Slots" for HTLCs. Bucket these slots into "buckets" of HTLCs.

Some slots are privileged and only accept endorsed or good reputation HTLCs.

Other Slots are free and open to any HTLCs. HTLC jamming can fill this up but it won't block endorsed HTLCs

Steady State (no attacks): neither bucket should fill up. HTLCs resolve quickly. 

Attack State: Only latter bucket will fill up

Attacker can't move into protected bucket, because to do so, they need to endorse the HTLC, and doing so will put them in bad reputation

Q: What about Hold Invoices? 
A: Sender should not endorse them. 