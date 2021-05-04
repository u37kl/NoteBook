# textarea

> 多行文本框标签，支持用户输入多行文字。
>
> textarea中的属性：
>
> 1. **`autofocus`**：网页加载完成后自动获得焦点，即自动选中，无需鼠标添加文本框选中后再输入文本了。
> 2. **`cols`**：设置文本框宽度，一行显示多少个字符
> 3. **`rows`**：设置文本框高度，一列显示多少个字符
> 4. **`disabled`**：不可选中
> 5. **`form`**：所属的表单
> 6. **`maxlength`**：文本最大输入多少个字符，一个中文字也是一个字符。
> 7. **`minlength`**：文本框最小输入多少个字符
> 8. **`name`**：文本框的name，会作为key提交给服务器
> 9. **`placeholder`**：占位提示文本
> 10. **`readonly`**：禁止输入字符
> 11. **`required`**：必填项
> 12. **`wrap`**：指定文本换行的方式。默认为soft。可能的值为：
>     1. hard: 在文本到达元素最大宽度的时候，在`进行表单提交`时，浏览器自动插入换行符(CR+LF) 。使用该属性值时`必须设置col属性`。
>     2. soft: 在到达元素最大宽度的时候，不会自动插入换行符。

```html
<label for="textA">评论</label>
<!-- 文本框输入文字个数为3～8个，页面加载时自动获取焦点，直接可以输入 -->
<textarea name="command" id="textA" cols="10"  placeholder="请输入......" wrap="soft" minlength="3" maxlength="8" rows="5" autocomplete="on"></textarea>
```

