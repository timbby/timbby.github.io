---
title: ruby selenium-webdriver 使用记录
date: 2017-08-25 18:22:00
tags:
 - crawler
 - ruby
 - selenium-webdriver
---

### WHAT
记录一下ruby下，selenium-webdriver的使用方法；

### WHY
最近在用selenium-webdriver抓取数据，但是好像没有找到什么相关的文档，许多东西只能一点点的找，用过后又总是忘记，就写下来，以备查阅；

<!-- more -->

### HOW
#### 安装gem包
```shell
	gem install selenium-webdriver
```
#### 引入gem包
```shell
require 'selenium-webdriver'
```

#### 正常使用（chrome）
```ruby
# 会打开一个谷歌浏览器
dr = Selenium::WebDriver.for :chrome
```

#### 无头浏览器（chrome）
```ruby
# 会在后台打开一个浏览器（headless）
options = Selenium::WebDriver::Chrome::Options.new
options.add_argument('--headless')
options.add_argument('--disable-gpu')
dr = Selenium::WebDriver.for :chrome, options: options
```

#### 切换标签页
```ruby
dr.window_handles #返回已有的标签页id
dr.window_handle  #返回当前标签页id
dr.switch_to.window dr.window_handles[1] #切换到第一个标签页
```

#### 设置超时时间
```ruby
dr.manage.timeouts.page_load = 30
begin
  dr.get 'https://www.example.com'
rescue
  retry if dr.find_elements(:css, 'div.content[id="1"]').length < 1
end
```

#### 获取网页源码
```ruby
page = dr.page_source
```

#### 获取属性
```ruby
class = dr.find_elements(:css, 'div#div-id').attribute('class')
```

#### 点击事件
```ruby
dr.find_elements(:css, 'div#content[id="1"]').click
```

### END

<div style="font-size: 25px; font-weight: 3px; text-align: center;">未完待续……</div>
