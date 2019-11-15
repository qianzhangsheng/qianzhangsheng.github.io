# 右键菜单

因为业务需要，我们制作了右键菜单组件，使用方法请参考以下内容。

!> alexeistukov >= 1.0.0-beta.12

## 用法

参考示例：

~~~javascript
// 加载插件
Vue.install(alexeistukov.contextmenu);

// 使用
this.$showContextMenu([
    { label: String, handler: Function, children: Array }
], mounseevent);
~~~

## Methods

<table width="100%" cellspacing="0" cellpadding="0" border="1" style="border-collapse: collapse;display: table;text-align: center;">
	<thead>
		<tr>
			<th>方法</th>
			<th>说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>showContextMenu</td>
			<td>展示右键菜单</td>
		</tr>
	</tbody>
</table>

## Arguments

<table width="100%" cellspacing="0" cellpadding="0" border="1" style="border-collapse: collapse;display: table;text-align: center;">
	<thead>
		<tr>
			<th>参数</th>
			<th>类型</th>
            <th>默认值</th>
			<th>说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>menuItems</td>
			<td>Array&lt;Object&gt;</td>
            <td>-</td>
			<td>菜单项</td>
		</tr>
        <tr>
			<td>mouseevent</td>
			<td>MouseEvent</td>
			<td>-</td>
			<td>鼠标事件对象</td>
		</tr>
	</tbody>
</table>
