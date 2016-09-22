# Encoding

```ruby
__ENCODING__ # 返回encoding对象，指定当前文件的编码
# 字符串的encode方法，第一参数为期望的编码，第二参数为当前使用编码
# ruby会使用第二个参数指定的编码来解释字符串
"你好".encode("utf-8", "gbk")
# 相同效果的代码：
"你好".force_encoding('gbk').encode('utf-8')
```

## force_encoding 与 encode 的区别

`force_encoding`
只改变字符串的编码信息，对象实际的字节内容不会改变，通常用于**修正**编码信息。
`encode` 改变编码信息的同时还改变字节内容

```ruby
m = "不知道"
m.bytes # => [228, 184, 141, 231, 159, 165, 233, 129, 147]
m.encoding # => #<Encoding:UTF-8>
y = m.encode 'gbk', 'utf-8' # => "\x{B2BB}\x{D6AA}\x{B5C0}"
m.bytes # => [228, 184, 141, 231, 159, 165, 233, 129, 147]
m.encoding # => #<Encoding:UTF-8>
y.bytes # => [178, 187, 214, 170, 181, 192]
y.encoding # => #<Encoding:GBK>

m.encode! 'gbk', 'utf-8' # => "\x{B2BB}\x{D6AA}\x{B5C0}"
m.encoding # => #<Encoding:GBK>
m.bytes # => [178, 187, 214, 170, 181, 192]
m # => "\x{B2BB}\x{D6AA}\x{B5C0}"
```

## 常量及类方法

支持的编码对象都以常量的方式放在`Encoding`下

```ruby
Encoding::UTF_8 # utf-8编码对象
Encoding::UTF_8.name # "UTF-8" 编码对象的字符串表示
Encoding::UTF_8.to_s # "UTF-8" 编码对象的字符串表示
Encoding::UTF_8.names # 编码对象的其他别名表示，返回字符串的数组

# 可以接受编码对象为参数的方法，都可以传入它的字符串表示，或别名表示。
"some string".encoding
#=> #<Encoding:UTF-8>
string = "some string".encode(Encoding::ISO_8859_1)
#=> "some string"
string.encoding
#=> #<Encoding:ISO-8859-1>
"some string".encode "ISO-8859-1"
#=> "some string"

Encoding.find('utf-8') # 查找编码对象
Encoding.aliases # 返回哈希，表示编码别名与编码原始名称的对应关系
Encoding.list
Encoding.name_list
Encoding.default_external
Encoding.default_internal
```

## 脚本编码

```ruby
# coding: utf-8
__ENCODING__ # => utf-8编码
```

## 实例方法

```ruby
ec = Encoding.find('utf-8')
ec.ascii_compatible? # true 是否与ASCII编码兼容
ec.dummy? # 是否为虚拟编码
ec.inspect # 返回字符串
ec.name
ec.names # 别名字符串数组
# 复制编码对象，并给它取名，若名字已使用会报错
ec.replicate('hello')
ec.to_s
```

