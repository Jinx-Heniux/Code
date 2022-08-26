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

### 15.2.1 修改标签文字和线条粗细

```python
import matplotlib.pyplot as plt

squares = [1, 4, 9, 16, 25]
fig, ax = plt.subplots()
ax.plot(squares, linewidth=3)
ax.set_title("square numbers", fontsize=24)
ax.set_xlabel("value", fontsize=14)
ax.set_ylabel("square of value", fontsize=14)
ax.tick_params(axis='both', labelsize=14)

plt.show()
```

