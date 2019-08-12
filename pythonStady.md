# 我的学习记录

### 2019/8/9

### 学习内容

###### Python 函数式编程
1. 高阶函数
	1. map/reduce
	2. filter
	3. sorted
2. 返回函数
	- 问题
		- 1 每次循环，都创建了一个新的函数，然后，把创建的3个函数都返回

```

			def count():
			    fs = []
			    for i in range(1, 4):
			        def f():
			             return i*i
			        fs.append(f)
			    return fs
		
			f1, f2, f3 = count()

```
	
		- 2 返回闭包时牢记一点：返回函数不要引用任何循环变量，或者后续会发生变化的变量

```
		- def count():
		    def f(j):
		        def g():
		            return j*j
		        return g
		    fs = []
		    for i in range(1, 4):
		        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
		    return fs
```