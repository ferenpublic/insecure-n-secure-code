# Misuse Prevention

Desain kode agar kesalahan penggunaan menjadi sulit dilakukan, misalnya mencegah token dicetak ke log atau disimpan dalam bentuk asli.

### Insecure Code

```python
token = secrets.token_urlsafe(32)

print(token)  # Bocor ke log

database[token] = email
```

### Secure Code

```python
token_hash = hashlib.sha256(token.encode()).hexdigest()

database[token_hash] = email

print("Password reset requested")
```
