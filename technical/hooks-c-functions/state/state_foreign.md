---
description: >-
  Retrieve the data pointed to, on another account, by a Hook State key and
  write it to an output buffer
---

# state\_foreign

### Behaviour

* Read a 20 byte Account ID from the `aread_ptr`
* Read a 32 byte Hook State key from the `kread_ptr`
* Write the data (value) at that key at that Account ID to the buffer pointed to by `write_ptr`

### Definition

C

```c
int64_t state_foreign (
    uint32_t write_ptr,
    uint32_t write_len,
    uint32_t kread_ptr,
    uint32_t kread_len,
    uint32_t nread_ptr,
    uint32_t nread_len,
    uint32_t aread_ptr,
    uint32_t aread_len  
);
```

### Example

C

```c
#define SBUF(str) (uint32_t)(str), sizeof(str)
uint8_t ns[32] = {0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0};
int64_t lookup =
    state_foreign(SBUF(blacklist_status), SBUF(otxn_accid), SBUF(ns), SBUF(blacklist_accid));
if (lookup < 0)
     rollback(SBUF("Error: could not find key on foreign state."), 1);
```

### Parameters

| Name       | Type      | Description                                                                     |
| ---------- | --------- | ------------------------------------------------------------------------------- |
| write\_ptr | uint32\_t | A pointer to the buffer to write the data in the Hook State into.               |
| write\_len | uint32\_t | The length of the write buffer.                                                 |
| kread\_ptr | uint32\_t | Pointer to a buffer containing the Hook State key.                              |
| kread\_len | uint32\_t | The length of the Hook State key. (Should be 32.)                               |
| nread\_ptr | uint32\_t | A pointer to the buffer containing the 32 byte Namespace to lookup the state on |
| nread\_len | uint32\_t | The length of the namespace buffer (Should be 32).                              |
| aread\_ptr | uint32\_t | A pointer to a buffer containing the 20 byte Account ID to look up state on.    |
| aread\_len | uint32\_t | The length of the Account buffer. (Should always be 20).                        |

> ### 📘Hint
>
> Ensure you check the return value. A state lookup can fail of a range of reasons and the buffer will then contain whatever it did before the call (typically all zeros).

### Return Code

| Type     | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| int64\_t | <p>The number of bytes written to the write buffer.<br><br>If negative, an error:<br><code>OUT_OF_BOUNDS</code><br>- pointers/lengths specified outside of hook memory.<br><br><code>DOESNT_EXIST</code><br>- the specified Hook State key doesn't have an associated value on the ledger at the time of the call.<br><br><code>TOO_BIG</code><br>- the key specified by <code>read_ptr</code> and <code>read_len</code> was larger than 32 bytes.<br><br><code>TOO_SMALL</code><br>- the output buffer was too small to store the Hook State data.<br><br><code>INVALID_ACCOUNT</code><br>- the account specified at <code>aread_ptr</code> is invalid or does not exist.</p> |