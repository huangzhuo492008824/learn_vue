- Object.freeze() 阻止响应系统追踪；
- 生命周期钩子函数：created，mounted，updated，destroyed
## 模板语法
### 插值
1. 文本 {{ msg }} ;
2. 原始html  v-html ;
3. 特性：Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 v-bind 指令;
4. 使用 JavaScript 表达式:
```javascript
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
```
5. 指令:
v-if, v-for, 
v-on: 监听DOM事件, v-on:click <===> @click
v-bind: 响应式地更新HTML事件, v-bind:href <===> :href ;

## 计算属性和侦听器
1. 计算属性：computed
```javascript
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```
2. 计算属性缓存 vs 方法:
a.计算属性是基于它们的依赖进行缓存的；
b.每当触发重新渲染时，调用方法将总会再次执行函数；
3. 计算属性 vs 侦听属性:
4. 计算属性的 setter:
计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：
```javascript
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```
5. 侦听器 ???

## 条件渲染
1. v-if v-else  v-else-if 
key 属性指定标签唯一性 ;
2. v-show 根据条件展示元素的选项
3. v-if vs v-show :
- v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
4. v-if vs v-for :
- 当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。
- 建议尽可能在使用 v-for 时提供 key，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
5. 数组更新检测:
- 变异方法: push, pop, shift, unshift, splice, sort, reverse ;
- 非变异方法: filter, concat, slice
6. 注意事项:
- 由于 JavaScript 的限制，Vue 不能检测以下变动的数组：

a. 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
a. 当你修改数组的长度时，例如：vm.items.length = newLength

- 正确的添加方式：
``` javascript
a. // Vue.set
  Vue.set(vm.items, indexOfItem, newValue)
b. // Array.prototype.splice
  vm.items.splice(indexOfItem, 1, newValue)
c. // vue.$set
  vm.$set(vm.items, indexOfItem, newValue)
```
- 为已有对象赋予多个新属性: Object.assign() 或 _.extend();
