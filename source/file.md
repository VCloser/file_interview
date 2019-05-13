## 文件路径的比对

### 栈的使用

~~~python
def foo(path):
    li = path.split('/')
    stack = []
    for item in li:
        if item == '.':
            continue
        elif item == '..':
            stack.pop()
        else:
            stack.append(item)
    return '/'.join(stack)

ret = foo('a/b/c/./../d/')
print(ret)  # a/b/d/
~~~

