<template>
  <div id="app">
    <h1>Todo List</h1>
    <input
      id="input-text"
      type="text"
      placeholder="add new todo"
      v-model="todoName"
      @keyup.enter="addTodo"
    />
    <ul>
      <li v-for="todo of todos" :key="todo.id">
        {{ todo.name }}
        <button v-on:click="removeTodo(todo.id)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
const baseURL = "http://localhost:3000/todos";
export default {
  name: "App",
  data() {
    return {
      todos: [],
      todoName: "",
    };
  },

  async created() {
    try {
      const response = await axios.get(baseURL);

      this.todos = response.data;
    } catch (error) {
      console.error(error);
    }
  },
  methods: {
    async addTodo() {
      const response = await axios.post(baseURL, { name: this.todoName });

      this.todos = [...this.todos, response.data];

      this.todoName = "";
    },

    async removeTodo(id) {
      const deleted = await axios
        .delete(baseURL + "/" + id)
        .then((response) => {
          this.todos = this.todos.filter((todos) => todos.id !== id);
          alert("Record deleted \n                id: " + id);
        })
        .catch((error) => {
          alert("todo item deleted \n" + index);
        });
    },
  },
};
</script>

<style>
#app {
  font-family: "Roboto", Helvetica, Arial, sans-serif;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  padding: 40px;
  color: #a29bfe;
}
h1 {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  margin: 20px auto;
  width: 600px;
  height: 50px;
  background: white;
  border-radius: 75px;
  font-family: 'Montserrat', sans-serif;
  font-size: 50px;
  font-weight: lighter;
  letter-spacing: 2px;
  transition: 1s box-shadow;
  
  color: transparent;
  background-image: linear-gradient(
    90deg,
    #fc87da 0%,
    #cb94ef 36%,
    #41e0e7 70%,
    #28f6c2 100%
  );
  -webkit-background-clip: text;
  background-clip: text;
}
h1:hover{
  box-shadow: 0 5px 35px 0px rgba(0,0,0,.1);
}
h1:hover::before, h1:hover::after {
  display: block;
  content: '';
  position: absolute;
  width: 50px;
  height: 50px;
  background: #FDA8CF;
  border-radius: 75px;
  z-index: -1;
  animation: 1s clockwise infinite;
}
h1:hover:after {
  background: #F3CE5E;
  animation: 1s counterclockwise infinite;
}

@keyframes clockwise {
  0% {
    top: -5px;
    left: 0;
  }
  12% {
    top: -2px;
    left: 2px;
  }
  25% {
    top: 0;
    left: 5px;    
  }
  37% {
    top: 2px;
    left: 2px;
  }
  50% {
    top: 5px;
    left: 0;    
  }
  62% {
    top: 2px;
    left: -2px;
  }
  75% {
    top: 0;
    left: -5px;
  }
  87% {
    top: -2px;
    left: -2px;
  }
  100% {
    top: -5px;
    left: 0;    
  }
}

@keyframes counterclockwise {
  0% {
    top: -5px;
    right: 0;
  }
  12% {
    top: -2px;
    right: 2px;
  }
  25% {
    top: 0;
    right: 5px;    
  }
  37% {
    top: 2px;
    right: 2px;
  }
  50% {
    top: 5px;
    right: 0;    
  }
  62% {
    top: 2px;
    right: -2px;
  }
  75% {
    top: 0;
    right: -5px;
  }
  87% {
    top: -2px;
    right: -2px;
  }
  100% {
    top: -5px;
    right: 0;    
  }
}
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 2px solid;
  padding: 12px 24px;
  border-radius: 6px;
  margin-bottom: 12px;
}

#input-text {
  display: block;
  margin-right: auto;
  margin-left: auto;
  width: 300px;
  border: 2px inset;
  border-color: #a29bfe;
  padding: 12px 24px;
  border-radius: 6px;
  margin-bottom: 30px;
}

button {
  cursor: pointer;
  background-color: #a0a4d9;
  border: 1px solid #1f2023;
  color: #1f2023;
  font-weight: bold;
  outline: none;
  border-radius: 2px solid;
}
</style>
