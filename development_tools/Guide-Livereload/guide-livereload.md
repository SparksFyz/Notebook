## 如何使用 guide-livereload

[guard-livereload官网](https://rubygems.org/gems/guard-livereload)

[guard-livereload github](https://github.com/guard/guard-livereload)


### 安装

1. `sudo gem install guard-livereload`
2. 进入工程目录， `guard init livereload`

### 配置

> 栗子(根据需要修改自己的配置)

```ruby
guard 'livereload' do
  watch(%r{app/views/.+\.(erb|haml|slim)})
  watch(%r{app/helpers/.+\.rb})
  watch(%r{public/.+\.(css|js|html)})
  watch(%r{config/locales/.+\.yml})
  # Rails Assets Pipeline
  watch(%r{(app|vendor)(/assets/\w+/(.+\.(css|js|html))).*}) { |m| "/assets/#{m[3]}" }
end
```

```ruby
guard 'livereload' do
  watch(%r{app/views/.+\.(erb|haml|slim)})
  watch(%r{app/helpers/.+\.rb}) watch(%r{public/.+\.(css|js|html)})
  watch(%r{config/locales/.+\.yml})
  # Rails Assets Pipeline watch(%r{(app|vendor)(/assets/\w+/(.+\.(css|js|html))).*}) { |m| "/assets/#{m[3]}" } end
```

