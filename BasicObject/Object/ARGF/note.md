# ARGF简介

## 处理文件参数

这个类主要作用是处理Ruby命令行程序的输入流。默认把输入参数（或`STDIN`）作为文件处理，把各个文件流连成一个输入流。

```ruby
# demo.rb
ARGF.each do |line|
  puts line
end
# 相当于
ARGV.each do |file|
  File.open(file, 'r').each_line do |line|
    puts line
  end
end
```

注意，当参数不是文件时会抛出异常`Errno::ENOENT`

## 处理标准输入

```bash
$echo "foo\nbar" | ruby demo.rb
```

当脚本调用没有带文件参数时，会从标准输入读取，这时就相当于调用

```ruby
while line = $stdin.gets
  puts line
end
```

## 常用api

```ruby
ARGF.argv.object_id == ARGV.object_id
ARGF.filename # 当前正在读取的文件名，如果是标准输入则返回 -
ARGF.file # 当前正读取的文件，File的实例
ARGF.file.lineno == 1 # 表示读取到文件的开头，可作为切换到新文件的标志
```

