# Expiry Invariant

Token harus memiliki batas waktu agar tidak bisa digunakan selamanya.

### Insecure Code

```python
# Token tidak pernah kedaluwarsa
reset_tokens[token] = email
```

### Secure Code

```python
if datetime.now() > token.expires_at:
    raise ValueError("Token expired")
```
