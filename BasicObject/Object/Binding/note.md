# Binding 简介

主要用于封装当前的执行上下文。通常是调用 `Kernel#binding` 来返回 `Binding` 对象。

```ruby
class Demo
  def initialize(n)
    @secret = n
  end

  def get_binding
    return binding()
  end
end

d1 = Demo.new(1)
b1 = d1.get_binding
d2 = Demo.new(2)
b2 = d2.get_binding

# binding 对象可以作为 Kernel#eval 方法的第二参数来建立执行环境。
eval("@secret", b1) # => 1
eval("@secret", b2) # => 2
eval("@secret")     # => nil
```

## 实例方法

### eval(string)

```ruby
def binding_eval(param)
  binding.eval('param')
end

binding_eval('hello') # => "hello"
binding_eval('world') # => "world"
```

### local_variable_defined?(symbol)

```ruby
def binding_defined?
  a = 1
  p binding.local_variable_defined?(:a)
  p binding.local_variable_defined?(:b)
end

binding_defined?
true
false
=> false
```

### local_variable_get(symbol)

```ruby
a = 1
binding.local_variable_get(:a) # => 1
binding.local_variable_get(:b) # => NameError
```

### local_variable_set(symbol, obj)

```ruby
a = 1
b = binding
b.local_variable_set(:a, 2) # 变量赋值
b.local_variable_set(:b, 3) # 变量创建并赋值，⚠️此时变量b与binding对象b不是同一个对象，变量b只存在与binging对象b中
p b # => binging对象
```

### local_variables

返回上下文中局部变量的`symbol`数组

### receiver

返回binding对象的接收者，也就是`self`


