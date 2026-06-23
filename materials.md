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

# Boolean Explosion

Terlalu banyak variabel boolean dapat menghasilkan banyak kombinasi state, termasuk state yang tidak mungkin atau tidak valid.

### Insecure Code

```python
class Order:
    def __init__(self):
        self.paid = False
        self.shipped = True
        self.delivered = True

# State tidak valid tetapi tetap bisa dibuat
order = Order()
```

### Secure Code

```python
from enum import Enum

class OrderState(Enum):
    CREATED = 1
    PAID = 2
    SHIPPED = 3
    DELIVERED = 4

state = OrderState.PAID
```

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

# Race Condition

Masalah terjadi ketika dua proses mengubah data yang sama secara bersamaan sehingga hasil akhirnya tidak konsisten.

### Insecure Code

```python
if not voucher.used:
    voucher.used = True
    print("Voucher dipakai")
```

### Secure Code

```python
with voucher.lock:

    if voucher.used:
        raise ValueError()

    voucher.used = True
```

# Atomic Transition

Perubahan state dan pengecekan kondisi harus dilakukan sebagai satu operasi yang tidak bisa disela proses lain.

### Insecure Code

```python
if not voucher.used:

    # proses lain bisa masuk di sini

    voucher.used = True
```

### Secure Code

```python
with voucher.lock:

    if voucher.used:
        return

    voucher.used = True
```

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

# Secure by Design

Objek harus dibuat dalam kondisi yang valid sejak awal sehingga kesalahan penggunaan dapat dicegah.

### Insecure Code

```python
transfer = {
    "sender": "alice",
    "receiver": "alice",
    "amount": -100000,
    "status": "COMPLETED"
}
```

### Secure Code

```python
if sender == receiver:
    raise ValueError()

if amount <= 0:
    raise ValueError()

status = "CREATED"
```
