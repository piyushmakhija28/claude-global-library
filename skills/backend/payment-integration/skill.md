# Payment Integration Expert

## Skill Identity

- **Name:** payment-integration
- **Version:** 1.0.0
- **Type:** Implementation Specialist
- **Languages:** Java, Python, TypeScript
- **Domain:** Payment Gateway Integration

---

## Overview

Expert skill for integrating payment gateways (Stripe, Razorpay, PayPal, Square, Braintree) into applications with secure, production-ready implementations.

## Language-Specific Guides

| Language | File | Framework Support |
|----------|------|-------------------|
| Java | `java.md` | Spring Boot |
| Python | `python.md` | Flask, Django, FastAPI |
| TypeScript | `typescript.md` | Express, NestJS |

## Supported Payment Gateways

1. **Stripe** - Most popular globally, subscriptions, SaaS
2. **Razorpay** - India-focused, UPI, Netbanking
3. **PayPal** - International, established
4. **Square** - In-person + online payments
5. **Braintree** - PayPal subsidiary, mobile-first

## Key Features

All implementations include:

- Secure webhook handling with signature verification
- 3D Secure / Strong Customer Authentication
- PCI DSS compliance guidance
- Subscription billing
- Refund processing
- Comprehensive error handling
- Test card examples

## Context7 Integration

**ALWAYS use Context7 for latest payment gateway documentation!**

Payment SDKs update frequently with:
- Security patches
- New API versions
- Changed best practices
- Updated compliance requirements

## Usage

```bash
# Interactive
/payment-integration

# Language-specific
# Load java.md for Spring Boot implementations
# Load python.md for Flask/Django/FastAPI implementations
# Load typescript.md for Express/NestJS implementations
```

## Quick Reference

### Stripe Integration Flow

1. Create PaymentIntent (server-side)
2. Collect card details (client-side)
3. Confirm payment
4. Handle webhook for success/failure
5. Update database

### Webhook Security

All webhooks MUST verify signatures:

```java
// Java example
Webhook.constructEvent(payload, sigHeader, webhookSecret);
```

```python
# Python example
stripe.Webhook.construct_event(payload, sig_header, endpoint_secret)
```

```typescript
// TypeScript example
stripe.webhooks.constructEvent(body, sig, endpointSecret);
```

## Security Notes

- NEVER log full card numbers
- NEVER store raw card data
- ALWAYS verify webhook signatures
- ALWAYS use HTTPS
- Use idempotency keys for retries

## Location

`~/.claude/skills/backend/payment-integration/`

## Version History

- 1.0.0 (2026-01-26): Initial release with Java, Python, TypeScript support
