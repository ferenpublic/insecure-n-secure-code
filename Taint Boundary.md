# Taint Boundary

Data dari pengguna tidak boleh langsung dipercaya. Selalu lakukan validasi sebelum digunakan.

### Insecure Code

```python
# Langsung percaya token dari user
if user_input_token in reset_tokens:
    reset_password()
```

### Secure Code

```python
# Verifikasi token terlebih dahulu
if token.matches(user_input_token):
    reset_password()
```
