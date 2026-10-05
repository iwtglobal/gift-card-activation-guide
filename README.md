# Gift Card Activation Guide

An educational guide to the **gift card activation lifecycle**: how physical and digital gift cards move from manufactured or generated stock to spendable value. Written for fintech, retail, and payment teams evaluating platforms in the [EVD System](https://evdsystem.com/) and [MoboGage](https://evdsystem.com/about-mobogage/) family.

---

## What Is Gift Card Activation?

**Gift Card activation** is the controlled transition that loads face value onto a card PAN or digital token and makes it redeemable. Until activation, a card is typically inert inventory — printing or generating it does not create spendable liability.

Activation may happen at POS (swipe/scan at sale), via online purchase, through reseller EVD channels, or as a delayed “activate later” step for corporate bulk loads. Weak activation design causes inactive cards sold as live, double-activation on retries, and finance mismatches between load files and issuer ledgers.

### Why activation design matters

- **Liability timing** — value should enter issuer books only when activated  
- **Fraud control** — stolen inactive cards should remain worthless  
- **Channel integrity** — POS, e-commerce, and EVD paths need shared rules  
- **Support** — disputes need immutable activate / reverse / reissue history  

Activation is not a marketing checkbox; it is the liability gate for gift card programs.

---

## Architecture Overview: Activation Lifecycle

```
Manufacture / generate (inactive stock)
        │
        ▼
Inventory (available to sell)
        │
        ▼
Sale / load request ──► Activation service
        │                      │
        ▼                      ▼
  Payment capture         Card status → active
                               │
                               ▼
                         Redeem / settle network
                               │
                               └──► Deplete / expire / reverse
```

### Core layers

| Layer | Responsibility |
|-------|----------------|
| **Card inventory** | Tracks inactive, reserved, active, blocked, and expired units |
| **Activation service** | Loads value and flips status under idempotent rules |
| **Channel adapters** | POS, e-commerce, reseller API, corporate bulk load |
| **Issuer ledger** | Records liability when activation succeeds |
| **Redeem gateway** | Accepts spend only for active, non-blocked cards |

Gift platforms keep activation events append-only so finance and fraud teams share one truth.

---

## How Gift Card Activation Works

### 1. Create inactive stock

Physical cards or digital tokens are manufactured/generated with product SKU and optional pre-printed face value. Status remains inactive until a successful activation.

### 2. Sell or allocate

Retail POS, online checkout, or EVD reseller commits a sale. Payment capture (or corporate funding) is confirmed before activation proceeds.

### 3. Activate and load

The activation service loads face value (fixed or variable), sets status to active, and returns confirmation to the channel. Retries use the same activation reference.

### 4. Redeem and deplete

Customers spend at merchant or network. Partial redemptions reduce remaining balance; rules define whether split tender and cash-out are allowed.

### 5. Reverse, expire, or block

Returns, fraud blocks, and expiry policies create controlled status changes with dual control where required.

---

## Patterns and Use Cases

1. **Closed-loop retail gift cards** — Activate at POS; redeem only in the merchant network.  
2. **Open-loop / brand network cards** — Activation feeds network settlement after redeem.  
3. **Digital e-gift** — Instant activate-and-deliver after online payment.  
4. **Corporate bulk load** — Delayed activation against approved funding files.  
5. **EVD reseller distribution** — Agents sell and activate via electronic voucher channels.

Platforms such as EVD System / MoboGage support gift card and voucher distribution paths where activation, PIN reveal, and inventory status stay aligned across reseller tiers.

---

## Implementation Considerations

- **Idempotent activate** — one sale reference → at most one successful load  
- **Inactive until paid** — never activate on soft-authorize alone for high-risk channels  
- **Variable load limits** — min/max amounts and KYC-aware caps for open-loop  
- **Reversal policy** — define window and dual control for activate-reverse  
- **PAN / PIN vaulting** — secrets stay out of ordinary support screens  
- **Reporting grain** — sold-not-activated, active outstanding, redeemed, expired  

Choosing an activation model should prioritize liability timing and fraud resistance over checkout speed alone.

---

## FAQ

**Is printing a card the same as activating it?**  
No. Manufacture creates inactive inventory; activation loads spendable value.

**Can a card be activated twice?**  
A correct design rejects duplicate activates for the same card or sale reference.

**What is delayed activation?**  
Corporate or wholesale programs that allocate cards now and load value later under funding approval.

**How do digital e-gifts differ?**  
Delivery and activation are often combined after payment; the same ledger and status rules still apply.

**Where do EVD resellers fit?**  
They sell and activate under issuer rules; multi-tier float and commission sit beside activation events.

**How does this relate to MoboGage / EVD System?**  
EVD System is MoboGage’s electronic voucher distribution and management platform family; gift card activation is part of how digital value products move from inventory to redeem. See the [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/) overview when evaluating product fit.

---

## Further Reading / Related Industry Resources

- [Electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system-evd-telecom/) — EVMS product context  
- [EVD System home](https://evdsystem.com/) — platform overview for digital value distribution  

See also [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md) for a component view.

---

## License

Documentation in this repository is provided under the MIT License. See [LICENSE](./LICENSE).

*Educational material only. Not a substitute for vendor due diligence or regulatory advice. MoboGage and EVD System refer to offerings on evdsystem.com.*
