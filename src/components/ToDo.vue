<template>
  <ul class="list">
    <li v-for="l, i in list" :key="i" :class="l.colour" class="task">
      <input class="task-meta name" @change="updateList(i, 'name', $event)" :value="l.name" />
      <input class="task-meta status" type="text" @change="updateList(i, 'status', $event)" :value="l.status" />
      <ColourSelect :selected="l.colour" @change-colour="updateColour(i, $event)" />
      <button @click="deleteTask(i)">x</button>
     </li>
    <form class="task" @submit.prevent="addTask">
      <input placeholder="Task" v-model="name" name="name" class="task-meta name" />
      <input placeholder="Current status" v-model="status" name="status" class="task-meta status" />
      <ColourSelect @change-colour="addColour" />
      <button type="submit">add</button>
    </form>
  </ul>
</template>

<script>
import ColourSelect from './ColourSelect.vue';
export default {
  name: 'ToDo',
  components: {
    ColourSelect
  },
  data: function() {
    return {
      name: '',
      status: '',
      colour: ''
    }

  },
  computed: {
    list: function() {
      console.log(localStorage.phireList);
      return localStorage.phireList ? JSON.parse(localStorage.phireList) : []
    }
  },
  mounted: function() {
    console.log(localStorage.phireList);
  },
  methods: {
    changeStatus: function(event, index) {
      console.log(event);
      console.log(index);
      console.log(this.color);
    },
    deleteTask: function(index) {
      const list = [...this.list];
      list.splice(index, 1)
      this.update('phireList', list);
    },
    update: function(storageKey, values) {
      localStorage.setItem(storageKey, JSON.stringify(values));
      this.$emit('re-render');
    },
    addColour: function(color) {
      this.color = color;
    },
    addTask: function() {
      const newList = [...this.list]
      newList.push({ name: this.name, status: this.status, colour: this.color });
      console.log(newList);
      this.update('phireList', newList);
    },
    updateColour: function(index, colour) {
      this.updateList(index, 'colour', colour)
    },
    updateList: function(index, type, event) {
      const newList = [...this.list];
      newList[index] = {
        ...this.list[index], [type]: event.target ? event.target.value : event,
      }
      this.update('phireList', newList);
    },
    getClass: function (statusType) {
      console.log(statusType);
      return "red";
    },
  }
}

</script>

<style>
.task {
  width: 100%;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 10px 20px;
  margin: 20px;
  box-sizing: border-box;
  display: flex;
}

.task-meta {
  cursor: pointer;
}

.green {
  background: green;
}

.yellow {
  background: yellow;
}

.red {
  background: red;
}

</style>
