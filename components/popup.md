# Popup 弹出菜单

我们经常会用到类似下拉菜单类似的组件，点击按钮，弹出菜单。但是，element-ui 提供的组件并不能满足现有的需求。所以我们制作了这个组件，称为 popup。

基于此组件，我们制作了表格字段筛选器，参见[表格字段筛选器](/components/field_filter)

!> alexeistukov-ui >= 1.0.0

## 用法

参考示例：

~~~javascript
// 表格字段筛选器的实现
<i-popup
    title="隐藏显示列"
    direction="north"
    :autoClose="false"
    icon-class="alexeistukov-icon icon-stop-square">
    <ul class="list">
        <i-checkbox-group
            v-model="checked"
            @change="handleCheckChange">
            <li class="list-item" v-for="item in items" :key="item">
                <i-checkbox :label="item"></i-checkbox>
            </li>
        </i-checkbox-group>
    </ul>
</i-popup>

data () {
    return {
        checked: []
    }
}

methods: {
    handleCheckChange (event) {
        this.$emit('change', event);
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
			<td>title</td>
			<td>触发器显示标题</td>
			<td>string</td>
			<td>——</td>
			<td>——</td>
		</tr>
        <tr>
			<td>direction</td>
			<td>菜单弹出方向</td>
			<td>string</td>
			<td>'south' | 'north'</td>
			<td>'south'</td>
		</tr>
        <tr>
			<td>iconClass</td>
			<td>触发器左侧展示字体图标类名</td>
			<td>string</td>
			<td>——</td>
			<td>——</td>
		</tr>
        <tr>
			<td>autoClose</td>
			<td>点击菜单时，是否自动关闭菜单</td>
			<td>boolean</td>
			<td>——</td>
			<td>——</td>
		</tr>
	</tbody>
</table>
