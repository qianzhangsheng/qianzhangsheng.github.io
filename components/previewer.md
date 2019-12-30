# 图片放大器

因为业务需要，我们新增了图片放大器，使用非常简单，请参考以下内容。

!> alexeistukov >= 1.0.0

## 用法

参考示例：

~~~javascript
// 加载插件
Vue.install(alexeistukov.previewer);

// 使用
<i-button @click="handleClick">Preview images</i-button>

methods: {
    handleClick () {
        this.$preview(
            [
                'http://pic41.nipic.com/20140529/18243620_101015342117_2.gif#1',
                'http://img.pconline.com.cn/images/upload/upc/tx/itbbs/1312/01/c4/29178321_1385862629022_mthumb.jpg#1',
                'https://farm4.staticflickr.com/3902/14985871946_24f47d4b53_h.jpg',
                // 'https://farm4.staticflickr.com/3920/15008465772_d50c8f0531_h.jpg'
            ]
        )
    }
}

// 方法签名
Vue.prototype.preview = (images, defaultIndex = 0) => void

images: Array | Object {
    images: Array,
    defaultIndex: Int
}
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
			<td>preview</td>
			<td>预览，被代理至 vue 实例的 $preview 方法，具体调用方法看示例</td>
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
			<td>images</td>
			<td>Object | Array&lt;String&gt;</td>
            <td>-</td>
			<td>图片数组或参数</td>
		</tr>
        <tr>
			<td>defaultIndex</td>
			<td>Int</td>
			<td>-</td>
			<td>-</td>
		</tr>
	</tbody>
</table>
