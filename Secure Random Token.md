# Secure Random Token

Gunakan generator angka acak yang aman secara kriptografi agar token sulit ditebak.

### Insecure Code

```python
import random

token = str(random.randint(1000, 9999))
```

### Secure Code

```python
import secrets

token = secrets.token_urlsafe(32)
```
