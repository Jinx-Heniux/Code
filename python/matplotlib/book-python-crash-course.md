---
description: '2nd Edition: A Hands-On, Project-Based Introduction to Programming'
---

# Book - Python Crash Course

## 15.2 绘制简单的折线图

```python
import matplotlib.pyplot as plt

squares = [1, 4, 9, 16, 25]
fig, ax = plt.subplots()
ax.plot(squares)

plt.show()

```

