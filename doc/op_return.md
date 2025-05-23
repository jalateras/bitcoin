# OP_RETURN

OP_RETURN is a Bitcoin script opcode (value `0x6a`) that immediately causes script execution to fail, making any output containing it **provably unspendable**. This is by design - it allows data to be embedded in Bitcoin transactions without creating UTXOs that could bloat the blockchain state.

## Key Characteristics

**Immediate Failure**: When the script interpreter encounters OP_RETURN, it immediately returns `SCRIPT_ERR_OP_RETURN` (src/script/interpreter.cpp:666-669)

**Unspendable by Design**: Scripts starting with OP_RETURN are recognized as unspendable and can be pruned from the UTXO set (src/script/script.h:571-574)

**Data Embedding**: OP_RETURN outputs are commonly used to embed arbitrary data in transactions, such as metadata, timestamps, or protocol messages

## Policy Limits

**Size Limit**: By default, OP_RETURN data is limited to 83 bytes total (`MAX_OP_RETURN_RELAY = 83` in src/policy/policy.h:79)
- This includes 1 byte for OP_RETURN + up to 2 bytes for push opcodes + 80 bytes of actual data

**Single Output**: Only one OP_RETURN output is allowed per transaction (src/policy/policy.cpp:162-166)

**Configurable**: Node operators can:
- Disable OP_RETURN acceptance: `-datacarrier=0`
- Change size limit: `-datacarriersize=n`

## Script Classification

OP_RETURN scripts are classified as `NULL_DATA` type (src/script/solver.h:30), which means they:
- Start with OP_RETURN
- Contain only data push operations after OP_RETURN
- Are considered standard for relay and mining

## Implementation Details

### Script Validation
- **Definition**: `OP_RETURN = 0x6a` in src/script/script.h:111
- **Execution**: Immediate failure in script interpreter (src/script/interpreter.cpp:666-669)
- **Detection**: Scripts starting with OP_RETURN classified as NULL_DATA (src/script/solver.cpp:185-187)

### Policy Configuration
- **Default acceptance**: `DEFAULT_ACCEPT_DATACARRIER = true` (src/policy/policy.h:74)
- **Size validation**: Checked in standard script validation (src/policy/policy.cpp:94-98)
- **Transaction limits**: Only one OP_RETURN output per transaction (src/policy/policy.cpp:162-166)

### Command Line Options
- `-datacarrier`: Enable/disable OP_RETURN acceptance (default: true)
- `-datacarriersize`: Maximum OP_RETURN data size in bytes (default: 83)

## Common Use Cases

1. **Timestamping**: Embedding document hashes to prove existence at a specific time
2. **Protocol Data**: Various protocols (Omni, Colored Coins, etc.) use OP_RETURN for metadata
3. **Commitment Schemes**: Storing cryptographic commitments
4. **Digital Signatures**: Embedding signatures or attestations

## Example Script Structure
```
OP_RETURN <data>
```

The data portion can be up to 80 bytes and is typically pushed using appropriate push opcodes (OP_PUSHDATA1, etc.).

## Testing

Comprehensive tests for OP_RETURN functionality can be found in:
- **Functional tests**: test/functional/mempool_datacarrier.py
- **Unit tests**: src/test/script_standard_tests.cpp

## Historical Context

OP_RETURN was standardized to provide a clean alternative to various workarounds for data embedding that were creating unspendable UTXOs. By making data outputs explicitly unspendable, OP_RETURN prevents UTXO set bloat while still allowing necessary data embedding use cases.