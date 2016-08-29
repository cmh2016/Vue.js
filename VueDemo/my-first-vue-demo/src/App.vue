<template>
<div id="app">
  <h1 v-text="title"></h1>
  <input type="text" placeholder="what do？" @keyup.enter="additem" v-model="newitem">
  <ul>
    <li v-for="item in items" @click="toggleFinished(item)" :class="{finished:item.isFinished}">
      事项名称：{{item.label}}.
    </li>
  </ul>
</div>
</template>

<script>
//引入store
import Store from './store.js'

export default {
  //数据
  data() {
      return {
        // note: changing this line won't causes changes
        // with hot-reload because the reloaded component
        // preserves its current state and we are modifying
        // its initial state.
        title: 'vue实现的待ff办事项列表!',
        items: Store.fetch(),
        newitem: ''
      }
    },
    //方法
    methods: {
      toggleFinished: function(item) {
        //切换true/false
        console.log(item)
        item.isFinished = !item.isFinished
      },
      additem: function() {
        //push进数组
        this.items.push({
            label: this.newitem,
            isFinished: true
          })
          //初始化input
        this.newitem = ""
      }
    },
    //监听
  watch: {
    // 深度 watcher
    items: {
      handler: function(items) {
          Store.save(items);
      },
      deep: true
    }
  }
}
</script>

<style>
#app {
  text-align: center;
}

.finished {
  text-decoration: underline;
}

li {
  cursor: pointer;
}
</style>
