---
title: 前端框架及项目面试(聚焦Vue-React-Webpack)
date: 2021-07-01 14:54:38
tags:
	- 笔试面试
---





## 导学

### 前端面试常见流程

![image-20210701145911454](http://blog.cdn.ionluo.cn/blog/image-20210701145911454.png)

### 思维导图和课程资料

https://blog.cdn.ionluo.cn/files/frame-project-interview-master.zip



### 注意事项

- 关于Vue的基础使用部分，推荐官方文档，这里只是罗列了一些基础的部分，想要学好还是看官方文档。https://cn.vuejs.org/v2/guide/
- 关于React的基础，官方文档虽然没有Vue那么清晰易懂，但是也是很完善的，也是推荐用官网文档来进行学习。https://reactjs.bootcss.com/

![image-20210701151037533](http://blog.cdn.ionluo.cn/blog/image-20210701151037533.png)



## Vue使用

### 说明

```json
// package.json
{
  "dependencies": {
    "vue": "^2.6.10"
  }
}
```



### [Computed](https://cn.vuejs.org/v2/guide/computed.html#%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7)

```vue
<template>
    <div>
        <p>num {{num}}</p>
        <p>double1 {{double1}}</p>
        <input v-model="double2"/>
    </div>
</template>

<script>
export default {
    data() {
        return {
            num: 20
        }
    },
    computed: {
        double1() {
            return this.num * 2
        },
        // 默认写法是getter， 像setter也可以举例子是fullName = firstName +' ' + lastName为例子
        double2: {
            // getter
            get() {
                return this.num * 2
            },
            // setter
            set(val) {
                this.num = val/2
            }
        }
    }
}
</script>
```

### [Watch](https://cn.vuejs.org/v2/guide/computed.html#%E4%BE%A6%E5%90%AC%E5%99%A8)

```vue
<template>
    <div>
        <input v-model="name"/>
        <input v-model="info.city"/>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: '双越',
            info: {
                city: '北京'
            }
        }
    },
    watch: {
        name(oldVal, val) {
            console.log('watch name', oldVal, val) // 值类型，可正常拿到 oldVal 和 val
        },
        info: {
            handler(oldVal, val) {
                console.log('watch info', oldVal, val) // 引用类型，拿不到 oldVal 。因为指针相同，此时已经指向了新的 val
            },
            deep: true // 深度监听
        }
    }
}
</script>
```

> **扩展：computed 和 watch 的区别和运用的场景？**
>
> **computed：** 是计算属性，依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed  的值；
>
> **watch：** 更多的是「观察」的作用，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作；
>
> **运用场景：**
>
> - 当我们需要进行数值计算，并且依赖于其它数据时，应该使用 computed，因为可以利用 computed 的缓存特性，避免每次获取值时，都要重新计算；
> - 当我们需要在数据变化时执行异步或开销较大的操作时，应该使用 watch，使用 watch 选项允许我们执行异步操作 ( 访问一个 API )，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。
>
> 关于这个问题，也可以看看这篇文章：https://blog.csdn.net/weixin_39015132/article/details/83310726

### [Class和Style](https://cn.vuejs.org/v2/guide/class-and-style.html)

```vue
<template>
    <div>
        <p :class="{ black: isBlack, yellow: isYellow }">使用 class</p>
        <p :class="[black, yellow]">使用 class （数组）</p>
        <p :style="styleData">使用 style</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            isBlack: true,
            isYellow: true,

            black: 'black',
            yellow: 'yellow',

            styleData: {
                fontSize: '40px', // 转换为驼峰式
                color: 'red',
                backgroundColor: '#ccc' // 转换为驼峰式
            }
        }
    }
}
</script>

<style scoped>
    .black {
        background-color: #999;
    }
    .yellow {
        color: yellow;
    }
</style>
```

### [v-if和v-show](https://cn.vuejs.org/v2/guide/conditional.html)

```vue
<template>
    <div>
        <p v-if="type === 'a'">A</p>
        <p v-else-if="type === 'b'">B</p>
        <p v-else>other</p>

        <p v-show="type === 'a'">A by v-show</p>
        <p v-show="type === 'b'">B by v-show</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            type: 'a'
        }
    }
}
</script>
```

### [v-for](https://cn.vuejs.org/v2/guide/list.html)

```vue
<template>
    <div>
        <p>遍历数组</p>
        <ul>
            <li v-for="(item, index) in listArr" :key="item.id">
                {{index}} - {{item.id}} - {{item.title}}
            </li>
        </ul>

        <p>遍历对象</p>
        <ul >
            <li v-for="(val, key, index) in listObj" :key="key">
                {{index}} - {{key}} -  {{val.title}}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            flag: false,
            listArr: [
                { id: 'a', title: '标题1' }, // 数据结构中，最好有 id ，方便使用 key
                { id: 'b', title: '标题2' },
                { id: 'c', title: '标题3' }
            ],
            listObj: {
                a: { title: '标题1' },
                b: { title: '标题2' },
                c: { title: '标题3' },
            }
        }
    }
}
</script>
```

> v-for的优先级比v-if高，但是不建议两者一起使用, 可以利用计算属性的方式生成需要的列表。

### [v-on](https://cn.vuejs.org/v2/guide/events.html)

```vue
<template>
    <div>
        <p>{{num}}</p>
        <button @click="increment1">+1</button>
        <button @click="increment2(2, $event)">+2</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            num: 0
        }
    },
    methods: {
        increment1(event) {
            // eslint-disable-next-line
            console.log('event', event, event.__proto__.constructor) // 是原生的 event 对象
            // eslint-disable-next-line
            console.log(event.target)
            // eslint-disable-next-line
            console.log(event.currentTarget) // 注意，事件是被注册到当前元素的，和 React 不一样
            this.num++

            // 1. event 是原生的
            // 2. 事件被挂载到当前元素
            // 和 DOM 事件一样
        },
        increment2(val, event) {
            // eslint-disable-next-line
            console.log(event.target)
            this.num = this.num + val
        },
        loadHandler() {
            // do some thing
        }
    },
    mounted() {
        window.addEventListener('load', this.loadHandler)
    },
    beforeDestroy() {
        //【注意】用 vue 绑定的事件，组建销毁时会自动被解绑
        // 自己绑定的事件，需要自己销毁！！！
        window.removeEventListener('load', this.loadHandler)
    }
}
</script>
```



![image-20210701222850191](http://blog.cdn.ionluo.cn/blog/image-20210701222850191.png)



> 上面漏了一个：
>
> ```html
> <!-- 点击事件只会触发一次 -->
> <a @click.once="doThis"></a>
> ```

![image-20210701222912385](http://blog.cdn.ionluo.cn/blog/image-20210701222912385.png)

> 通用法：
>
> ```html
> <!-- 只有在keyCode 是 13 时调用 vm.submit() -->
> <!-- 等于写法： <input @keyup.enter="submit"> -->
> <input @keyup.13="submit">
> ```
>
> 记住所有的keyCode比较困难，所以Vue为最常用的按键提供了别名：
>
> ```
> .enter
> .tab
> .delete
> .esc
> .space
> .up
> .down
> .left
> .right
> .ctrl
> .alt
> .shift
> .meta
> <!-- 以下为鼠标按键修饰符 -->
> .left
> .right
> .midddle
> ```
>
> 可以通过全局config.keyCodes对象自定义键值修饰符别名：
>
> ```javascript
> // 可以通过 @keyup.f1 使用
> Vue.config.keyCodes.f1 = 11we2
> ```



### [v-model](https://cn.vuejs.org/v2/guide/forms.html)

```vue
<template>
    <div>
        <p>输入框: {{name}}</p>
        <input type="text" v-model.trim="name"/>
        <input type="text" v-model.lazy="name"/>
        <input type="text" v-model.number="age"/>

        <p>多行文本: {{desc}}</p>
        <textarea v-model="desc"></textarea>
        <!-- 注意，<textarea>{{desc}}</textarea> 是不允许的！！！ -->

        <p>复选框 {{checked}}</p>
        <input type="checkbox" v-model="checked"/>

        <p>多个复选框 {{checkedNames}}</p>
        <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
        <label for="jack">Jack</label>
        <input type="checkbox" id="john" value="John" v-model="checkedNames">
        <label for="john">John</label>
        <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
        <label for="mike">Mike</label>

        <p>单选 {{gender}}</p>
        <input type="radio" id="male" value="male" v-model="gender"/>
        <label for="male">男</label>
        <input type="radio" id="female" value="female" v-model="gender"/>
        <label for="female">女</label>

        <p>下拉列表选择 {{selected}}</p>
        <select v-model="selected">
            <option disabled value="">请选择</option>
            <option>A</option>
            <option>B</option>
            <option>C</option>
        </select>

        <p>下拉列表选择（多选） {{selectedList}}</p>
        <select v-model="selectedList" multiple>
            <option disabled value="">请选择</option>
            <option>A</option>
            <option>B</option>
            <option>C</option>
        </select>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: '双越',
            age: 18,
            desc: '自我介绍',

            checked: true,
            checkedNames: [],

            gender: 'male',

            selected: '',
            selectedList: []
        }
    }
}
</script>
```

> **扩展：直接给一个数组项赋值，Vue 能检测到变化吗？**
>
> 由于 JavaScript 的限制，Vue 不能检测到以下数组的变动：
>
> - 当你利用索引直接设置一个数组项时，例如：`vm.items[indexOfItem] = newValue`
> - 当你修改数组的长度时，例如：`vm.items.length = newLength`
>
> 为了解决第一个问题，Vue 提供了以下操作方法：
>
> ```javascript
> // Vue.set
> Vue.set(vm.items, indexOfItem, newValue)
> // vm.$set，Vue.set的一个别名
> vm.$set(vm.items, indexOfItem, newValue)
> // Array.prototype.splice
> vm.items.splice(indexOfItem, 1, newValue)
> ```
>
> 为了解决第二个问题，Vue 提供了以下操作方法：
>
> ```javascript
> // Array.prototype.splice
> vm.items.splice(newLength)
> ```
>
>
> **扩展：对象属性添加和删除，Vue 能检测到变化吗？**
>
> 还是由于JavaScript的限制，Vue不能检测对象属性的添加和删除：
>
> ```javascript
> // a的变化可以检测到
> var vm = new Vue({
>     data: {
>         a: 1
>     }
> })
> // b的变化检测不到
> vm.b = 2
> ```
> 对于已经创建的实例，Vue不能动态添加根级别的响应式属性。但是，可以使用 Vue.set(object, key, value) 方法向嵌套对象添加响应式属性。例如，对于：
>
> ```javascript
> var vm = new Vue({
>     data: {
>         message: {
>             text1: 'hello'
>         }
>     }
> })
> ```
>
> 你可以添加一个新的text2属性到message对象：
>
> ```javascript
> Vue.set(vm.message, 'text2', 'world')
> ```
>
> 你还可以使用vm.$set实例方法，它只是全局Vue.set的别名：
>
> ```javascript
> this.$set(this.message, 'text2', 'world')
> ```
>
> 有时你可能需要为已有对象赋予多个新属性，比如使用Object.assign()或_.extend()。在这种情况下，你应该用两个对象的属性创建一个新的对象。所以，如果你想添加新的响应式属性，可以这么做：
>
> ```javascript
> this.message = Object.assign({}, this.message, {
>     text2: 'wold',
>     text3: '!'
> })
> ```
>
> 



### [组件通信](https://cn.vuejs.org/v2/guide/components.html)

> 有父子组件通信，子父组件通信，平行组件通信

Index.vue

```vue
<template>
    <div>
        <Input @add="addHandler"/>
        <List :list="list" @delete="deleteHandler"/>
    </div>
</template>

<script>
import Input from './Input'
import List from './List'

export default {
    components: {
        Input,
        List
    },
    data() {
        return {
            list: [
                {
                    id: 'id-1',
                    title: '标题1'
                },
                {
                    id: 'id-2',
                    title: '标题2'
                }
            ]
        }
    },
    methods: {
        addHandler(title) {
            this.list.push({
                id: `id-${Date.now()}`,
                title
            })
        },
        deleteHandler(id) {
            this.list = this.list.filter(item => item.id !== id)
        }
    },
    created() {
        // eslint-disable-next-line
        console.log('index created')
    },
    mounted() {
        // eslint-disable-next-line
        console.log('index mounted')
    },
    beforeUpdate() {
        // eslint-disable-next-line
        console.log('index before update')
    },
    updated() {
        // eslint-disable-next-line
        console.log('index updated')
    },
}
</script>
```

Input.vue

```vue
<template>
    <div>
        <input type="text" v-model="title"/>
        <button @click="addTitle">add</button>
    </div>
</template>

<script>
import event from './event'

export default {
    data() {
        return {
            title: ''
        }
    },
    methods: {
        addTitle() {
            // 调用父组件的事件
            this.$emit('add', this.title)

            // 调用自定义事件
            event.$emit('onAddTitle', this.title)

            this.title = ''
        }
    }
}
</script>
```

List.vue

```vue
<template>
    <div>
        <ul>
            <li v-for="item in list" :key="item.id">
                {{item.title}}

                <button @click="deleteItem(item.id)">删除</button>
            </li>
        </ul>
    </div>
</template>

<script>
import event from './event'

export default {
    // props: ['list']
    props: {
        // prop 类型和默认值
        list: {
            type: Array,
            default() {
                return []
            }
        },
        // 基础类型检测（如果是null，意思是任何类型都可以）
        propA: Number, 
        // 多种类型
        propB: [Number, String],
        // 必传且是字符串
        propC: {
            type: String,
            required: true
        },
        // 数字，有默认值
        propD: {
            type: NUmber,
            default: 100
        },
        // 数组或对象的默认值应当由一个工厂函数返回
        propE: {
            type: Object,
            default: function(){
                return { message: 'hello word!' }
            }
        },
        // 自定义验证函数
        propF: {
            validator: function(value){
                return value > 10
            }
        }
        // type可以是下面原生构造器
        // String, Number, Boolean, Function, Object, Array, Symbol
        // type也可以是一个自定义构造器函数，使用instanceof检测
    },
    data() {
        return {

        }
    },
    methods: {
        deleteItem(id) {
            this.$emit('delete', id)
        },
        addTitleHandler(title) {
            // eslint-disable-next-line
            console.log('on add title', title)
        }
    },
    created() {
        // eslint-disable-next-line
        console.log('list created')
    },
    mounted() {
        // eslint-disable-next-line
        console.log('list mounted')

        // 绑定自定义事件
        event.$on('onAddTitle', this.addTitleHandler)
    },
    beforeUpdate() {
        // eslint-disable-next-line
        console.log('list before update')
    },
    updated() {
        // eslint-disable-next-line
        console.log('list updated')
    },
    beforeDestroy() {
        // 及时销毁，否则可能造成内存泄露
        event.$off('onAddTitle', this.addTitleHandler)
    }
}
</script>
```

event.js

```javascript
import Vue from 'vue'

export default new Vue()
```



> **组件扩展：**
>
> 1. 给组件绑定原生事件（.native）
>
>    ```html
>    <my-component @click.native="doTheThing"></my-component>
>    ```
>
> 2. 自定更新父组件属性（.sync）
>
>    ```html
>    <!-- 有时候，我们希望实现Props的“双向绑定”,可以使用.sync修饰符，它是一个编译时的语法糖 -->
>    <my-component :foo.sync="bar"></my-component>
>    <!-- 编译成： <my-component :foo="bar" @update:foo="value => bar = value"></my-component> -->
>    <!-- 子组件需要更新foo时，显示触发一个更新事件 -->
>    this.$emit('update:foo', newValue)
>    ```
>
> 3. 组件命名约定
>
>    ```vue
>    component: {
>        // 使用 kebab-case 形式注册
>        'kebab-cased-component': {},
>        // register using camelCase
>        'camelCasedComponent': {},
>        // register using PascalCase
>        'PascalCaseComponent': {},
>    }
>    // 在HTML模板中，请使用kebab-case形式
>    <kebab-cased-component></kebab-cased-component>
>    <camel-cased-component></camel-cased-component>
>    <pascal-case-component></pascal-case-component>
>    ```
>
> 4. 递归组件
>
>    参考推荐阅读中的[Vue: export default中的name属性到底有啥作用呢？](https://blog.csdn.net/weixin_39015132/article/details/83573896)
>
>    



### [组件生命周期](https://cn.vuejs.org/v2/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA)

1. **谈谈你对 Vue 生命周期的理解？**

   **（1）生命周期是什么？**

   Vue 实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模版、挂载 Dom -> 渲染、更新 -> 渲染、卸载等一系列过程，我们称这是 Vue 的生命周期。

   **（2）各个生命周期的作用**

| 生命周期      | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| beforeCreate  | 组件实例被创建之初，组件的属性生效之前                       |
| created       | 组件实例已经完全创建，属性也绑定，但真实 dom 还没有生成，$el 还不可用 |
| beforeMount   | 在挂载开始之前被调用：相关的 render 函数首次被调用           |
| mounted       | el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子    |
| beforeUpdate  | 组件数据更新之前调用，发生在虚拟 DOM 打补丁之前              |
| update        | 组件数据更新之后                                             |
| activited     | keep-alive 专属，组件被激活时调用                            |
| deactivated   | keep-alive 专属，组件被销毁时调用                            |
| beforeDestory | 组件销毁前调用                                               |
| destoryed     | 组件销毁后调用                                               |

**（3）生命周期示意图**

![1.png](http://blog.cdn.ionluo.cn/blog/16ca74f183827f46)

2. **Vue 的父组件和子组件生命周期钩子函数执行顺序？**

   Vue 的父组件和子组件生命周期钩子函数执行顺序可以归类为以下 4 部分：

- 加载渲染过程

  > 父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted

- 子组件更新过程

  > 父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated

- 父组件更新过程

  > 父 beforeUpdate -> 父 updated

- 销毁过程

  > 父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed

3. **在哪个生命周期内调用异步请求？**

   可以在钩子函数 created、beforeMount、mounted 中进行调用，因为在这三个钩子函数中，data 已经创建，可以将服务端端返回的数据进行赋值。但是本人推荐在 created 钩子函数中调用异步请求，因为在 created 钩子函数中调用异步请求有以下优点：

- 能更快获取到服务端数据，减少页面 loading 时间；
- ssr 不支持 beforeMount 、mounted 钩子函数，所以放在 created 中有助于一致性；

4. **在什么阶段才能访问操作DOM？**

   在钩子函数 mounted 被调用前，Vue 已经将编译好的模板挂载到页面上，所以在 mounted 中可以访问操作 DOM。

5. **父组件可以监听到子组件的生命周期吗？**

   比如有父组件 Parent 和子组件 Child，如果父组件监听到子组件挂载 mounted 就做一些逻辑处理，可以通过以下写法实现：

```javascript
// Parent.vue
<Child @mounted="doSomething"/>
    
// Child.vue
mounted() {
  this.$emit("mounted");
}
```

以上需要手动通过 $emit 触发父组件的事件，更简单的方式可以在父组件引用子组件时通过 @hook 来监听即可，如下所示：

```javascript
//  Parent.vue
<Child @hook:mounted="doSomething" ></Child>

doSomething() {
   console.log('父组件监听到 mounted 钩子函数 ...');
},
    
//  Child.vue
mounted(){
   console.log('子组件触发 mounted 钩子函数 ...');
},    
    
// 以上输出顺序为：
// 子组件触发 mounted 钩子函数 ...
// 父组件监听到 mounted 钩子函数 ...     
```

当然 @hook 方法不仅仅是可以监听 mounted，其它的生命周期事件，例如：created，updated 等都可以监听。



### [自定义组件的v-model](https://cn.vuejs.org/v2/guide/components-custom-events.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6%E7%9A%84-v-model)

Index.vue

```vue
<template>
    <div>
        <p>vue 高级特性</p>
        <hr>

        <!-- 自定义 v-model -->
        <p>{{name}}</p>
        <CustomVModel v-model="name"/>
    </div>
</template>

<script>
import CustomVModel from './CustomVModel'

export default {
    components: {
        CustomVModel
    },
    data() {
        return {
            name: '双越'
        }
    }
}
</script>
```

CustomVModel.vue

```vue
<template>
    <!-- 例如：vue 颜色选择 -->
    <input type="text"
        :value="text1"
        @input="$emit('change1', $event.target.value)"
    >
    <!--
        1. 上面的 input 使用了 :value 而不是 v-model
        2. 上面的 change1 和 model.event 要对应起来
        3. text1 属性对应起来
    -->
</template>

<script>
export default {
    model: {
        prop: 'text1', // 对应 props text1
        event: 'change1'
    },
    props: {
        text1: String,
        default() {
            return ''
        }
    }
}
</script>
```



### [$nextTick](https://cn.vuejs.org/v2/api/index.html#Vue-nextTick)

![image-20210702133332538](http://blog.cdn.ionluo.cn/blog/image-20210702133332538.png)

```vue
<template>
  <div id="app">
    <ul ref="ul1">
        <li v-for="(item, index) in list" :key="index">
            {{item}}
        </li>
    </ul>
    <button @click="addItem">添加一项</button>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
      return {
        list: ['a', 'b', 'c']
      }
  },
  methods: {
    addItem() {
        this.list.push(`${Date.now()}`)
        this.list.push(`${Date.now()}`)
        this.list.push(`${Date.now()}`)

        // 1. 异步渲染，$nextTick 待 DOM 渲染完再回调
        // 3. 页面渲染时会将 data 的修改做整合，多次 data 修改只会渲染一次
        this.$nextTick(() => {
          // 获取 DOM 元素
          const ulElem = this.$refs.ul1
          // eslint-disable-next-line
          console.log( ulElem.childNodes.length )
        })
    }
  }
}
</script>
```

### [slot](https://cn.vuejs.org/v2/guide/components-slots.html)

![image-20210702144002080](http://blog.cdn.ionluo.cn/blog/image-20210702144002080.png)

#### 基本使用

Index.vue

```vue
<template>
    <div>
        <p>vue 高级特性</p>
        <hr>
        <!-- slot -->
        <SlotDemo :url="website.url">
            {{website.title}}
        </SlotDemo>
    </div>
</template>

<script>
import SlotDemo from './SlotDemo'

export default {
    components: {
        SlotDemo
    },
    data() {
        return {
            website: {
                url: 'http://imooc.com/',
                title: 'imooc',
                subTitle: '程序员的梦工厂'
            }
        }
    }
}
</script>
```

SlotDemo.vue

```vue
<template>
    <a :href="url">
        <slot>
            默认内容，即父组件没设置内容时，这里显示
        </slot>
    </a>
</template>

<script>
export default {
    props: ['url'],
    data() {
        return {}
    }
}
</script>
```



#### 作用域插槽

Index.vue

```vue
<template>
    <div>
        <p>vue 高级特性</p>
        <hr>
        <ScopedSlotDemo :url="website.url">
            <!-- 这里通过作用域插槽，拿到了组件里面的作用域（变量website）-->
            <template v-slot="slotProps">
                {{slotProps.slotData.title}}
            </template>
        </ScopedSlotDemo>
    </div>
</template>

<script>
import ScopedSlotDemo from './ScopedSlotDemo'

export default {
    components: {
        ScopedSlotDemo
    },
    data() {
        return {
            website: {
                url: 'http://imooc.com/',
                title: 'imooc',
                subTitle: '程序员的梦工厂'
            }
        }
    }
}
</script>
```

ScopedSlotDemo.vue

```vue
<template>
    <a :href="url">
        <slot :slotData="website">
            {{website.subTitle}} <!-- 默认值显示 subTitle ，即父组件不传内容时 -->
        </slot>
    </a>
</template>

<script>
export default {
    props: ['url'],
    data() {
        return {
            website: {
                url: 'http://wangEditor.com/',
                title: 'wangEditor',
                subTitle: '轻量级富文本编辑器'
            }
        }
    }
}
</script>
```



#### 具名插槽

![image-20210702150727795](http://blog.cdn.ionluo.cn/blog/image-20210702150727795.png)



### [动态组件](https://cn.vuejs.org/v2/guide/components.html#%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6)

有的时候，在不同组件之间进行动态切换是非常有用的，比如在一个多标签的界面里：

![image-20210702153304384](http://blog.cdn.ionluo.cn/blog/image-20210702153304384.png)

```vue
<template>
    <div>
        <p>vue 高级特性</p>
        <hr>

        <!-- 动态组件(组件会在 `NextTickName` 改变时改变) -->
        <component :is="NextTickName"/>
    </div>
</template>

<script>
import NextTick from './NextTick'

export default {
    components: {
        NextTick
    },
    data() {
        return {
            NextTickName: "NextTick"
        }
    }
}
</script>
```

> 动态组件的缓存需要借助keep-alive: https://cn.vuejs.org/v2/guide/components-dynamic-async.html#%E5%9C%A8%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6%E4%B8%8A%E4%BD%BF%E7%94%A8-keep-alive





### [异步组件](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#%E5%BC%82%E6%AD%A5%E7%BB%84%E4%BB%B6)

在大型应用中，我们可能需要将应用分割成小一些的代码块，并且只在需要的时候才从服务器加载一个模块。为了简化，Vue 允许你以一个工厂函数的方式定义你的组件，这个工厂函数会异步解析你的组件定义。Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染。例如：

```javascript
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 向 `resolve` 回调传递组件定义
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```
下面展示Webpack 和 ES2015 语法加在一起（vue-cli），我们可以这样使用动态导入：

```vue
<template>
    <div>
        <p>vue 高级特性</p>
        <hr>
        
        <!-- 异步组件 -->
        <FormDemo v-if="showFormDemo"/>
        <button @click="showFormDemo = true">show form demo</button>
    </div>
</template>

<script>

export default {
    components: {
        // 这里不用关系这个组件是上面，仅做展示
        FormDemo: () => import('../BaseUse/FormDemo'),
    },
    data() {
        return {
            showFormDemo: false
        }
    }
}
</script>
```



### [mixin](https://cn.vuejs.org/v2/guide/mixins.html)

![image-20210702172015545](http://blog.cdn.ionluo.cn/blog/image-20210702172015545.png)

Index.vue

```vue
<template>
    <div>
        <p>{{name}} {{major}} {{city}}</p>
        <button @click="showName">显示姓名</button>
    </div>
</template>

<script>
import myMixin from './mixin'

export default {
    mixins: [myMixin], // 可以添加多个，会自动合并起来
    data() {
        return {
            name: '双越',
            major: 'web 前端'
        }
    },
    methods: {
    },
    mounted() {
        // eslint-disable-next-line
        console.log('component mounted', this.name)
    }
}
</script>
```

mixin.js

```javascript
export default {
    data() {
        return {
            city: '北京'
        }
    },
    methods: {
        showName() {
            // eslint-disable-next-line
            console.log(this.name)
        }
    },
    mounted() {
        // eslint-disable-next-line
        console.log('mixin mounted', this.name)
    }
}
```

**mixin的说明**

- 数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。
- 同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。
- 值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。
- 请谨慎使用全局混入，因为它会影响每个单独创建的 Vue 实例 (包括第三方组件)。大多数情况下，只应当应用于自定义选项，就像上面示例一样。推荐将其作为[插件](https://cn.vuejs.org/v2/guide/plugins.html)发布，以避免重复应用混入。
- 可[自定义选项合并策略](https://cn.vuejs.org/v2/guide/mixins.html#自定义选项合并策略)

**mixin的问题**

- 变量来源不明确，不利于阅读（合理的形式应该像es6的import）
- 多个mixin可能会造成命名冲突（官方推荐使用一定的规范来避免）
- mixin和组件可能出现多对多的关系，复杂度高（要极力避免）



### [Vuex](https://vuex.vuejs.org/zh/guide/)

面试考点并不多，但是基本概念，基本使用和API必须掌握，可能会考察state的数据结构设计。

代码demo见：https://gitee.com/cheerfulion/my_public_demos/tree/master/vuex_demo

![vuex](https://vuex.vuejs.org/vuex.png)

### [Vue-router](https://router.vuejs.org/zh/)

这里的考点我觉得文档上面已经列的很好了，内容也不多，推荐直接点击上面标题阅读源文档。这里就主要讲解几个点吧！

**路由模式**

- hash模式（默认）

- [history模式](https://router.vuejs.org/zh/guide/essentials/history-mode.html)（需要后台支持，文档中给出了各个后端服务器的配置方法，注意该方式需要前端指定404页面）

**动态路由**

![image-20210703102839179](http://blog.cdn.ionluo.cn/blog/image-20210703102839179.png)

| 模式                          | 匹配路径            | $route.params                          |
| ----------------------------- | ------------------- | -------------------------------------- |
| /user/:username               | /user/evan          | `{ username: 'evan' }`                 |
| /user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: '123' }` |
| /user/ion*                    | /user/ionluo        | `{ pathMatch: 'luo' }`g                |

除了 `$route.params` 外，`$route` 对象还提供了其它有用的信息，例如，`$route.query` (如果 URL 中有查询参数)、`$route.hash` 等等。你可以查看 [API 文档](https://router.vuejs.org/zh/api/#路由对象) 的详细说明。

详见官方文档。



**路由懒加载**

其实就是上面的异步组件的webpack实现。

![image-20210703103652695](http://blog.cdn.ionluo.cn/blog/image-20210703103652695.png)



### 推荐阅读

- [Vue基础](https://blog.csdn.net/ion_L/article/details/82691731)

- [Vue: export default中的name属性到底有啥作用呢？](https://blog.csdn.net/weixin_39015132/article/details/83573896)

  

## Vue原理

- 组件化和MVVM

- 响应式原理

- vdom和diff算法

- 模板编译

- 组件渲染过程

- 前端路由


### 关于MVC、MVP和MVVM的理解

![img](http://blog.cdn.ionluo.cn/blog/20181016230357827)


MVC作为经典的框架模式，视图层，数据层以及业务逻辑层都有关联，缺点是数据和视图耦合性高，逻辑层臃肿（Jquery）。

MVP视图和数据都通过P这个中间层交互，导致P层特别臃肿。但是去除了数据和视图的耦合性，维护起来更方便（Django）。

MVVM把VM代替上面的C和P层，直接通过数据驱动渲染视图（Angular， React， Vue）。



### 响应式原理

**核心API** - `Object.defineProperty`

Vue3.0使用`Proxy`实现响应式，因为`Object.defineProperty`具有一些缺点。

> 注意：`Proxy`兼容性不如`Object.defineProperty`，且无法`polyfill`

![image-20210703222522825](http://blog.cdn.ionluo.cn/blog/image-20210703222522825.png)



**Object.defineProperty缺点**

- 深度监听，需要递归到底，一次性计算量大
- 无法监听新增属性/删除属性(所以Vue提供了Vue.set和 Vue.delete方法)
- 无法原生监听数组，需要特殊处理



**下面实现对象和数组的响应式：**

视频讲解见：https://www.bilibili.com/video/BV1VA411x76D

```javascript
// 触发更新视图
function updateView() {
    console.log('视图更新')
}

// 重新定义数组原型
const oldArrayProperty = Array.prototype
// 创建新对象，原型指向 oldArrayProperty ，再扩展新的方法不会影响原型
const arrProto = Object.create(oldArrayProperty);
// 这里是一些常用的数组方法，如果要更多需要添加一下
['push', 'pop', 'shift', 'unshift', 'splice'].forEach(methodName => {
    arrProto[methodName] = function () {
        updateView() // 触发视图更新
        oldArrayProperty[methodName].call(this, ...arguments)
        // Array.prototype.push.call(this, ...arguments)
    }
})

// 重新定义属性，监听起来
function defineReactive(target, key, value) {
    // 深度监听
    observer(value)

    // 核心 API
    Object.defineProperty(target, key, {
        get() {
            return value
        },
        set(newValue) {
            if (newValue !== value) {
                // 深度监听(赋值有可能是对象，需要深度监听)
                observer(newValue)

                // 设置新值
                // 注意，value 一直在闭包中，此处设置完之后，再 get 时也是会获取最新的值
                value = newValue

                // 触发更新视图
                updateView()
            }
        }
    })
}

// 监听对象属性
function observer(target) {
    if (typeof target !== 'object' || target === null) {
        // 不是对象或数组
        return target
    }

    // 污染全局的 Array 原型
    // Array.prototype.push = function () {
    //     updateView()
    //     ...
    // }

    if (Array.isArray(target)) {
        target.__proto__ = arrProto
    }

    // 重新定义各个属性（for in 也可以遍历数组）
    for (let key in target) {
        defineReactive(target, key, target[key])
    }
}

// 准备数据
const data = {
    name: 'zhangsan',
    age: 20,
    info: {
        address: '北京' // 需要深度监听
    },
    nums: [10, 20, 30]
}

// 监听数据
observer(data)

// 测试
data.name = 'lisi'
data.age = 21
// // console.log('age', data.age)
// data.x = '100' // 新增属性，监听不到 —— 所以有 Vue.set
// delete data.name // 删除属性，监听不到 —— 所以有 Vue.delete
data.info.address = '上海' // 深度监听
data.nums[0] = 0 // 监听数组
data.nums.push(4) // 监听数组

```

### 虚拟DOM和diff算法

视频讲解见：https://www.bilibili.com/video/BV1dV411a7mT







- vdom是实现vue和React的重要基石
- diff算法是vdom中最核心、最关键的部分
- vdom是一个热门话题，也是面试中热门话题



**vdom**

- DOM操作非常耗费性能
- 以前用JQuery，可以自行控制DOM操作的时机，手动调整
- vdom用JS模拟DOM结构，计算出最小的变更（diff算法）来操作DOM
- vdom是React提出的，vue2.0后也开始使用。

![image-20210706120827755](http://blog.cdn.ionluo.cn/blog/image-20210706120827755.png)

> 这里的JS模拟在不同的框架中可能不一样的表示，即没有既定的标准， 如`tag`可能有的称为`element`, `style` 可能不放在`props`中。

![image-20210706121127785](http://blog.cdn.ionluo.cn/blog/image-20210706121127785.png)

![image-20210706121404311](http://blog.cdn.ionluo.cn/blog/image-20210706121404311.png)





**diff算法**

![image-20210706121725352](http://blog.cdn.ionluo.cn/blog/image-20210706121725352.png)

![image-20210706122349504](http://blog.cdn.ionluo.cn/blog/image-20210706122349504.png)

![image-20210706161234841](http://blog.cdn.ionluo.cn/blog/image-20210706161234841.png)

![image-20210706122406643](http://blog.cdn.ionluo.cn/blog/image-20210706122406643.png)

![image-20210706161306892](http://blog.cdn.ionluo.cn/blog/image-20210706161306892.png)

![image-20210706161326080](http://blog.cdn.ionluo.cn/blog/image-20210706161326080.png)

![image-20210706161343107](http://blog.cdn.ionluo.cn/blog/image-20210706161343107.png)

![image-20210706145131148](http://blog.cdn.ionluo.cn/blog/image-20210706145131148.png)



### 模板编译

- 前置知识：JS的with语法
- vue template complier 将模板编译为 render 函数
- 执行 render 函数生成 vnode



**with语法**

> with 要慎用，它打破了作用域规则，易读性变差

```javascript ///
const obj = {a:100, b:200}

console.log(obj.a)  // 100
console.log(obj.b)  // 200
console.log(obj.c)  // undefined

// 使用with，能改变 {} 内自由变量的查找方式
// 将 {} 内自由变量，当做obj的属性来查找
with (obj) {
    console.log(a)  // 100
	console.log(b)  // 200
	console.log(c)  // 会报错！！！ Uncaught ReferenceError: c is not defined
}
```



**编译模板（vue template complier 将模板编译为 render 函数）**

![image-20210706163521064](http://blog.cdn.ionluo.cn/blog/image-20210706163521064.png)

```javascript
// "vue-template-compiler": "^2.6.10"
const compiler = require('vue-template-compiler')

// 插值
// const template = `<p>{{message}}</p>`
// with(this){return createElement('p',[createTextVNode(toString(message))])}
// h -> vnode
// createElement -> vnode

// // 表达式
// const template = `<p>{{flag ? message : 'no message found'}}</p>`
// // with(this){return _c('p',[_v(_s(flag ? message : 'no message found'))])}

// // 属性和动态属性
// const template = `
//     <div id="div1" class="container">
//         <img :src="imgUrl"/>
//     </div>
// `
// with(this){return _c('div',
//      {staticClass:"container",attrs:{"id":"div1"}},
//      [
//          _c('img',{attrs:{"src":imgUrl}})])}

// // 条件
// const template = `
//     <div>
//         <p v-if="flag === 'a'">A</p>
//         <p v-else>B</p>
//     </div>
// `
// with(this){return _c('div',[(flag === 'a')?_c('p',[_v("A")]):_c('p',[_v("B")])])}

// 循环
// const template = `
//     <ul>
//         <li v-for="item in list" :key="item.id">{{item.title}}</li>
//     </ul>
// `
// with(this){return _c('ul',_l((list),function(item){return _c('li',{key:item.id},[_v(_s(item.title))])}),0)}

// 事件
// const template = `
//     <button @click="clickHandler">submit</button>
// `
// with(this){return _c('button',{on:{"click":clickHandler}},[_v("submit")])}

// v-model
const template = `<input type="text" v-model="name">`
// 主要看 input 事件
// with(this){return _c('input',{directives:[{name:"model",rawName:"v-model",value:(name),expression:"name"}],attrs:{"type":"text"},domProps:{"value":(name)},on:{"input":function($event){if($event.target.composing)return;name=$event.target.value}}})}

// render 函数
// 返回 vnode
// patch

// 编译
const res = compiler.compile(template)
console.log(res.render)

// ---------------分割线--------------

// // 从 vue 源码中找到缩写函数的含义
// function installRenderHelpers (target) {
//     target._o = markOnce;
//     target._n = toNumber;
//     target._s = toString;
//     target._l = renderList;
//     target._t = renderSlot;
//     target._q = looseEqual;
//     target._i = looseIndexOf;
//     target._m = renderStatic;
//     target._f = resolveFilter;
//     target._k = checkKeyCodes;
//     target._b = bindObjectProps;
//     target._v = createTextVNode;
//     target._e = createEmptyVNode;
//     target._u = resolveScopedSlots;
//     target._g = bindObjectListeners;
//     target._d = bindDynamicKeys;
//     target._p = prependModifier;
// }

```

## Vue面试真题演练



## Vue3预学习





## React使用





## React原理



## React面试真题演练



## React Hooks



## Webpack和babel



## 项目设计



## 项目流程





## 其他Vue课程



## 其他React课程

- [React16+Redux 实战企业级大众点评Web App](https://coding.imooc.com/class/evaluation/313.html#Anchor)
- [React 17 系统精讲 结合TS打造旅游电商平台](https://coding.imooc.com/class/chapter/475.html#Anchor)



## 其他Vue面试题

- [30 道 Vue 面试题，内含详细讲解（涵盖入门到精通，自测 Vue 掌握程度）](https://juejin.cn/post/6844903918753808398)

- [很全面的vue面试题总结](https://segmentfault.com/a/1190000018630871)



## 其他React面试题

- [必须要会的 50 个 React 面试题](https://juejin.cn/post/6844903806715559943)

