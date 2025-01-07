---
description: Search for a keylet within a specified range on the current ledger
---

# ledger\_keylet

### Behaviour

* Read a 34 byte Keylet from the `lread_ptr`
* Read a 32 byte Keylet from the `hread_ptr`
* Search the ledger for the first (lowest) Keylet of this type in this range.
* If any matching Keylet is found, write it to `write_ptr`.

### Definition

C

```c
int64_t ledger_keylet (
    uint32_t write_ptr,
    uint32_t write_len,
    uint32_t lread_ptr,
    uint32_t lread_len,
    uint32_t hread_ptr,
    uint32_t hread_len
);
```

### Example

C

```c
//TODO
```

### Parameters

| Name       | Type      | Description                                                                                                |
| ---------- | --------- | ---------------------------------------------------------------------------------------------------------- |
| write\_ptr | uint32\_t | Pointer to a buffer to store the output serialised Keylet. .                                               |
| write\_len | uint32\_t | Length of the output buffer. Must be 34 bytes                                                              |
| lread\_ptr | uint32\_t | Pointer to the 34 byte serialised Keylet that represents the lower boundary of the Keylet range to search. |
| lread\_len | uint32\_t | Always 34 bytes                                                                                            |
| hread\_ptr | uint32\_t | Pointer to the 34 byte serialised Keylet that represents the upper boundary of the Keylet range to search. |
| hread\_len | uint32\_t | Always 34 bytes                                                                                            |

### Return Code

| Type     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| int64\_t | <p>The number of bytes written (34 bytes) on success.<br><br>If negative, an error:<br><code>OUT_OF_BOUNDS</code><br>- pointers/lengths specified outside of hook memory.<br><br><code>TOO_SMALL</code> / <code>TOO_BIG</code><br>- <code>write_len</code>, <code>lread_len</code> or <code>hread_len</code> was not 34 bytes<br><br><code>INVALID_ARGUMENT</code><br>- One or more of the provided Keylets was not a valid serialised Keylet<br><br><code>DOES_NOT_MATCH</code><br>- The two provided Keylets were not of the same Keylet Type.<br><br><code>DOESNT_EXIST</code><br>- No matching Keylet was found in the specified range.</p> |