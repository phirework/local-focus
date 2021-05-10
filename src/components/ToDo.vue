<template>
  <ul class="list">
    <li v-for="l, i in list" :key="i" :class="l.colour" class="task">
      <input class="task-meta name" @change="updateList(i, 'name', $event)" :value="l.name" />
      <input class="task-meta status" type="text" @change="updateList(i, 'status', $event)" :value="l.status" />
      <ColourSelect :selected="l.colour" @change-colour="updateColour(i, $event)" />
      <button class="task-meta" @click="deleteTask(i)">✕</button>
     </li>
    <form class="task" @submit.prevent="addTask" :class="colour">
      <input placeholder="Task" v-model="name" name="name" class="task-meta name" />
      <input placeholder="Current status" v-model="status" name="status" class="task-meta status" />
      <ColourSelect @change-colour="addColour" />
      <button class="task-meta add" type="submit">✓</button>
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
      colour: 'none'
    }

  },
  computed: {
    list: function() {
      return localStorage.phireList ? JSON.parse(localStorage.phireList) : []
    }
  },
  methods: {
    sortList: function(array) {
      return array.sort((a, b) => {
        if (a.colour === b.colour) {
          if (a.name < b.name) {
            return -1
          } else if (a.name > b.name) {
            return 1;
          }
        } else if (a.colour === 'green' || b.colour === 'red' || b.colour === 'none') {
          return -1;
        } else if (a.colour === 'red' || a.colour === 'none' || b.colour === 'green') {
          return 1;
        }
        return 0;
      });
    },
    deleteTask: function(index) {
      const list = [...this.list];
      list.splice(index, 1)
      this.update('phireList', list);
    },
    update: function(storageKey, values) {
      localStorage.setItem(storageKey, JSON.stringify(this.sortList(values)));
      this.$emit('re-render');
    },
    addColour: function(colour) {
      this.colour = colour;
    },
    addTask: function() {
      const newList = [...this.list]
      newList.push({ name: this.name, status: this.status, colour: this.colour });
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
  }
}

</script>

<style>
ul, li {
  margin: 0;
  padding: 0;
  list-style: none;
}

input {
  padding: 8px 15px;
  background: transparent;
  border: 0;
  cursor: text;
  font-size: 18px;
}

form input {
  border: 1px solid #eee;
}

.red input,
.green input {
  color: #fff;
}


input:focus {
  background: #fff;
  color: #2c3e50;
}

button {
  cursor: pointer;
}

.task {
  width: 100%;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 10px 15px;
  margin: 0 10px 12px;
  box-sizing: border-box;
  display: flex;
}

.task-meta {
  margin: 0 10px;
}

.name {
  width: 60%;
}

.status {
  width: 40%;
}

button.task-meta {
  width: 30px;
  min-width: 30px;
  height: 30px;
  border-radius: 20px;
  text-align: center;
  font-weight: bold;
  background: transparent;
  border: 2px solid #fff;
  color: #fff;
  margin-top: 5px;
  margin-bottom: 5px;
}

.none button.task-meta,
button.task-meta.add {
  border-color: #2c3e50;
  color: #2c3e50;
}

.green {
  background: #28a745;
}

.yellow {
  background: #ffc107;
}

.red {
  background: #dc3545;
}
</style>