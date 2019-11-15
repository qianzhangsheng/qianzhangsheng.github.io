# 表格字段筛选器

因为业务需要，我们新增了表格字段筛选器，请参考以下内容。

此组件基于 popup 组件制作。（参见[popup](/popup))

!> alexeistukov >= 1.0.0-beta.16

## 用法

参考示例：

~~~javascript
<i-field-filter v-model="fields"></i-field-filter>

data () {
    return {
        fields: {
			a: {
				label: 'a',
				checked: true
			}
		}
    }
}
~~~

## Attributes

<table width="100%" cellspacing="0" cellpadding="0" border="1" style="border-collapse: collapse;display: table;text-align: center;">
	<thead>
		<tr>
			<th>参数</th>
			<th>说明</th>
			<th>类型</th>
			<th>可选值</th>
			<th>默认值</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>v-model</td>
			<td>筛选字段对象</td>
			<td>Object</td>
			<td>——</td>
			<td>——</td>
		</tr>
	</tbody>
</table>

## Filtered Field Object

~~~javascript
{
	fieldName: {
		label,
		checked
	}
}
~~~
