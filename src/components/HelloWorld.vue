<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      Select a colour for each button below. You can only choose 4 out of 12
      colours. Colours can't be repeated.
    </p>
    <h3>Buttons</h3>
    <ul>
      <li @click="hola('a')">
        <button id="a"></button>
      </li>
      <li @click="hola('b')">
        <button id="b"></button>
      </li>
      <li @click="hola('c')">
        <button id="c"></button>
      </li>
      <li @click="hola('d')">
        <button id="d"></button>
      </li>
    </ul>
    <div hidden="true" id="divColours">
      <h3>Available Colours</h3>
      <ul>
        <li>
          <div @click="select(1)" class="selectable s1"></div>
        </li>
        <li>
          <div @click="select(2)" class="selectable s2"></div>
        </li>
        <li>
          <div @click="select(3)" class="selectable s3"></div>
        </li>
        <li>
          <div @click="select(4)" class="selectable s4"></div>
        </li>
        <li>
          <div @click="select(5)" class="selectable s5"></div>
        </li>
        <li>
          <div @click="select(6)" class="selectable s6"></div>
        </li>
        <li>
          <div @click="select(7)" class="selectable s7"></div>
        </li>
        <li>
          <div @click="select(8)" class="selectable s8"></div>
        </li>
        <li>
          <div @click="select(9)" class="selectable s9"></div>
        </li>
        <li>
          <div @click="select(10)" class="selectable s10"></div>
        </li>
        <li>
          <div @click="select(11)" class="selectable s11"></div>
        </li>
        <li>
          <div @click="select(12)" class="selectable s12"></div>
        </li>
      </ul>
    </div>
    <p>{{ text }}</p>
    <ul v-if="completed">
      <h3>Solution</h3>
      <li>
        <div class="selectable" id="111"></div>
      </li>
      <li>
        <div class="selectable" id="222"></div>
      </li>
      <li>
        <div class="selectable" id="333"></div>
      </li>
      <li>
        <div class="selectable" id="444"></div>
      </li>
    </ul>
    <div>
      <button hidden="true" @click="check" id="btnCheck">Check Results</button>
      <button hidden="true" @click="reload" id="btnReload">Play again!</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  methods: {
    initApp: function() {
      for (let i = 0; i < 4; i++) {
        var r = Math.floor(Math.random() * 12) + 1;
        if (this.arr.indexOf(r) === -1) {
          this.arr.push(r);
        } else {
          r = Math.floor(Math.random() * 12) + 1;
          if (this.arr.indexOf(r) === -1) this.arr.push(r);
        }
      }
    },
    hola: function(data) {
      this.buttonId = data;
      document.getElementById("divColours").hidden = false;
      document.getElementById(data).style = "border: 1px solid black;";
    },
    select: function(data) {
      document.getElementById(this.buttonId).style = "border: none;";
      document.getElementById(this.buttonId).className += "s" + data;
      document.getElementById("divColours").hidden = true;
      this.count++;
      this.solution.push(data);
      if (this.count == 4) {
        document.getElementById("btnCheck").hidden = false;
      }
    },
    check: function() {
      this.completed = true;
      console.log(this.arr);
      console.log(this.solution);
      // Loop for array1
      for (let i = 0; i < 4; i++) {
        // Loop for array2
        let searching = true;
        for (let j = 0; j < 4 && searching; j++) {
          searching=true
            var k;
            switch (i) {
              case 0:
                k = "a";
                break;
              case 1:
                k = "b";
                break;
              case 2:
                k = "c";
                break;
              case 3:
                k = "d";
                break;
            }
          if (this.arr[j] === this.solution[i]) {
            // Return if common element found
            console.log("true");
            this.correct++;
            document.getElementById(k).style = "border: 5px solid green;";
            searching= false;
          }else if(searching && j==3){
            console.log("fals");
            document.getElementById(k).style = "border: 5px solid red;";
          }
        }
      }
      document.getElementById("btnCheck").hidden = true;
      document.getElementById("btnReload").hidden = false;
      this.text = "You got " + this.correct + "/4 correct colours.";
      setTimeout(() => {
        document.getElementById("111").classList.add("s" + this.arr[0]);
        document.getElementById("222").classList.add("s" + this.arr[1]);
        document.getElementById("333").classList.add("s" + this.arr[2]);
        document.getElementById("444").classList.add("s" + this.arr[3]);
      }, 500);
    },
    reload: function() {
      console.log("reload");
      window.history.go();
    },
  },
  data: function() {
    return {
      buttonId: "",
      count: 0,
      arr: [],
      solution: [],
      text: "",
      correct: 0,
      completed: false,
    };
  },
  beforeMount() {
    this.initApp();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
  margin-bottom: 30px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

button {
  font-family: "Montserrat", sans-serif;
  text-transform: uppercase;
  position: relative;
  border: none;
  font-size: 18px;
  transition: color 0.5s, transform 0.2s, background-color 0.2s;
  outline: none;
  border-radius: 3px;
  margin: 0 10px;
  padding: 23px 33px;
  border: 3px solid transparent;
}

button:active {
  transform: translateY(5px);
}

button::after,
button::before {
  border-radius: 3px;
}
.reload-btn {
  border: 2px solid grey;
  width: 20%;
  height: 20%;
  text-align: center;
}

.selectable {
  width: 40px;
  height: 40px;
  border-radius: 100%;
}
.s1 {
  background-color: lightgreen;
}
.s2 {
  background-color: greenyellow;
}
.s3 {
  background-color: yellow;
}
.s4 {
  background-color: rgb(221, 21, 21);
}
.s5 {
  background-color: purple;
}
.s6 {
  background-color: magenta;
}
.s7 {
  background-color: violet;
}
.s8 {
  background-color: salmon;
}
.s9 {
  background-color: firebrick;
}
.s10 {
  background-color: lightblue;
}
.s11 {
  background-color: blue;
}
.s12 {
  background-color: midnightblue;
}
</style>
