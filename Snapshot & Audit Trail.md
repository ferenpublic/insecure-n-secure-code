# Snapshot & Audit Trail

Setiap perubahan penting harus dicatat agar riwayat aktivitas dapat ditelusuri kembali.

### Insecure Code

```python
payment.state = "PAID"

# Tidak ada catatan perubahan
```

### Secure Code

```python
audit_log.append({
    "actor": "payment-gateway",
    "old": "PENDING",
    "new": "PAID"
})

payment.state = "PAID"
```
