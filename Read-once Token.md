# Read-once Token

Token reset password sebaiknya hanya bisa digunakan satu kali agar tidak bisa dipakai ulang oleh penyerang.

### Insecure Code

```python
# Token bisa dipakai berkali-kali
service.reset_password(token)
service.reset_password(token)
```

### Secure Code

```python
if token.used:
    raise ValueError("Token sudah digunakan")

token.used = True
reset_password()
```
