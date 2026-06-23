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
