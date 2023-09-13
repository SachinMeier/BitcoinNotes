# OP_CHECKLOCKTIMEVERIFY

OP_CHECKLOCKTIMEVERIFY (OP_CLTV) is a script opcode that allows you to lock up coins until a certain block height by requiring that an transaction spending the UTXO has a locktime number greater than a certain value.


## Bitcoin Core Source

[link](https://github.com/bitcoin/bitcoin/blob/v0.12.0/src/script/interpreter.cpp#L334-L374)

```cpp
case OP_CHECKLOCKTIMEVERIFY:
{
    if (!(flags & SCRIPT_VERIFY_CHECKLOCKTIMEVERIFY)) {
        // not enabled; treat as a NOP2
        if (flags & SCRIPT_VERIFY_DISCOURAGE_UPGRADABLE_NOPS) {
            return set_error(serror, SCRIPT_ERR_DISCOURAGE_UPGRADABLE_NOPS);
        }
        break;
    }
    if (stack.size() < 1)
        return set_error(serror, SCRIPT_ERR_INVALID_STACK_OPERATION);
    // Note that elsewhere numeric opcodes are limited to
    // operands in the range -2**31+1 to 2**31-1, however it is
    // legal for opcodes to produce results exceeding that
    // range. This limitation is implemented by CScriptNum's
    // default 4-byte limit.
    //
    // If we kept to that limit we'd have a year 2038 problem,
    // even though the nLockTime field in transactions
    // themselves is uint32 which only becomes meaningless
    // after the year 2106.
    //
    // Thus as a special case we tell CScriptNum to accept up
    // to 5-byte bignums, which are good until 2**39-1, well
    // beyond the 2**32-1 limit of the nLockTime field itself.
    const CScriptNum nLockTime(stacktop(-1), fRequireMinimal, 5);
    // In the rare event that the argument may be < 0 due to
    // some arithmetic being done first, you can always use
    // 0 MAX CHECKLOCKTIMEVERIFY.
    if (nLockTime < 0)
        return set_error(serror, SCRIPT_ERR_NEGATIVE_LOCKTIME);
    // Actually compare the specified lock time with the transaction.
    if (!checker.CheckLockTime(nLockTime))
        return set_error(serror, SCRIPT_ERR_UNSATISFIED_LOCKTIME);
    break;
}
```

## Sources

[BIP65](https://github.com/bitcoin/bips/blob/master/bip-0065.mediawiki)