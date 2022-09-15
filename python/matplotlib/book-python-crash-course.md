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



### scatter() 单点

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



### scatter() 多点

```python
import matplotlib.pyplot as plt

x_values = [1, 2, 3, 4, 5]
y_values = [1, 4, 9, 16, 25]

plt.style.use('seaborn')
fig, ax = plt.subplots()

# ax.scatter(2,4, s=200)
ax.scatter(x_values,y_values,s=100)

# ax.plot(input_values, squares, linewidth=3)

ax.set_title("square numbers", fontsize=24)
ax.set_xlabel("value", fontsize=14)
ax.set_ylabel("square of value", fontsize=14)

ax.tick_params(axis='both', which='major', labelsize=14)

plt.show()

```



### 点的颜色

```python
import matplotlib.pyplot as plt

x_values = range(1,1001)
y_values = [x**2 for x in x_values]
# print(x_values)
# print(y_values)

# plt.style.use('seaborn')
fig, ax = plt.subplots()

# ax.scatter(2,4, s=200)
# ax.scatter(x_values,y_values,s=10,c='red')
# ax.scatter(x_values,y_values,s=10,c=[0,0.8,0])
ax.scatter(x_values,y_values,s=10,c=y_values,cmap=plt.cm.Blues)

# ax.plot(input_values, squares, linewidth=3)

ax.set_title("square numbers", fontsize=24)
ax.set_xlabel("value", fontsize=14)
ax.set_ylabel("square of value", fontsize=14)

# ax.tick_params(axis='both', which='major', labelsize=14)

ax.axis([0, 1100, 0, 1100000])

plt.show()

```

