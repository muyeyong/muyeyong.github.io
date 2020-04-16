---
title: HTML-表单
top: false
cover: false
toc: true
mathjax: true
date: 2020-04-15 16:01:22
password:
summary:
tags: HTML基础
categories: HTML
---

# 表单

> 由三部分组成，表单域，表单元素和提示信息



## 表单域

>  form 设置表单域

```html
<from action="url地址"  method="提交方式" name="表单域名称">
    表单元素
</from>
```

## 表单元素

> 允许用户输入或选择的内容控件

### input标签

```html
<input type='属性值'/>
```

+ 根据type的不同可以指定不同的控件类型


#### 其他常用属性

| 属性      | 属性值     | 描述                |
| --------- | ---------- | ------------------- |
| name      | 用户自定义 | 定义input元素的名称 |
| value     | 用户自定义 | 规定input元素的值   |
| checked   | checked    | 设置是否被选中      |
| maxlength | 正整数     | input的最大输入长度 |

+ 对于单选和复选需要设定相同的name
+ checked是对单复选框使用的

####   label标签

> 绑定表单元素，当点击lable标签包含的文本时，浏览器会自动将焦点转到对应的表单元素上去

```html
<lable for='sex'>男</lable>
<input type='radio' name = 'sex'  id = 'sex' />
```

+ lable标签的for属性要与相关标签的id属性相同

### select

```html1
<select>
	<option>选项名</option>
	...........
</select>
```

+ 可以设置option的selected属性为'selected'选中

### textarea

> 定义多行文本控件

```
<textarea>

</textarea>
```

| 属性 | 属性值       | 说明                            |
| ---- | ------------ | ------------------------------- |
| clos | 每行中的字符 | 实际开发中不会使用，通过css控制 |
| rows | 自大输入行数 | 实际开发中不会使用，通过css控制 |

