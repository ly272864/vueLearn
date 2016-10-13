<template>
  <div id="app">
    <h1 v-text="title"></h1>
    <input v-model="newItem" v-on:keyup.enter="addNew">
    <ul>
      <li v-for="item in items" v-bind:class="{finished: item.isFinished}" 
      v-on:click="toggleFinished(item)">
        {{ item.label }}
      </li>
    </ul>

    <p>child tells me: {{ childWords }}</p>

    <!-- 通过component-a标签来调用子组件 -->
    <!-- 通过msgFromFather传递给子组件内容 -->
    <!-- 通过child-tell-me-something事件绑定的listenToMyboy来获取子组件传递的内容 -->
    <component-a 
    msgFromFather="you die" 
    v-on:child-tell-me-something="listenToMyboy"></component-a>
  </div>
</template>

<script>
import Store from "./store"
// ES6的语法引入子组件类似于 var ComponentA = require(./components/componentA)
import ComponentA from "./components/componentA"

export default {
  // 存放数据
  data: function(){
    return {
      title: "this id a todo list1",
      items: Store.fetch(),
      newItem: "",
      childWords: ""
    }
  },
  // 方法都应该写在methods里面
  methods: {
    toggleFinished: function (item) {
      item.isFinished = !item.isFinished
    },
    addNew: function () {
      this.items.push({
        label: this.newItem,
        isFinished: false
      });

      // 将数据存放在html5中的local Storage中
      Store.save(this.newItem)
      
      // 清空输入框
      this.newItem = ""
    },

    // 通过listenToMyboy方法获取子组件传递给过来的内容
    listenToMyboy: function (msg) {
      this.childWords = msg
    }
  },

  // 监听到内容的变化，并实时更新
  watch: {
    items: {
      handler: function (val, oldVal) {
        Store.save(val);
      },
      deep: true
    }
  },
  // 调用子组件声明标签，
  // 注：标签中涉及到的大写会自动转为小写并在前面新增“-”
  // 如此处调用会写成
  components: { ComponentA }
}
</script>

<style>
.finished {
  text-decoration: underline;
}

html {
  height: 100%;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}

#app {
  color: #2c3e50;
  margin-top: -100px;
  max-width: 600px;
  font-family: Source Sans Pro, Helvetica, sans-serif;
  text-align: center;
}

#app a {
  color: #42b983;
  text-decoration: none;
}

.logo {
  width: 100px;
  height: 100px
}
</style>
