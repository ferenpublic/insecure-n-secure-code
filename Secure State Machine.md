# Secure State Machine

Gunakan state machine agar perpindahan state hanya bisa mengikuti aturan yang sudah ditentukan.

### Insecure Code

```python
order.status = "DELIVERED"

# Langsung lompat dari CREATED ke DELIVERED
```

### Secure Code

```python
order.pay()
order.ship()
order.deliver()

# Harus mengikuti urutan
```
