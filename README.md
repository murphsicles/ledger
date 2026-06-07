# ledger — Double-entry accounting state machine for Zeta

**Namespace:** `@accounting/ledger`

Double-entry accounting primitives: Accounts, Transfers with two-phase
commit (pending → post/void), balance enforcement, and cross-ledger
validation.

## Two-phase transfer model

1. **Create pending transfer** — reserves funds without applying them
2. **Post pending transfer** — applies reserved funds to both accounts
3. **Void pending transfer** — releases reserved funds without applying

## Balance enforcement

Accounts can be created with constraints:
- `A_DEBITS_MUST_NOT_EXCEED_CREDITS` — prevents overdrawing
- `A_CREDITS_MUST_NOT_EXCEED_DEBITS` — prevents credit overflow

## API

| Function | Description |
|----------|-------------|
| `Accounts_create(account)` | Create account with flags |
| `execute_transfer(transfer)` | Single-phase transfer |
| `execute_pending(transfer, timeout)` | Two-phase pending transfer |
| `execute_post(transfer_id, pending_id)` | Post (apply) pending |
| `execute_void(pending_id)` | Void (cancel) pending |
| `account_balance(account)` | Net balance (debits - credits) |

## Error codes

- `ERR_NONE` — success
- `ERR_EXISTS` — account already exists
- `ERR_EXCEEDS_CREDITS` — violates debit cap
- `ERR_EXCEEDS_DEBITS` — violates credit cap
- `ERR_PENDING_NOT_FOUND` — pending transfer missing
- `ERR_SAME_ACCOUNT` — debit == credit account

## License

MIT — The Zeta Foundation
