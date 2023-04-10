<script setup>
import { ref, onMounted, computed, watch } from 'vue';
const todos = ref([])
const name = ref("")

const input_content = ref("")
const input_category = ref(null)


//sort data in ascending order
const todos_asc = computed(() => todos.value.sort((a, b) => {
  return b.createdAt - a.createdAt
}))

// to set the title name to local storage
watch(name, (newVal) => {
  localStorage.setItem("name", newVal)
})

//to get the title name value from local storage
onMounted(() => {
  name.value = localStorage.getItem("name") || ""
  todos.value = JSON.parse(localStorage.getItem(todos)) || []
})

//on form submit addTodo function will execute
const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null) {
    return
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    createdAt: new Date().getTime()
  })

  input_content.value = ""
  input_category.value = null



}

//watch function will execute once when the todo array gets a value and here assign it to 
//local storage , with the deep it will check the todo everytime a change will happen
watch(todos, newVal => {
  localStorage.setItem("todos", JSON.stringify(newVal))
}, { deep: true })

//function to filter or remove todo
const removeTodo = todo => {
  todos.value = todos.value.filter(t => t !== todo)
}


const businessTodo = computed(() => todos.value.filter((items) => {
  return items.category === "business"
}))
const personalTodo = computed(() => todos.value.filter((items) => {
  return items.category === "personal"
}))
const sportsTodo = computed(() => todos.value.filter((items) => {
  return items.category === "sports"
}))
const shoppingTodo = computed(() => todos.value.filter((items) => {
  return items.category === "shopping"
}))
const foodTodo = computed(() => todos.value.filter((items) => {
  return items.category === "food"
}))
const familyTodo = computed(() => todos.value.filter((items) => {
  return items.category === "family"
}))
const studyTodo = computed(() => todos.value.filter((items) => {
  return items.category === "study"
}))
const othersTodo = computed(() => todos.value.filter((items) => {
  return items.category === "others"
}))


</script>

<template>
  <div class="mainDiv">
    <main class="app">
      
      <section class="greeting">
        <h2 class="title">
          What's up, <input type="text" placeholder="Name here" v-model="name" />
        </h2>
      </section>

      <section class="create-todo">

        <h3>Create A Todo</h3>
        <form @submit.prevent="addTodo">
          <h4>What's on your todo list?</h4>
          <input type="text" placeholder="e.g. make todo app" v-model="input_content" />

          <h4>Pick a category</h4>
          <div class="options">
            <label>
              <input type="radio" name="category" value="business" v-model="input_category" />
              <span class="bubble business"></span>
              <div>business</div>
            </label>

            <label>
              <input type="radio" name="category" value="personal" v-model="input_category" />
              <span class="bubble personal"></span>
              <div>personal</div>
            </label>

            <label>
              <input type="radio" name="category" value="sports" v-model="input_category" />
              <span class="bubble sports"></span>
              <div>Sports/Fitness</div>
            </label>

            <label>
              <input type="radio" name="category" value="shopping" v-model="input_category" />
              <span class="bubble shopping"></span>
              <div>Shopping</div>
            </label>

            <label>
              <input type="radio" name="category" value="food" v-model="input_category" />
              <span class="bubble food"></span>
              <div>Food</div>
            </label>

            <label>
              <input type="radio" name="category" value="family" v-model="input_category" />
              <span class="bubble family"></span>
              <div>Family</div>
            </label>

            <label>
              <input type="radio" name="category" value="study" v-model="input_category" />
              <span class="bubble study"></span>
              <div>Study/Reading</div>
            </label>

            <label>
              <input type="radio" name="category" value="others" v-model="input_category" />
              <span class="bubble others"></span>
              <div>Others</div>
            </label>
          </div>

          <input type="submit" value="Add todo" />
        </form>
      </section>
      
    </main>

    <section class="todo-list">
        <h3>TODO LIST</h3>
        <div class="list">
          <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
            <label>
              <input type="checkbox" v-model="todo.done" />
              <span :class="`bubble ${todo.category}`"></span>
            </label>

            <div class="todo-content">
              <input type="text" v-model="todo.content" />
            </div>

            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">
                Delete
              </button>
            </div>
          </div>
        </div>
      </section>
      
    <aside class="sortedTodos">

      <div :class ="`${businessTodo.length === 0 ? 'hideCard' : 'businessTodo'}`"> 
       <h3> Business Todo's </h3> 
      <div v-for="todo in businessTodo">
        <input type="text" v-model="todo.content">
      </div>
    </div>

      <div :class ="`${personalTodo.length === 0 ? 'hideCard' : 'personalTodo'}`"> 
        <h3>Personal Todo's </h3>
      <div v-for="todo in personalTodo">
        <input type="text" v-model="todo.content">
      </div>
    </div>

      <div :class ="`${sportsTodo.length === 0 ? 'hideCard' : 'sportsTodo'}`">
         <h3>Sports Todo's </h3>
      <div v-for="todo in sportsTodo">
        <input type="text" v-model="todo.content">
      </div>
    </div>

      <div :class ="`${shoppingTodo.length === 0 ? 'hideCard' : 'shoppingTodo'}`"> 
        <h3>Shopping Todo's</h3>
      <div v-for="todo in shoppingTodo">
        <input type="text" v-model="todo.content">
      </div>
    </div>

      <div :class ="`${foodTodo.length === 0 ? 'hideCard' : 'foodTodo'}`"> 
        <h3>Food Todo's </h3>
      <div v-for="todo in foodTodo">
        <input type="text" v-model="todo.content">
      </div>
    </div>

      <div :class ="`${familyTodo.length === 0 ? 'hideCard' : 'familyTodo'}`">
        <h3>Family Todo's </h3> 
      <div v-for="todo in familyTodo">
        <input type="text" v-model="todo.content">
      </div>
    </div>

      <div :class ="`${studyTodo.length === 0 ? 'hideCard' : 'studyTodo'}`"> 
        <h3>Study Todo's </h3>
      <div v-for="todo in studyTodo">
        <input type="text" v-model="todo.content">
      </div>
    </div>

      <div :class ="`${othersTodo.length === 0 ? 'hideCard' : 'othersTodo'}`"> 
        <h3>Other Todo's </h3>
      <div v-for="todo in othersTodo">
        <input type="text" v-model="todo.content">
      </div>
    </div>
    </aside>
  </div>
</template>

