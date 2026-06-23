# Domain Primitive

Token reset password sebaiknya bukan sekadar string biasa. Gunakan objek khusus yang memiliki aturan dan perilaku yang jelas sehingga lebih aman digunakan.

### Insecure Code

```python
# Token hanya string biasa
token = "abc123"

reset_tokens[token] = "alice@example.com"
```

### Secure Code

```python
class PasswordResetToken:
    def __init__(self):
        self.value = secrets.token_urlsafe(32)

token = PasswordResetToken()
```
