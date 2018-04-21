---
title: jQuery获取表单值及属性内容
date: 2018-04-21 20:21:01
tags: Article
categories: jQuery
---
>平时前端在处理表单数据的时候，总是需要通过表单属性或者值来进行一些用户看不到的处理。
>比如：联动选项的处理、数据的验证过滤等。
>本节针对不同的表单内容进行了不同的获取方式。

### 获取当前表单元素的值
```
<input type="text" name="name" onblur="getValue(this)">
<script type="text/javascript">
	function getValue(obj){
	    // 获取值
		$(obj).val();
	}
</script>
```

### 获取表单元素值及属性内容
```
<input type="text" name="name">
<select name="sex" data-type="sex" field="sex">
	<option value="man">男</option>
	<option value="woman">女</option>
</select>
<script type="text/javascript">
	function do_submit(obj){
	    // 获取值
		var name = $("input[name='name']").val();
		var sex_value = $("select[name='sex']").val();
		// 获取特定文本值的数据
	    $('select[name="data[select]"] [value="man"]').val();
		// 获取文本值
		var sex_text = $("select[name='sex']").find('option:selected').text();
		// 获取data-type属性值
		var sex_type = $("select[name='sex']").data('type');
		var sex_type = $("select[name='sex']").attr('data-type');
		// 获取field属性值
		var sex_field = $("select[name='sex']").attr('field');
	}
</script>
```

### 获取多个多行文本框内容值
```
<textarea name="formula[]" class="text"></textarea>
<textarea name="formula[]" class="text"></textarea>
<textarea name="formula[]" class="text"></textarea>
<script type="text/javascript">
    var formula = new Array();
	$(".text,textarea").each(function(e){
	    formula.push($(this).val());
	})
</script>
```