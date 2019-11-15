# Pan🍳

> Simple CLI for scaffolding Vue.js projects

## 核心

简单易用当然是我的目标之一，准确来说，我希望给开发者们提供一个“无压”的开发环境。
每个人的精力都是有限的，因为有限，我们就需要将注意力集中在项目的核心 -- 代码上。但是现实的开发环境并不是如此，我们时常被代码之外的内容夺去注意力，甚至花上更多的精力去处理这些并不能让代码变得更好的东西上。所以我选择将代码之外的这部分内容剥离。

## 介绍

现在来简单介绍下项目的构成，它主要由这几部分内容构成：

- 项目搭建
- 开发环境
- 项目打包
- 内容更新
- 代码校验

分别对应着五个命令：

- tq init
- tq server
- tq build
- tq update
- tq lint

现在来介绍下这些命令的用法。

> alexeistukov-cli 下载以后，需要到 alexeistukov-cli 的 node_modules 里把 webpack-hot-middleware 降级为 2.22.2

## 用法

### init

~~~bash
tq init
~~~

### server

~~~bash
tq server

# 指定端口
tq server [-p|--port <port>]
~~~

### build

~~~bash
tq build

# 不压缩
tq build [-C|--no-compress]
# 压缩后删除 dist
tq build [-d|--delete]
~~~

### update

~~~bash
tq update

# 更新模板，alexeistukov-ui 和 alexeistukov-cli
tq update [-a|--all]
~~~

### lint

~~~bash
tq lint

# 自动修复
tq lint [-f|--fix]
~~~

## .tofurc 配置选项说明

<table width="100%" cellspacing="0" cellpadding="0" border="1" style="border-collapse: collapse;display: table;text-align: center;">
	<thead>
		<tr>
			<th>参数</th>
			<th>说明</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>href</td>
			<td>hostname</td>
		</tr>
        <tr>
			<td>port</td>
			<td>端口</td>
		</tr>
        <tr>
			<td>proxy</td>
			<td>代理配置</td>
		</tr>
        <tr>
			<td>rules</td>
			<td>Eslint 的规则配置</td>
		</tr>
        <tr>
			<td>webpack</td>
			<td>用来覆盖基础配置</td>
		</tr>
        <tr>
			<td>updateList</td>
			<td>配置需要更新的文件</td>
		</tr>
        <tr>
			<td>_meta</td>
			<td>元信息</td>
		</tr>
	</tbody>
</table>
