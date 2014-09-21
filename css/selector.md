
## 选择器


#### [强制] 如无必要，不得为 `id`、`class` 选择器添加类型选择器进行限定。

解释：

1. 对性能有影响。
2. 老旧浏览器的兼容性存在问题。

示例：


```css
/* good */
#error,
.danger-message {
    font-color: #c00;
}

/* bad */
dialog#error,
p.danger-message {
    font-color: #c00;
}
```

#### [建议] 选择器的嵌套层级应不大于 3 级，位置靠后的限定条件应尽可能精确。

示例：

```css
/* good */
#username input {}
.comment .avatar {}

/* bad */
.page .header .login #username input {}
.comment div * {}
```


