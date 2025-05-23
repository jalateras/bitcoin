# Bitcoin Script Opcodes

This document provides a comprehensive list of all Bitcoin script opcodes supported by Bitcoin Core, as defined in `src/script/script.h`.

## Push Value Operations (0x00-0x60)
```
OP_0 (OP_FALSE)     = 0x00   // Push empty array
OP_PUSHDATA1        = 0x4c   // Push data (1-byte length)
OP_PUSHDATA2        = 0x4d   // Push data (2-byte length)
OP_PUSHDATA4        = 0x4e   // Push data (4-byte length)
OP_1NEGATE          = 0x4f   // Push -1
OP_RESERVED         = 0x50   // Reserved (invalid)
OP_1 (OP_TRUE)      = 0x51   // Push 1
OP_2                = 0x52   // Push 2
OP_3                = 0x53   // Push 3
OP_4                = 0x54   // Push 4
OP_5                = 0x55   // Push 5
OP_6                = 0x56   // Push 6
OP_7                = 0x57   // Push 7
OP_8                = 0x58   // Push 8
OP_9                = 0x59   // Push 9
OP_10               = 0x5a   // Push 10
OP_11               = 0x5b   // Push 11
OP_12               = 0x5c   // Push 12
OP_13               = 0x5d   // Push 13
OP_14               = 0x5e   // Push 14
OP_15               = 0x5f   // Push 15
OP_16               = 0x60   // Push 16
```

**Note**: Opcodes 0x01-0x4b directly push that many bytes onto the stack.

## Control Flow (0x61-0x6a)
```
OP_NOP              = 0x61   // No operation
OP_VER              = 0x62   // Invalid (disabled)
OP_IF               = 0x63   // Execute if top stack is true
OP_NOTIF            = 0x64   // Execute if top stack is false
OP_VERIF            = 0x65   // Invalid (disabled)
OP_VERNOTIF         = 0x66   // Invalid (disabled)
OP_ELSE             = 0x67   // Else clause
OP_ENDIF            = 0x68   // End if clause
OP_VERIFY           = 0x69   // Fail if top stack is false
OP_RETURN           = 0x6a   // Immediately fail (provably unspendable)
```

## Stack Operations (0x6b-0x7d)
```
OP_TOALTSTACK       = 0x6b   // Move top item to alt stack
OP_FROMALTSTACK     = 0x6c   // Move top alt stack item to main stack
OP_2DROP            = 0x6d   // Drop top 2 items
OP_2DUP             = 0x6e   // Duplicate top 2 items
OP_3DUP             = 0x6f   // Duplicate top 3 items
OP_2OVER            = 0x70   // Copy 3rd and 4th items to top
OP_2ROT             = 0x71   // Move 5th and 6th items to top
OP_2SWAP            = 0x72   // Swap top 2 pairs
OP_IFDUP            = 0x73   // Duplicate if not zero
OP_DEPTH            = 0x74   // Push stack depth
OP_DROP             = 0x75   // Drop top item
OP_DUP              = 0x76   // Duplicate top item
OP_NIP              = 0x77   // Remove second item
OP_OVER             = 0x78   // Copy second item to top
OP_PICK             = 0x79   // Copy nth item to top
OP_ROLL             = 0x7a   // Move nth item to top
OP_ROT              = 0x7b   // Rotate top 3 items
OP_SWAP             = 0x7c   // Swap top 2 items
OP_TUCK             = 0x7d   // Copy top item to 3rd position
```

## Splice Operations (0x7e-0x82) - **DISABLED**
```
OP_CAT              = 0x7e   // Concatenate (disabled)
OP_SUBSTR           = 0x7f   // Substring (disabled)
OP_LEFT             = 0x80   // Left substring (disabled)
OP_RIGHT            = 0x81   // Right substring (disabled)
OP_SIZE             = 0x82   // String length
```

**Note**: Most splice operations are disabled for security reasons, except `OP_SIZE`.

## Bitwise Logic (0x83-0x8a)
```
OP_INVERT           = 0x83   // Bitwise NOT (disabled)
OP_AND              = 0x84   // Bitwise AND (disabled)
OP_OR               = 0x85   // Bitwise OR (disabled)
OP_XOR              = 0x86   // Bitwise XOR (disabled)
OP_EQUAL            = 0x87   // Equal
OP_EQUALVERIFY      = 0x88   // Equal then verify
OP_RESERVED1        = 0x89   // Reserved (invalid)
OP_RESERVED2        = 0x8a   // Reserved (invalid)
```

## Arithmetic (0x8b-0xa5)
```
OP_1ADD             = 0x8b   // Add 1
OP_1SUB             = 0x8c   // Subtract 1
OP_2MUL             = 0x8d   // Multiply by 2 (disabled)
OP_2DIV             = 0x8e   // Divide by 2 (disabled)
OP_NEGATE           = 0x8f   // Negate
OP_ABS              = 0x90   // Absolute value
OP_NOT              = 0x91   // Boolean NOT
OP_0NOTEQUAL        = 0x92   // Not equal to zero

OP_ADD              = 0x93   // Add
OP_SUB              = 0x94   // Subtract
OP_MUL              = 0x95   // Multiply (disabled)
OP_DIV              = 0x96   // Divide (disabled)
OP_MOD              = 0x97   // Modulo (disabled)
OP_LSHIFT           = 0x98   // Left shift (disabled)
OP_RSHIFT           = 0x99   // Right shift (disabled)

OP_BOOLAND          = 0x9a   // Boolean AND
OP_BOOLOR           = 0x9b   // Boolean OR
OP_NUMEQUAL         = 0x9c   // Numeric equal
OP_NUMEQUALVERIFY   = 0x9d   // Numeric equal then verify
OP_NUMNOTEQUAL      = 0x9e   // Numeric not equal
OP_LESSTHAN         = 0x9f   // Less than
OP_GREATERTHAN      = 0xa0   // Greater than
OP_LESSTHANOREQUAL  = 0xa1   // Less than or equal
OP_GREATERTHANOREQUAL = 0xa2 // Greater than or equal
OP_MIN              = 0xa3   // Minimum
OP_MAX              = 0xa4   // Maximum

OP_WITHIN           = 0xa5   // Within range
```

## Cryptographic Operations (0xa6-0xaf)
```
OP_RIPEMD160        = 0xa6   // RIPEMD-160 hash
OP_SHA1             = 0xa7   // SHA-1 hash
OP_SHA256           = 0xa8   // SHA-256 hash
OP_HASH160          = 0xa9   // SHA-256 then RIPEMD-160
OP_HASH256          = 0xaa   // Double SHA-256
OP_CODESEPARATOR    = 0xab   // Code separator for signatures
OP_CHECKSIG         = 0xac   // Check signature
OP_CHECKSIGVERIFY   = 0xad   // Check signature then verify
OP_CHECKMULTISIG    = 0xae   // Check multisignature
OP_CHECKMULTISIGVERIFY = 0xaf // Check multisignature then verify
```

## Expansion/NOP Operations (0xb0-0xb9)
```
OP_NOP1             = 0xb0   // No operation
OP_CHECKLOCKTIMEVERIFY = 0xb1 // Check locktime (BIP 65)
OP_NOP2             = 0xb1   // Alias for OP_CHECKLOCKTIMEVERIFY
OP_CHECKSEQUENCEVERIFY = 0xb2 // Check sequence (BIP 112)
OP_NOP3             = 0xb2   // Alias for OP_CHECKSEQUENCEVERIFY
OP_NOP4             = 0xb3   // No operation
OP_NOP5             = 0xb4   // No operation
OP_NOP6             = 0xb5   // No operation
OP_NOP7             = 0xb6   // No operation
OP_NOP8             = 0xb7   // No operation
OP_NOP9             = 0xb8   // No operation
OP_NOP10            = 0xb9   // No operation
```

**Note**: NOP opcodes are reserved for future soft-fork upgrades. Some have been repurposed through soft forks.

## Tapscript Operations (0xba)
```
OP_CHECKSIGADD      = 0xba   // Check signature and add (BIP 342 - Tapscript only)
```

**Note**: This opcode is only available in Tapscript contexts, not in legacy Bitcoin scripts.

## Invalid Operations
```
OP_INVALIDOPCODE    = 0xff   // Invalid opcode
```

## Important Notes

### Disabled Opcodes
Many opcodes are recognized but cause script failure when executed. These were disabled for security reasons, including most bitwise operations, multiplication, division, and string manipulation operations.

### Script Limits
- **Maximum script size**: 10,000 bytes (`MAX_SCRIPT_SIZE`)
- **Maximum stack size**: 1,000 items (`MAX_STACK_SIZE`)
- **Maximum ops per script**: 201 (`MAX_OPS_PER_SCRIPT`)
- **Maximum element size**: 520 bytes (`MAX_SCRIPT_ELEMENT_SIZE`)

### Version Compatibility
- **Legacy scripts**: Support opcodes 0x00-0xb9 (excluding disabled ones)
- **Tapscript**: Additionally supports `OP_CHECKSIGADD` and has modified behavior for some existing opcodes

### Reserved Opcodes
The following opcodes are reserved and cause script failure:
- `OP_RESERVED` (0x50)
- `OP_VER` (0x62)
- `OP_VERIF` (0x65)
- `OP_VERNOTIF` (0x66)
- `OP_RESERVED1` (0x89)
- `OP_RESERVED2` (0x8a)

### Future Extensibility
`OP_NOP1` through `OP_NOP10` are reserved for future protocol upgrades and currently do nothing when executed. Some have been repurposed through soft forks:
- `OP_NOP2` → `OP_CHECKLOCKTIMEVERIFY` (BIP 65)
- `OP_NOP3` → `OP_CHECKSEQUENCEVERIFY` (BIP 112)

## References
- **Source code**: `src/script/script.h` (lines 72-213)
- **Script interpreter**: `src/script/interpreter.cpp`
- **BIP 65**: CHECKLOCKTIMEVERIFY
- **BIP 112**: CHECKSEQUENCEVERIFY  
- **BIP 342**: Tapscript validation rules