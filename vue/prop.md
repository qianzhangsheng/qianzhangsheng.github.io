# Vue 组件数据通信方案总结

<a name="8e1b944f"></a>
### 背景

初识 Vue.js ，了解到组件是 Vue 的主要构成部分，但组件内部的作用域是相对独立的部分，组件之间的关系一般如下图：

![](/1.jpeg)

组件 A 与组件 B 、C 之间是父子组件，组件 B 、C 之间是兄弟组件，而组件 A 、D 之间是隔代的关系。

那么对于这些不同的关系，本文主要分享了他们之间可以采用的几种数据通信方式，例如 Props 、$emit / $on 、Vuex 等，大家可以根据自己的使用场景可以选择适合的使用方式。

<a name="5d782b8f"></a>
### 一、 Prop / $emit

1、**Prop 是你可以在组件上注册的一些自定义特性。当一个值传递给一个 Prop 特性的时候，它就变成了那个组件实例的一个属性**。父组件向子组件传值，通过绑定属性来向子组件传入数据，子组件通过 Props 属性获取对应数据

```html
// 父组件
<template>
  <div class="container">
    <child :title="title"></child>
  </div>
</template>

<script>
import Child from "./component/child.vue";
export default {
  name: "demo",
  data: function() {
    return {
      title: "我是父组件给的"
    };
  },
  components: {
    Child
  },
};
</script>
```

```html
// 子组件
<template>
  <div class="text">{{title}}</div>
</template>

<script>
export default {
  name: 'demo',
  data: function() {},
  props: {
    title: {
      type: String
    }
  },
};
</script>
```

2、$emit 子组件向父组件传值（通过事件形式），子组件通过 $emit 事件向父组件发送消息，将自己的数据传递给父组件。

![](/2.jpeg)

```html
// 父组件
<template>
  <div class="container">
    <div class="title">{{title}}</div>
    <child @changeTitle="parentTitle"></child>
  </div>
</template>

<script>
import Child from "./component/child.vue";

export default {
  name: "demo",
  data: function() {
    return {
      title: null
    };
  },
  components: {
    Child
  },
  methods: {
    parentTitle(e) {
      this.title = e;
    }
  }
};
</script>
```

```html
// 子组件
<template>
  <div class="center">
    <button @click="childTitle">我给父组件赋值</button>
  </div>
</template>

<script>
export default {
  name: 'demo',
  data() {
    return {
      key: 1
    };
  },
  methods: {
    childTitle() {
      this.$emit('changeTitle', `我给父组件的第${this.key}次`);
      this.key++;
    }
  }
};
</script>
```

**小总结：常用的数据传输方式，父子间传递。**

<a name="31b338bc"></a>
### 二、 $emit / $on

这个方法是通过创建了一个空的 vue 实例，当做 $emit 事件的处理中心（事件总线），通过他来触发以及监听事件，方便的实现了任意组件间的通信，包含父子，兄弟，隔代组件。

![](/3.gif)

```html
// 父组件
<template>
  <div class="container">
    <child1 :Event="Event"></child1>
    <child2 :Event="Event"></child2>
    <child3 :Event="Event"></child3>
  </div>
</template>

<script>
import Vue from "vue";

import Child1 from "./component/child1.vue";
import Child2 from "./component/child2.vue";
import Child3 from "./component/child3.vue";

const Event = new Vue();

export default {
  name: "demo",
  data: function() {
    return {
      Event: Event
    };
  },
  components: {
    Child1,
    Child2,
    Child3
  },
};
</script>
```

```html
// 子组件1
<template>
  <div class="center">
    1.我的名字是：{{name}}
    <button @click="send">我给3组件赋值</button>
  </div>
</template>

<script>
export default {
  name: "demo1",
  data() {
    return {
      name: "政采云"
    };
  },
  props: {
    Event
  },
  methods: {
    send() {
      this.Event.$emit("message-a", this.name);
    }
  }
};
</script>
```

```html
// 子组件2
<template>
  <div class="center">
    2.我的年龄是：{{age}}岁
    <button @click="send">我给3组件赋值</button>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  name: "demo2",
  data() {
    return {
      age: "3"
    };
  },
  props: {
    Event
  },
  methods: {
    send() {
      this.Event.$emit("message-b", this.age);
    }
  }
};
</script>
```

```html
// 子组件3
<template>
  <div class="center">我的名字是{{name}}，今年{{age}}岁</div>
</template>

<script>
export default {
  name: 'demo3',
  data() {
    return {
      name: '',
      age: ''
    };
  },
  props: {
    Event
  },
  mounted() {
    this.Event.$on('message-a', name => {
      this.name = name;
    });
    this.Event.$on('message-b', age => {
      this.age = age;
    });
  },
};
</script>
```

**小总结：巧妙的在父子，兄弟，隔代组件中都可以互相数据通信。**

<a name="2e30002f"></a>
### 三、 Vuex

[Vuex](http://vuex.vuejs.org/zh/) **是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。**

![](/4.png)

Vuex实现了一个单项数据流，通过创建一个全局的 State 数据，组件想要修改 State 数据只能通过 Mutation 来进行，例如页面上的操作想要修改 State 数据时，需要通过 Dispatch (触发 Action )，而 Action 也不能直接操作数据，还需要通过 Mutation 来修改 State 中数据，最后根据 State 中数据的变化，来渲染页面。

![](/5.gif)

```javascript
// index.js
import Vue from 'vue';
import Tpl from './index.vue';
import store from './store';

new Vue({
  store,
  render: h => h(Tpl),
}).$mount('#app');
```

```javascript
// store
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment(state) {
      state.count++;
    },
    reduce(state) {
      state.count--;
    }
  },
  actions: {
    actIncrement({ commit }) {
      commit('increment');
    },
    actReduce({ commit }) {
      commit('reduce');
    }
  },
  getters: {
    doubleCount: state => state.count*2
  }
});

export default store;
```

```javascript
// vue文件
<template>
  <div class="container">
    <p>我的count：{{count}}</p>
    <p>doubleCount:{{doubleCount}}</p>
    <button @click="this.actIncrement">增加</button>
    <button @click="this.actReduce">减少</button>
  </div>
</template>

<script>
import { mapGetters, mapActions, mapState } from "vuex";

export default {
  name: "demo",
  data: function() {
    return {};
  },
  components: {},
  props: {},
  computed: {
    ...mapState(["count"]),
    ...mapGetters(["doubleCount"])
  },
  methods: {
    ...mapActions(["actIncrement", "actReduce"])
  }
};
</script>
```

Vuex 中需要注意的点：

Mutation ：是修改State数据的唯一推荐方法，且只能进行同步操作。

Getter ：Vuex 允许在Store中定义 "Getter"（类似于 Store 的计算属性）。Getter 的返回值会根据他的依赖进行缓存，只有依赖值发生了变化，才会重新计算。

本段只是简单介绍了一下 Vuex 的运行方式，更多功能例如 Module 模块请参考[官网](http://vuex.vuejs.org/zh/) 。

**小总结：统一的维护了一份共同的 State 数据，方便组件间共同调用。**

<a name="83ffaab6"></a>
### 四、 $attrs / $listeners

Vue 组件间传输数据在 Vue2.4 版本后有了新方法。除了 Props 外，还有了 $attrs / $listeners。

- $attrs：**包含了父作用域中不作为 Prop 被识别 (且获取) 的特性绑定 (`Class` 和 `Style` 除外)。当一个组件没有声明任何 Prop 时，这里会包含所有父作用域的绑定 (`Class` 和 `Style` 除外)，并且可以通过 `v-bind="$attrs"` 传入内部组件——在创建高级别的组件时非常有用。**
- $listeners：**包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件**

下面来看个例子

![](/6.gif)

```javascript
// 父组件
<template>
  <div class="container">
    <button style="backgroundColor:lightgray" @click="reduce">减dd</button>
    <child1 :aa="aa" :bb="bb" :cc="cc" :dd="dd" @reduce="reduce"></child1>
  </div>
</template>

<script>
import Child1 from './component/child1.vue';
export default {
  name: 'demo',
  data: function() {
    return {
      aa: 1,
      bb: 2,
      cc: 3,
      dd: 100
    };
  },
  components: {
    Child1
  },
  methods: {
    reduce() {
      this.dd--;
    }
  }
};
</script>
```

```javascript
// 子组件1
<template>
  <div>
    <div class="center">
      <p>aa:{{aa}}</p>
      <p>child1的$attrs:{{$attrs}}</p>
      <button @click="this.reduce1">组件1减dd</button>
    </div>
    <child2 v-bind="$attrs" v-on="$listeners"></child2>
  </div>
</template>

<script>
import child2 from './child2.vue';
export default {
  name: 'demo1',
  data() {
    return {};
  },
  props: {
    aa: Number
  },
  components: {
    child2
  },
  methods: {
    reduce1() {
      this.$emit('reduce');
    }
  }
};
</script>
```

```javascript
// 子组件2
<template>
  <div>
    <div class="center">
      <p>bb:{{bb}}</p>
      <p>child2的$attrs:{{$attrs}}</p>
      <button @click="this.reduce2">组件2减dd</button>
    </div>
    <child3 v-bind="$attrs"></child3>
  </div>
</template>

<script>
import child3 from './child3.vue';
export default {
  name: 'demo1',
  data() {
    return {};
  },
  props: {
    bb: Number
  },
  components: {
    child3
  },
  methods: {
    reduce2() {
      this.$emit('reduce');
    }
  }
};
</script>
```

```javascript
// 子组件3
<template>
  <div class="center">
    <p>child3的$attrs:{{$attrs}}</p>
  </div>
</template>

<script>
export default {
  name: 'demo3',
  data() {
    return {};
  },
  props: {
    dd: String
  },
};
</script>
```

简单来说，$attrs 里存放的是父组件中绑定的非 props 属性，$listeners 里面存放的是父组件中绑定的非原生事件。

**小总结：当传输数据、方法较多时，无需一一填写的小技巧。**

<a name="b8a7b9a1"></a>
### 五、 Provider / Inject

Vue2.2 版本以后新增了这两个 API ，**这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。**简单来说，就是父组件通过 Provider 传入变量，任意子孙组件通过 Inject 来拿到变量。

```javascript
// 父组件
<template>
  <div class="container">
    <button @click="this.changeName">我要改名字了</button>
    <p>我的名字：{{name}}</p>
    <child1></child1>
  </div>
</template>

<script>
import Child1 from './component/child1.vue';
export default {
  name: 'demo',
  data: function() {
    return {
      name: '政采云'
    };
  },
  // provide() {
  //   return {
  //     name: this.name //这种绑定方式是不可响应的
  //   };
  // },
  provide() {
    return {
      obj: this
    };
  },
  components: {
    Child1
  },
  methods: {
    changeName() {
      this.name = '政采云前端';
    }
  }
};
</script>
```

```javascript
// 子组件
<template>
  <div>
    <div class="center">
      <!-- <p>子组件名字:{{name}}</p> -->
      <p>子组件名字:{{this.obj.name}}</p>
    </div>
    <child2></child2>
  </div>
</template>

<script>
import child2 from './child2.vue';

export default {
  name: 'demo1',
  data() {
    return {};
  },
  props: {},
  // inject: ["name"],
  inject: {
    obj: {
      default: () => {
        return {};
      }
    }
  },
  components: {
    child2
  },
};
</script>
```

需要注意的是：**Provide 和 Inject 绑定并不是可响应的。这是刻意为之的。然而，如果你传入了一个可监听的对象，那么其对象的属性还是可响应的。**

所以，如果采用的是我代码中注释的方式，父级的 name 如果改变了，子组件this.name 是不会改变的，仍然是 **政采云**，而当采用代码中传入一个监听对象，修改对象中属性值，是可以监听到修改的。

Provider / Inject 在项目中需要有较多公共传参时使用还是颇为方便的。

**小总结：传输数据父级一次注入，子孙组件一起共享的方式。**

<a name="7c51dd66"></a>
### 六、 $parent / $children & $refs

- $parent / $children：**指定已创建的实例之父实例，在两者之间建立父子关系。子实例可以用 `this.$parent` 访问父实例，子实例被推入父实例的 `$children` 数组中。**
- $refs：**一个对象，持有注册过 [`ref` 特性](https://cn.vuejs.org/v2/api/index.html?_sw-precache=a7a4d39c5138496b644f27256d087649#ref) 的所有 DOM 元素和组件实例。ref 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 `$refs` 对象上。如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件。**

```html
// 父组件
<template>
  <div class="container">
    <p>我的title：{{title}}</p>
    <p>我的name：{{name}}</p>
    <child1 ref="comp1"></child1>
    <child2 ref="comp2"></child2>
  </div>
</template>

<script>
import Child1 from './component/child1.vue';
import Child2 from './component/child2.vue';
export default {
  name: 'demo',
  data: function() {
    return {
      title: null,
      name: null,
      content: '就是我'
    };
  },
  components: {
    Child1,
    Child2
  },
  mounted() {
    const comp1 = this.$refs.comp1;
    this.title = comp1.title;
    comp1.sayHello();
    this.name = this.$children[1].title;
  },
};
</script>
```

```html
// 子组件1-ref方式
<template>
  <div>
    <div class="center">我的父组件是谁:{{content}}</div>
  </div>
</template>

<script>
export default {
  name: 'demo1',
  data() {
    return {
      title: '我是子组件',
      content: null
    };
  },
  mounted() {
    this.content = this.$parent.content;
  },
  methods: {
    sayHello() {
      window.alert('Hello');
    }
  }
};
</script>
```

```html
// 子组件2-children方式
<template>
  <div>
    <div class="center"></div>
  </div>
</template>

<script>
export default {
  name: 'demo1',
  data() {
    return {
      title: '我是子组件2'
    };
  },
};
</script>
```

通过例子可以看到这两种方式都可以父子间通信，而缺点也很统一，就是都不能跨级以及兄弟间通信。

**小总结：父子组件间共享数据以及方法的便捷实践之一。**

<a name="25f9c7fa"></a>
### 总结

组件间不同的使用场景可以分为 3 类，对应的通信方式如下：

- 父子通信：Props / $emit，$emit / $on，Vuex，$attrs / $listeners，provide/inject，$parent / $children＆$refs
- 兄弟通信：$emit / $on，Vuex
- 隔代（跨级）通信：$emit / $on，Vuex，provide / inject，$attrs / $listeners

大家可以根据自己的使用场景选择不同的通信方式，当然还是都自己写写代码，试验一把来的印象深刻喽。
