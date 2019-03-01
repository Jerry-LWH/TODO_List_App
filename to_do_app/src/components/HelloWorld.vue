<template>
 <div class = "hello">
  <h1>TODO LIST</h1>
  <h5>Open Task: {{unfinishedItems.length+pastDueItems.length}} ---- Completed Task: {{finishedItems.length}}</h5>
  <div id="message">
  </div>
  <input type="text" v-model = "content" placeholder = "Please enter task" required>
  <input type="date" v-model = "due_date">
  <button class="ui red basic button" v-on:click="add"> Add </button>
  <h2 v-if="unfinishedItems.length+pastDueItems.length==0 && finishedItems == 0"> You Don't Have Any Task In The List</h2>
  <h2 v-else-if="unfinishedItems.length+pastDueItems.length==0 && finishedItems != 0"> Well Done, You Have Finished All Task</h2>
  <ul id = "pastDue_items">
    <div v-for="(item,index) in pastDueItems" :key="index" :class="{'isActive':item.finish}" v-on:click="status(item)">
      <h5 class="past_due" v-if="index==0">By:{{item.due_date}} ---! Past Due !---<hr></h5>
      <h5 class="past_due" v-else-if="item.due_date != pastDueItems[index-1].due_date">By:{{item.due_date}} ---! Past Due !---<hr></h5>
      <li id="pastdue_li">{{item.content}} <img src="../assets/trash_can.png" height="25" width="25" align="right" @click="remove(item)"></li>
    </div>
  </ul>

  <ul id = "unfinished_items">
    <div v-for="(item,index) in unfinishedItems" :key="index" :class="{'isActive':item.finish}" v-on:click="status(item)">
      <h5 v-if="index==0">By:{{item.due_date}}<hr></h5>
      <h5 v-else-if="item.due_date != unfinishedItems[index-1].due_date">{{item.due_date}}<hr></h5>
      <li>{{item.content}} <img src="../assets/trash_can.png" height="25" width="25" align="right" @click="remove(item)"></li>
    </div>
  </ul>

  <ul id = "finished_items">
  <h5 v-if="finishedItems.length > 0">Completed Task</h5>
    <li v-for="item in finishedItems" :class="{'isActive':item.finish}" v-on:click="status(item)">
      {{item.content}}
      <img src="../assets/trash_can.png" height="25" width="25" align="right" @click="remove(item)">
    </li>
  </ul>
 </div>
</template>
<script>
export default {
  computed: {
    pastDueItems () {
      return this.items.filter((item) => {
        return item.finish === false
      }).filter((item) => {
        return new Date(item.due_date) < new Date()
      }).sort((a, b) => {
        if (new Date(a.due_date) > new Date(b.due_date)) {
          return 1
        } else if (new Date(a.due_date) < new Date(b.due_date)) {
          return -1
        }
        return 0
      })
    },
    finishedItems () {
      return this.items.filter((item) => {
        return item.finish === true
      }).sort((a, b) => {
        if (new Date(a.due_date) > new Date(b.due_date)) {
          return -1
        } else if (new Date(a.due_date) < new Date(b.due_date)) {
          return 1
        }
        return 0
      })
    },
    unfinishedItems () {
      return this.items.filter((item) => {
        return item.finish === false
      }).filter((item) => {
        return new Date(item.due_date) >= new Date()
      }).sort((a, b) => {
        if (new Date(a.due_date) > new Date(b.due_date)) {
          return 1
        } else if (new Date(a.due_date) < new Date(b.due_date)) {
          return -1
        }
        return 0
      })
    }
  },
  data () {
    return {
      items: [],
      content: '',
      due_date: ''
    }
  },

  methods: {
    show_message () {
      document.getElementById('message').innerHTML = 'Please enter both task and date.'
    },
    remove_message () {
      document.getElementById('message').innerHTML = ''
    },
    status (item) {
      item.finish = !item.finish
      this.remove_message()
    },
    remove (item) {
      this.items = this.items.filter((x) => { return x !== item })
    },
    add () {
      if (!this.content == '' && !this.due_date == '') {
        this.remove_message()
        this.items.push({
          due_date: this.due_date,
          content: this.content,
          finish: false
        })
        this.content = ''
        this.due_date = ''
      } else {
        this.show_message()
      }
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
hr{
  margin-right:250px;
}
h5{
  text-align:left;
  margin-left:250px;
}
.past_due{
  color:red;
}
#message {
  color:red;
}
input {
  height:25px;
  width: 200px;
}

ul {
  list-style-type: none;
}

li{
  text-align: left;
  border: 1px solid grey;
  font-size: 15px;
  background-color:#FFC300;
  padding: 10px;
  margin: 0px 400px 0px 400px;
}

li.isActive{
  background-color:lightgreen;
  color: grey;
  text-decoration: line-through;
}

p{
  color:Red;
}

</style>
