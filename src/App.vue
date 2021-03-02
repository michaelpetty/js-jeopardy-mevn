<template>
  <div id="app">
    <h1> JavaScript Jeopardy!</h1>
    <table class="board">
      <thead>
        <tr>
          <th v-for="(category, i) in categories" :key="i">{{category}}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, i) in board" :key="i">
          <td v-for="(square, i) in row" :key="i">
            <a href="#" v-show="!square.answered" @click="clicky(square)">{{square.value}}</a>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="modal" v-show="modal.state != 'CLOSED'">
      <h2>{{ modal.category }} for {{ modal.value }}</h2>
      <p v-show="modal.state == 'ANSWER'">{{ modal.answer }}</p>
      <p><a href="#" v-show="modal.state == 'ANSWER'" @click="modal.state='QUESTION'">Show Question</a></p>
      <p v-show="modal.state == 'QUESTION'">{{ modal.question }}</p>
      <p><a href="#" @click="modal.state='CLOSED'">Close</a></p>
    </div>
    <div class="modal-overlay" v-show="modal.state != 'CLOSED'"></div>
  </div>
</template>

<script>
localStorage.debug = '*'
// import HelloWorld from './components/HelloWorld.vue'
import io from "socket.io-client"
export default {
  name: 'App',
  methods: {
    clicky(square) {
      square.answered = true
      this.modal.category = square.category
      this.modal.value = square.value
      this.modal.answer = square.answer
      this.modal.question = square.question
      this.modal.state = 'ANSWER'
      this.socket.emit('hello', square)
    },
  },
  created() {
    console.log('in the created method')
    this.socket = io('http://localhost:8000')
    console.log(this.socket)
  },
  mounted() {
    console.log('in the mounted method')
    this.socket.on('hello', data => {
      console.log('mounted data from socket: ', data)
    })
    this.socket.emit('hello', 'connected')
  },
  data() {
  console.log('in data method')
  return {
    modal: {
      state: 'CLOSED',
      category: null,
      value: null,
      answer: null,
      quetion: null
    },
    categories: [ "General", "Data Types", "Control Flow", "Functions", "Objects & Arrays" ],
    board: [
      [
        {
          category: "General",
          answered: false,
          value: "$200",
          answer: "A reference in code that holds a value.",
          question: "What is a variable?",
        },
        {
          category: "Data Types",
          answered: false,
          value: "$200",
          answer: "Meow, Washington D.C. and Taco are examples of this type",
          question: "What is a string?"
        },
        {
          category: "Control Flow",
          answered: false,
          value: "$200",
          answer: "This loop will run so long as a general condition is true",
          question: "What is a while loop?"
        },
        {
          category: "Functions",
          answered: false,
          value: "$200",
          answer: "A value that is passed into a funciton",
          question: "What is an argument?"
        },
        {
          category: "Objects & Arrays",
          answered: false,
          value: "$200",
          answer: "The position of an item in an array",
          question: "What is an index?"
        },
      ],
      [
        {
          category: "General",
          answered: false,
          value: "$400",
          answer: "The language used to add behavior to a web page",
          question: "What is JavaScript?"
        },
        {
          category: "Data Types",
          answered: false,
          value: "$400",
          answer: "This operator concatenates strings",
          question: "What is the plus sign?"
        },
        {
          category: "Control Flow",
          answered: false,
          value: "$400",
          answer: "This loop will run for a set number of iterations",
          question: "What is a for loop?"
        },
        {
          category: "Functions",
          answered: false,
          value: "$400",
          answer: "This statement outputs a single value from a function",
          question: "What is a return statement?"
        },
        {
          category: "Objects & Arrays",
          answered: false,
          value: "$400",
          answer: "This function removes the last element from an array",
          question: "What is the `pop()` function?"
        },
      ],
      [
        {
          category: "General",
          answered: false,
          value: "$600",
          answer: "The syntax for a single-line comment in JavaScript",
          question: "What are two slashes?"
        },
        {
          category: "Data Types",
          answered: false,
          value: "$600",
          answer: "When a variable is declared but not assigned a value, it is this type",
          question: "What is `undefined`?"
        },
        {
          category: "Control Flow",
          answered: false,
          value: "$600",
          answer: "This performs a loose, not strict, equality comparison",
          question: "What are two equal signs?"
        },
        {
          category: "Functions",
          answered: false,
          value: "$600",
          answer: "A variable that exists only within the body of a function",
          question: "What is a local variable?"
        },
        {
          category: "Objects & Arrays",
          answered: false,
          value: "$600",
          answer: "The succinct notation for accessing a value stored in an object",
          question: "What is dot notation?"
        },
      ],
      [
        {
          category: "General",
          answered: false,
          value: "$800",
          answer: "When a computer doesn't know how to proceed execution",
          question: "What is an error?"
        },
        {
          category: "Data Types",
          answered: false,
          value: "$800",
          answer: "When JavaScript converts a variable from one type to another",
          question: "What is coercion?"
        },
        {
          category: "Control Flow",
          answered: false,
          value: "$800",
          answer: "This statement can take the place of a very large `if` statement",
          question: "What is a `switch` statement?"
        },
        {
          category: "Functions",
          answered: false,
          value: "$800",
          answer: "The default return value of a function",
          question: "What is `undefined`?"
        },
        {
          category: "Objects & Arrays",
          answered: false,
          value: "$800",
          answer: "Short for \"JavaScript Object Notation\"",
          question: "What does JSON stand for?"
        },
      ],
      [
        {
          category: "General",
          answered: false,
          value: "$1000",
          answer: "When someone breaks down problems before writing code",
          question: "What is pseudocoding?"
        },
        {
          category: "Data Types",
          answered: false,
          value: "$1000",
          answer: "This keyword prevents reassignment",
          question: "What is `const`?"
        },
        {
          category: "Control Flow",
          answered: false,
          value: "$1000",
          answer: "An expression that branches, similar to the `if` statement",
          question: "What is a ternary?"
        },
        {
          category: "Functions",
          answered: false,
          value: "$1000",
          answer: "What determines where in our code a variable can be accessed",
          question: "What is scope?"
        },
        {
          category: "Objects & Arrays",
          answered: false,
          value: "$1000",
          answer: "The object where `pop`, `push`, `map` and more are located",
          question: "What is `Array.prototype`?"
        },
      ],
    ]
  }
}
  // components: {
  //   HelloWorld
  // }
}
</script>
