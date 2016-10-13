<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    
    <!-- 子组件调用父组件的内容 -->
    <h1>{{ msgFromFather }} </h1>

    <button v-on:click="onclickMe">click!</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello from component a!'
    }
  },

  // 子组件需要通过props属性来实现调用父组件
  props: ["msgFromFather"],

  methods: {
  	onclickMe: function() {
//  		console.log(this.msgFromFather);
  		this.$emit("child-tell-me-something", this.msg)
  	}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  color: #42b983;
}
</style>
