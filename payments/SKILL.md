---
name: payments
description: "Stripe payment integration expertise. Use when implementing Stripe Checkout, Payment Intents, Stripe Elements, Stripe Billing, subscription billing, idempotency keys, Stripe webhook handling, refunds, chargebacks, PCI DSS compliance, dunning, invoicing, or any feature involving money movement via Stripe."
---

# Payment Processing (Stripe)

## Core Principles

### 1. Idempotency is Non-Negotiable
Every payment initiation MUST be idempotent. Network retries, user double-clicks, and page refreshes cause duplicate requests. Without idempotency keys, users get double-charged. Frontend button disabling is not sufficient — you need server-side idempotency.

### 2. Webhooks are the Source of Truth
Never trust the client redirect for payment confirmation. The processor's webhook is the authoritative signal. Build your state machine around webhook events, not redirect parameters.

### 3. Never Touch Raw Card Data
Use Stripe Checkout (hosted) or Stripe Elements (embedded iframe). Both keep raw card data off your server. PCI SAQ-A scope is 22 self-assessment questions vs a full QSA audit.

### 4. Money is Always Integers
Store amounts in the smallest currency unit — cents for USD. `$12.99` → `1299`. Never use floats: `0.1 + 0.2 = 0.30000000000000004`.

### 5. Subscriptions are a State Machine
States: `trialing → active → past_due → canceled → unpaid`. Handle every transition explicitly.

## Which Stripe Product to Use

| Use Case | Stripe Product |
|----------|---------------|
| One-time payments (fastest) | Stripe Checkout |
| Custom payment UI | Stripe Elements |
| Subscriptions + invoicing | Stripe Billing |
| Customer self-service billing | Stripe Customer Portal |
| Marketplaces / platforms | Stripe Connect |

**Decision rule**: Use Stripe Checkout unless you need a custom UI. Use Stripe Billing for any recurring revenue.

## Payment Flow Architecture

```
Client → Server → Stripe
          ↓           ↓
     DB (idempotency) Webhook
          ↓           ↓
     State Machine ←── Event
```

1. Client initiates → server creates PaymentIntent + writes idempotency key to DB
2. Client confirms on Stripe → Stripe processes
3. Stripe fires webhook → server validates signature → updates payment state
4. UI reflects state from DB, not from client redirect

## Data Model

```sql
payments (
  id               uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  idempotency_key  varchar(255) UNIQUE NOT NULL,
  user_id          uuid REFERENCES users NOT NULL,
  amount_cents     integer NOT NULL,
  currency         char(3) NOT NULL DEFAULT 'usd',
  status           payment_status NOT NULL, -- pending|processing|completed|failed|refunded
  processor_id     varchar(255),             -- Stripe PaymentIntent ID
  amount_refunded_cents integer NOT NULL DEFAULT 0,
  created_at       timestamptz NOT NULL DEFAULT now()
);

subscriptions (
  id                   uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id              uuid REFERENCES users NOT NULL,
  plan_id              varchar(100) NOT NULL,
  status               subscription_status NOT NULL, -- trialing|active|past_due|canceled|unpaid
  current_period_end   timestamptz,
  cancel_at_period_end boolean NOT NULL DEFAULT false,
  processor_id         varchar(255),
  created_at           timestamptz NOT NULL DEFAULT now()
);
```

## Refunds and Chargebacks

- Refund through Stripe's API (`stripe.refunds.create`) — never create a new payment
- Track `amount_refunded_cents` to prevent over-refunding
- Chargebacks are initiated by the card network via `charge.dispute.created` webhook — respond with evidence within Stripe's deadline (typically 7–21 days)

## Testing

| Scenario | Stripe Test Card |
|----------|------------------|
| Success | `4242 4242 4242 4242` |
| Declined | `4000 0000 0000 0002` |
| Insufficient funds | `4000 0000 0000 9995` |
| 3DS required | `4000 0025 0000 3155` |
| Webhook testing | `stripe listen --forward-to localhost:3000/webhooks/stripe` |

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "We'll add idempotency later" | You'll add it after the first double-charge complaint, under pressure. |
| "Frontend success callback is enough" | Redirects fail, tabs close, JS throws. Webhooks are the only reliable signal. |
| "Floats are fine for money" | `0.1 + 0.2 = 0.30000000000000004`. Use integers. |
| "PCI is optional for small apps" | If your checkout submits to your server, you're in scope. SAQ-A is 22 questions and costs nothing. |

## Verification

- [ ] All payment requests include idempotency keys written to DB before calling Stripe
- [ ] Payment status updated via webhook only — not from client-side callbacks
- [ ] Webhook signatures verified before processing any event
- [ ] All monetary amounts stored as integers (cents/pence)
- [ ] No raw card data passes through your server
- [ ] Test mode keys used in all non-production environments
- [ ] Webhook processing is itself idempotent (re-delivery safe)
- [ ] Subscription state machine handles all transitions including payment failure
- [ ] Refund logic guards against over-refunding
