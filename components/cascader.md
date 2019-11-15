# Cacader级联选择器

当一个数据集合有清晰的层级结构时，可通过级联选择器逐级查看并选择。(具体示例参见[element](http://element.eleme.io/#/zh-CN/component/cascader))

!> alexeistukov >= 0.3.7

## Attributes

<table width="100%" cellspacing="0" cellpadding="0" border="1" style="border-collapse:collapse;display:table;">
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
			<td>options</td>
			<td>选项数据源，键名可通过props配置</td>
			<td>array</td>
			<td>——</td>
			<td>——</td>
		</tr>
		<tr>
			<td>props</td>
			<td>配置选项键名</td>
			<td>object</td>
			<td>——</td>
			<td>——</td>
		</tr>
		<tr>
			<td>value</td>
			<td>选中项对应值</td>
			<td>array</td>
			<td>——</td>
			<td>——</td>
		</tr>
		<tr>
			<td>placeholder</td>
			<td>输入框占位文本</td>
			<td>string</td>
			<td>——</td>
			<td>请选择</td>
		</tr>
		<tr>
			<td>disabled</td>
			<td>是否禁用</td>
			<td>array</td>
			<td>boolean</td>
			<td>false</td>
		</tr>
		<tr>
			<td>clearable</td>
			<td>是否可清空</td>
			<td>array</td>
			<td>boolean</td>
			<td>false</td>
		</tr>
		<tr>
			<td>expand-trigger</td>
			<td>次级菜单的展开方式</td>
			<td>string</td>
			<td>click / hover</td>
			<td>click</td>
		</tr>
		<tr>
			<td>show-all-levels</td>
			<td>输入框中是否显示选中值的完整路径</td>
			<td>boolean</td>
			<td>——</td>
			<td>true</td>
		</tr>
		<tr>
			<td>filterable</td>
			<td>是否可搜索选项</td>
			<td>boolean</td>
			<td>——</td>
			<td>——</td>
		</tr>
		<tr>
			<td>change-on-select</td>
			<td>是否允许选择任意一级的选项</td>
			<td>boolean</td>
			<td>——</td>
			<td>false</td>
		</tr>
		<tr>
			<td>size</td>
			<td>尺寸</td>
			<td>string</td>
			<td>large / small / mini</td>
			<td>——</td>
		</tr>
	</tbody>
</table>

## props

<table width="100%" cellspacing="0" cellpadding="0" border="1" style="border-collapse:collapse;display:table;">
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
			<td>label</td>
			<td>指定选项的标签为选项对象的某个属性值</td>
			<td>string</td>
			<td>——</td>
			<td>——</td>
		</tr>
		<tr>
			<td>value</td>
			<td>指定选项的值为选项对象的某个属性值</td>
			<td>string</td>
			<td>——</td>
			<td>——</td>
		</tr>
		<tr>
			<td>children</td>
			<td>指定选项的子选项为选项对象的某个属性值</td>
			<td>string</td>
			<td>——</td>
			<td>——</td>
		</tr>
		<tr>
			<td>disabled</td>
			<td>指定选项的禁用为选项对象的某个属性值</td>
			<td>string</td>
			<td>——</td>
			<td>——</td>
		</tr>
	</tbody>
</table>

## Events

<table width="100%" cellspacing="0" cellpadding="0" border="1" style="border-collapse:collapse;display:table;">
	<thead>
		<tr>
			<th>事件名称</th>
			<th>说明</th>
			<th>回调参数</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>change</td>
			<td>当绑定值变化时触发的事件</td>
			<td>当前值</td>
		</tr>
		<tr>
			<td>active-item-change	</td>
			<td>当父级选项变化时触发的事件，仅在 change-on-select 为 false 时可用</td>
			<td>各父级选项组成的数组</td>
		</tr>
	</tbody>
</table>

## Bug & Tip

- tip: value不论是一级还是多级都是array类型。
- tip: props中的disabled不可配置。
- tip: 级联选择器的弹出层不是全局弹出层，若对兄弟元素设置了层级可能会覆盖弹出层。
- tip: 级联选择器设置了固定宽，若要改变可自行设置。