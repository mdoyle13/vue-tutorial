<template>
  <div class="header">
    <h1>{{header}}</h1>
    <button type="button" v-if="editing" @click="toggleEdit(false)">Cancel</button>
    <button type="button" v-else @click="toggleEdit(true)">Add Item</button>
  </div>
  
  <div class="form" v-if="editing">
    <input type="text" @keyup.enter="saveItem" v-model="newItem" placeholder="Add a new item">
    <button type="button" name="button" @click="saveItem" :disabled="newItem.trim().length === 0">Save Item</button>
    <br>
    <label>
      <input type="checkbox" v-model="newItemHighPriority">
      High Priority?
    </label>
  </div>
  
  <p v-if="items.length == 0">You don't have any items :(</p>
  <ul>
    <li 
      v-for="item in reversedItems" 
      @click="togglePurchased(item)"
      :key="item.id"
      :class="[
        item.purchased? 'strikeout gray' : '',
        item.highPriority? 'high' : ''
      ]">
      {{ item.name }}
    </li>
  </ul>
</template>

<script>
export default {
  name: "App",
  computed: {
    reversedItems() {
      return [...this.items].reverse();
    }
  },
  methods: {
    saveItem() {
      const id = this.items.length + 1;
      this.items.push({id: id, name: this.newItem, highPriority: this.newItemHighPriority});
      this.newItem = '';
      this.newItemHighPriority = false
    },
    toggleEdit(bool) {
      this.editing = bool;
      this.newItem = '';
      this.newItemHighPriority = false
    },
    togglePurchased(item) {
      item.purchased = !item.purchased
    }
  },
  data() {
    return {
      header: "Shopping List",
      newItem: '',
      editing: false,
      newItemHighPriority: false,
      items: [
        {id: 1, name: "apples", purchased: false, highPriority: false},
        {id: 2, name: "bread", purchased: true, highPriority: true},
        {id: 3, name: "milk", purchased: false, highPriority: true}
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.strikeout {
  text-decoration: line-through;
}

.high {
  font-weight: bold;
}

.gray {
  opacity: 0.5;
}
</style>
