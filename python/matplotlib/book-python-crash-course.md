---
description: '2nd Edition: A Hands-On, Project-Based Introduction to Programming'
---

# Book - Python Crash Course

## 15.2 绘制简单的折线图

```python
import matplotlib.pyplot as plt

input_values = [1, 2, 3, 4, 5]
squares = [1, 4, 9, 16, 25]
fig, ax = plt.subplots()
ax.plot(input_values, squares, linewidth=3)
ax.set_title("square numbers", fontsize=24)
ax.set_xlabel("value", fontsize=14)
ax.set_ylabel("square of value", fontsize=14)
ax.tick_params(axis='both', labelsize=14)

plt.show()

```



### scatter()

```python
import matplotlib.pyplot as plt

plt.style.use('seaborn')

# input_values = [1, 2, 3, 4, 5]
# squares = [1, 4, 9, 16, 25]

fig, ax = plt.subplots()

ax.scatter(2,4, s=200)

# ax.plot(input_values, squares, linewidth=3)

ax.set_title("square numbers", fontsize=24)
ax.set_xlabel("value", fontsize=14)
ax.set_ylabel("square of value", fontsize=14)

ax.tick_params(axis='both', which='major', labelsize=14)

plt.show()

```

