# Glossary — Mobile Money Bill Payment

Educational terminology for **mobile money bill payment** flows. Definitions are industry-oriented; vendor labels vary.

## Core Terms

| Term | Definition |
|------|------------|
| **Biller** | Organization that issues invoices or account balances payable via mobile money |
| **Biller directory** | Catalog of supported billers, product codes, and validation rules |
| **Inquiry** | Lookup of outstanding amount / account status before payment |
| **Quote** | Payable amount presented to the customer, including disclosed fees |
| **Payment intent** | Durable ID created before wallet debit; used for idempotency and retries |
| **Remittance** | Message or file that posts value to the biller or aggregator switch |
| **Aggregator / switch** | Intermediate that connects many wallets to many billers |
| **Receipt** | Customer-facing proof combining wallet debit and biller confirmation refs |
| **Pending debit** | Wallet state after debit instruction but before final biller ACK |
| **Reverse / reverse-pending** | Ledger action that restores wallet funds after failed remittance |
| **Velocity limit** | Cap on bill-pay count or volume over a time window |
| **Account token** | Stored reference to a biller account for faster repeat pay (not a secret PIN) |

## Flow States (Typical)

```
inquiry → quoted → authorized → debiting → remitting → confirmed
                                              └→ pending → reversed / ops review
```

## Roles

| Role | Concern |
|------|---------|
| **Customer** | Selects biller, confirms amount, receives receipt |
| **Agent (optional)** | Assists payment; needs dual-receipt discipline |
| **Wallet operator** | Owns ledger, fees, and customer support |
| **Biller / aggregator** | Validates accounts and posts remittances |

## Design Notes

- Treat inquiry results as **time-bound**; do not pay stale quotes without re-inquiry when policy requires.  
- Correlate **wallet debit ID** and **biller confirmation** on every success path.  
- Prefer explicit **pending** over silent success when the biller ACK is delayed.  
- Publish fee math before authorization so disputes stay rare.

## Related Reading

Live product reading: [electronic top-up for telecom airtime](https://evdsystem.com/evd-software/electronic-top-up-system-for-telecom-airtime/), [About MoboGage](https://evdsystem.com/about-mobogage/).

MoboGage / EVD System materials on evdsystem.com describe adjacent digital value distribution capabilities that often sit beside mobile money bill payment in telecom programs.
