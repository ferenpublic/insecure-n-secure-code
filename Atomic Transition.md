# Atomic Transition

Perubahan state dan pengecekan kondisi harus dilakukan sebagai satu operasi yang tidak bisa disela proses lain.

### Insecure Code

```python
if not voucher.used:

    # proses lain bisa masuk di sini

    voucher.used = True
```

### Secure Code

```python
with voucher.lock:

    if voucher.used:
        return

    voucher.used = True
```
