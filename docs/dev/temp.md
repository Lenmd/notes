## Python

```python
🔵 创建环境 ➔ `conda create -n 名字 python=版本`  
🟢 进入环境 ➔ `conda activate 名字`  
🟡 退出环境 ➔ `conda deactivate`  
🔴 查看环境 ➔ `conda env list`


打印语句格式
```
默认情况下，print 命令始终以换行符结尾，但您也可以更改此设置。关键字参数 `end` 指定放在行尾的内容。将 `end` 设置为空字符串意味着打印输出末尾没有换行符：
```python
print("Hi ", end="")
print("there!")
```
## f-strings  F 字符串
```python
name = "Erkki"
age = 39
print(f"Hi {name} your age is {age} years")
```

格式说明符 `.2f` 表示我们要显示 2 位小数。末尾的字母 _f_ 表示我们希望将变量显示为`浮点`数，即浮点数。
```python
number = 1/3
print(f"The number is {number:.2f}")

```

这是另一个示例，其中我们指定为打印输出中的变量保留的空白量。两次将变量`名称`包含在结果字符串中时，它都保留了 15 个字符的空格。首先，名称向左对齐，然后向右对齐：
```python
names =  [ "Steve", "Jean", "Katherine", "Paul" ]
for name in names:
  print(f"{name:15} centre {name:>15}")
```

你可以将 f-string 视为一种函数，它根据大括号内的 “arguments” 创建一个普通字符串。


迭代
```python
for i in range(5):
    print(i)
```

```python
for i in range(3, 7):
    print(i)
```

```python
for i in range(1, 9, 2):
    print(i)
```

```python
for i in range(6, 2, -1):
    print(i)

6
5
4
3
```

列表

## 访问列表中的项
```python
my_list = [7, 2, 2, 5, 2]

#访问列表中项
print(my_list[0])
print(my_list[1])
print(my_list[3])
```
## 向列表添加项
```python
my_list.append(5)

# 添加到特定位置
my_list.insert(2, 10)
```
## 从列表中删除项
```python

# 删除根据索引删pop  根据内容删除remove
my_list.pop(2)   #返回已删除的项

my_list.remove(3) #删除第一次出现的值

#如果项不在列表中，remove函数会导致报错
#判断是否存在
if 1 in my_list:
	print(True)

```
## 对列表进行排序

可以使用 `sort` 方法将列表中的项从最小到最大_排序_ 。
```python
my_list = [2,5,1,2,4]
my_list.sort()
print(my_list)
```
有时我们不想更改原始列表，因此我们改用函数 `sorted`。它_返回_一个排序列表：
```python
my_list = [2,5,1,2,4]
print(sorted(my_list)))
```
## 最大值、最小值和总和
```python
my_list = [5, 2, 3, 1, 4]

greatest = max(my_list)
smallest = min(my_list)
list_sum = sum(my_list)

print("Smallest:", smallest)
print("Greatest:", greatest)
print("Sum:", list_sum)
```

## 方法和函数
```python
#方法调用
my_list.append(1)
my_list.sort()

#函数调用, 独立不依赖对象
max()
min()
sorted()
```