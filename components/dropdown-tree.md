# 下拉树组件

因为业务需要，我们新增了下拉树组件，请参考以下内容。

!> alexeistukov-ui >= 1.0.0

## 用法

参考示例：

~~~javascript
<i-dropdown-tree
    ref="tree"
    size="small"
    placeholder="请输入占位字符"
    :title="treeTitle"
    :data="treeData"
	:dict="treeDict"
	:multiple="true"
    :showCounter="false"
    :checkChildren="false"
    :checkedNodes="checkedNodesOnInit"
    @change="handleChange">
</i-dropdown-tree>

data() {
    return {
        treeTitle: 'title',
        treeDict: {
            label: 'text',
            childNodes: 'children'
        },
        treeData: [
            {
                id: 1,
                text: 'test-node-1',
                children: [
                    {
                        id: 1-1,
                        text: 'test-node-1',
                        children: []
                    }
                ]
            },
            {
                id: 2,
                text: 'test-node-1',
                children: []
            }
        ],
        checkedNodesOnInit: [1]
    }
}

methods: {
    handleChange (checkedNodes) {
        console.log('选中的节点：', checkedNodes);
    },
    cleanTree() {
        this.tree.clean();
    },
    selectTreeAllNodes() {
        this.tree.selectAll();
    }
}

mounted() {
    this.tree = this.$refs.tree; // 获取树实例
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
			<td>size</td>
			<td>dropdown 的尺寸</td>
			<td>string</td>
			<td>——</td>
			<td>--</td>
		</tr>
        <tr>
			<td>placeholder</td>
			<td>输入框的占位符</td>
			<td>string</td>
			<td>——</td>
			<td>——</td>
		</tr>
        <tr>
			<td>title</td>
			<td>输入框显示的信息</td>
			<td>string</td>
			<td>——</td>
			<td>选中节点标签名</td>
		</tr>
        <tr>
			<td>data</td>
			<td>传入 Tree 组件的数据</td>
			<td>Array</td>
			<td>——</td>
			<td>——</td>
		</tr>
        <tr>
			<td>dict</td>
			<td>树组件属性映射字典</td>
			<td>Object</td>
			<td>——</td>
			<td>——</td>
		</tr>
		<tr>
			<td>multiple</td>
			<td>多选/单选</td>
			<td>Boolean</td>
			<td>true/false</td>
			<td>true</td>
		</tr>
        <tr>
			<td>showCounter</td>
			<td>是否显示计数</td>
			<td>Boolean</td>
			<td>true/false</td>
			<td>false</td>
		</tr>
        <tr>
			<td>checkChildren</td>
			<td>父子节点关联，即勾选父节点时默认勾选所有子节点</td>
			<td>Boolean</td>
			<td>true/false</td>
			<td>true</td>
		</tr>
        <tr>
			<td>checkedNodes</td>
			<td>默认勾选的节点</td>
			<td>Array</td>
			<td>-</td>
			<td>-</td>
		</tr>
	</tbody>
</table>

## Events

<table width="100%" cellspacing="0" cellpadding="0" border="1" style="border-collapse: collapse;display: table;text-align: center;">
	<thead>
		<tr>
			<th>事件</th>
			<th>说明</th>
			<th>回调参数</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>change</td>
			<td>-</td>
			<td>选中的树节点</td>
		</tr>
	</tbody>
</table>
