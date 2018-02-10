<template>
<div id="todolist-widget" class="todolist-widget">
  <form @submit.prevent="add">
        <input type="text" v-model="inputValue"/>
        <input v-if="!isSearch" type="button" value="搜索" @click="find"/>
        <input v-else type="button" value="取消搜索" @click="cancelFind"/>
    </form>
    <div>
        <div v-for="(item, index) in list" :key="index">
            <div v-if="item.show">
                <input class="remove" @click="remove(index)" value="X" type="button"/>
                <span>{{item.index}}、</span>
                <form class="edit-form" v-if="item.edit" @submit.prevent="saveEdit(index)">
                    <input v-model="item.text"/>
                </form>
                <span v-else @click="edit(index)">{{item.text}}</span>
            </div>
        </div>
    </div>
    </div>
</template>

<script>
export default {
  name: 'TodoList',
  data: function () {
    return {
      isSearch: false,
      inputValue: null,
      list: [{
        index: 0,
        text: 'fuck ie6',
        edit: false,
        show: true
      }]
    }
  },
  methods: {
    add: function () {
      const currentMaxIndex = this.list.length - 1
      const s = {
        index: currentMaxIndex + 1,
        text: this.inputValue,
        edit: false,
        show: true
      }
      this.list.push(s)
      this.inputValue = null
    },

    remove: function (index) {
      const result = []
      this.list.forEach(function (element, i) {
        if (i !== index) {
          result.push(element)
        }
      })
      this.list = result
    },

    changeEditState: function (index, isEdit) {
      const result = this.list.map(function (element, i) {
        if (i !== index) {
          return element
        } else {
          return {
            index: element.index,
            text: element.text,
            edit: isEdit,
            show: element.show
          }
        }
      })

      this.list = result
    },

    edit: function (index) {
      this.changeEditState(index, true)
    },

    saveEdit: function (index) {
      this.changeEditState(index, false)
    },

    changeDisplayState: function (isSearch) {
      var val = this.inputValue
      const result = this.list.map(function (element) {
        if (element.text.indexOf(val) >= 0) {
          return element
        } else {
          return {
            index: element.index,
            text: element.text,
            edit: element.edit,
            show: !isSearch
          }
        }
      })

      this.list = result
      this.isSearch = isSearch
    },

    find: function () {
      this.changeDisplayState(true)
    },

    cancelFind: function () {
      this.changeDisplayState(false)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.todolist-widget {
    text-align: left;
    max-width: 600px;
    margin: 0 auto;
}

.remove {
    display: inline-block;
    margin: 0 10px;
    cursor: pointer;
}
.edit-form {
    display: inline-block;
    margin: 0;
}
</style>
