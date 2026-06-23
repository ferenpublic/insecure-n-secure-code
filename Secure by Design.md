# Secure by Design

Objek harus dibuat dalam kondisi yang valid sejak awal sehingga kesalahan penggunaan dapat dicegah.

### Insecure Code

```python
transfer = {
    "sender": "alice",
    "receiver": "alice",
    "amount": -100000,
    "status": "COMPLETED"
}
```

### Secure Code

```python
if sender == receiver:
    raise ValueError()

if amount <= 0:
    raise ValueError()

status = "CREATED"
```
