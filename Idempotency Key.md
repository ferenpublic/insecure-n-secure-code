# Idempotency Key

Request yang sama boleh dikirim berkali-kali, tetapi efek bisnisnya hanya boleh terjadi sekali.

### Insecure Code

```python
balance += 100000
balance += 100000

# Callback yang sama diproses dua kali
```

### Secure Code

```python
if tx_id in processed:
    return

balance += 100000

processed.add(tx_id)
```
