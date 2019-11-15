# alexeistukov-template

> 内部系统脚手架

## 路由

我们有时候会用到路由参数的情况，比如 `/path/to/:id`, 这里的 id 就是路由参数。

所以，当我们发现多个页面的逻辑和界面大多相同时，我们会想利用它来达到模拟切换页面的效果以复用我们的逻辑和界面。但是我们往往会疏忽了页面状态的问题，我们需要隔离并且保留每个页面的状态。我们利用路由参数来达到复用的目的同时，却需要额外编写大量的逻辑来完成对页面状态的隔离。并且，因为我们需要在关闭一个 tab 时，同时销毁这个页面的实例，但是我们如果通过路由参数来实现页面切换，那么我们多个页面使用的都是同一个实例，所以这个需求必然做不到。

所以，我们做出规定，**禁止使用路由参数来实现页面的切换**，一定要记住，**每个页面必须是一个独立的实例，但是我们依然可以利用路由参数来实现其他功能**

所以，当我们发现多个页面的逻辑和界面大多相同时，我们该如何去复用他？

可以参考以下示例：

~~~javascript
// 创建组件容器
const createCpt = (component, name) => {
    return Object.assign({}, component, {name});
};

let routes = [
    { path: '/', redirect: '/system/dashboard' },
    { path: '/auth/:token', component: Auth },
    { path: '/system', component: Portal, children: [
        { path: 'dashboard', component: Dashboard },
        { path: 'coupons', component: CouponsComponent },
        { path: 'cancel', component: CancelComponent },
        // 我们利用 createCpt 方法来，指定每个页面的名字（必须）与父组件
        // 并且，我们通过分开写每个路径配置，来代替使用路由参数
        { path: 'channel/register', component: createCpt(ChannelComponent, 'channel-register') },
        { path: 'channel/invite', component: createCpt(ChannelComponent, 'channel-invite') },
        { path: 'channel/exchange', component: createCpt(ChannelComponent, 'channel-exchange') },
    ] }
];
~~~
