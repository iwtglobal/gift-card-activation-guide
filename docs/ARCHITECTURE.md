# Architecture — Gift Card Activation

High-level reference for evaluating **gift card activation** controls inside gift card and EVD platforms. Educational only; vendor designs vary.

## Logical Components

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│ Card inventory  │────►│ Sale / load      │────►│ Activation svc  │
│ inactive stock  │     │ POS / API / bulk │     │ + issuer ledger │
└─────────────────┘     └────────┬─────────┘     └────────┬────────┘
                                 │                        │
                                 ▼                        ▼
                        ┌──────────────────┐     ┌─────────────────┐
                        │ Fraud / block    │     │ Redeem gateway  │
                        │ + reverse rules  │     │ + settle export │
                        └──────────────────┘     └─────────────────┘
```

## Suggested Status Set

| Status | Meaning |
|--------|---------|
| `inactive` | Manufactured/generated; not spendable |
| `reserved` | Soft-locked during checkout |
| `active` | Value loaded; redeemable |
| `blocked` | Fraud or support hold |
| `depleted` / `expired` | Terminal non-spend paths |

## Audit Events Worth Keeping

- Manufacture / generate batch  
- Sale reserve and payment capture  
- Activate success / fail with reference  
- Reverse / reissue / block  
- First and last redeem markers  

Live product reading: [EVMS page](https://evdsystem.com/electronic-voucher-management-system/), [evdsystem.com](https://evdsystem.com/).
