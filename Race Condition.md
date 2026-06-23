# Race Condition

Masalah terjadi ketika dua proses mengubah data yang sama secara bersamaan sehingga hasil akhirnya tidak konsisten.

### Insecure Code

```python
if not voucher.used:
    voucher.used = True
    print("Voucher dipakai")
```

### Secure Code

```python
with voucher.lock:

    if voucher.used:
        raise ValueError()

    voucher.used = True
```
