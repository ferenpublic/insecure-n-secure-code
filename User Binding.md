# User Binding

Token harus terikat ke pengguna tertentu sehingga tidak bisa digunakan untuk akun lain.

### Insecure Code

```python
# Siapa pun yang punya token bisa reset password
reset_password(token)
```

### Secure Code

```python
if token.user_id != user.user_id:
    raise ValueError("Token bukan milik user ini")

reset_password()
```
