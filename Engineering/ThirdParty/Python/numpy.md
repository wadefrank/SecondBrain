### shape

The shape of an array is the number of elements in each dimension.

```shell
import numpy as np

arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8]])

print(arr.shape)        # (2, 4)
print(arr.shape[0])     # 2
```

### sum

求和

### argmin

求最小值对应的索引

```shell
min_index = np.argmin(distances)
```