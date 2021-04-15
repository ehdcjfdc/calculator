<template>
  <div id = "app">
    <div class = "calculator">
      <div class = "result">
        {{result}}

      </div>
      <button class = "mod" @click = "clean()">AC</button>
      <button class = "mod" @click = "sign()">+/-</button>
      <button class = "mod" @click = "del()">DEL</button>
      <button id = "/" class = "op" @click = "append('/')">/</button>
      <button @click = "append('7')">7</button>
      <button @click = "append('8')">8</button>
      <button @click = "append('9')">9</button>
      <button id = "*" class = "op" @click = "append('*')">x</button>
      <button @click = "append('4')">4</button>
      <button @click = "append('5')">5</button>
      <button @click = "append('6')">6</button>
      <button id = "-" class = "op" @click = "append('-')">-</button>
      <button @click = "append('1')" >1</button>
      <button @click = "append('2')">2</button>
      <button @click = "append('3')">3</button>
      <button id = "+" class = "op" @click = "append('+')">+</button>
      <button id = "zero" @click = "append('0')">0</button>
      <button @click = "append('.')">.</button>
      <button id = "=" class = "op" @click = "calculate()">=</button>
    
    </div>
  </div>
</template>
<script>

export default{
  data(){
      return{
      result:0,
      onOp : "",
      output :"",


      }

  },
  methods:{
    isOper(c){
      if(c=='*' || c=='+' || c == '-' || c == '/'){
        return true
      }
      else return false
    },
    append(c){
      if(this.isOper(c)){
        if(this.onOp ==""){
          this.output += "("+this.result+")"
          
        }
        this.onOp = c
        this.buttonclean()
        var target = document.getElementById(c)
        target.style.backgroundColor = "white"
        target.style.color = "coral";
      }
      else if(c == '.'){
        var last = ''
        var i = 0;
        while(!this.isOper(this.result[this.result.length-1-i]) && this.result.length-1-i>=0){
          last = this.result[this.result.length-1-i] + last
          i++

        }
        if(last.indexOf('.')<=-1){
          this.result += ''+c
        }
      }
      else{
        if(this.result == '0'){
          this.result = ''+c
        }
        else if(this.result == "-0"){
          this.result = "-"+c
        }
        else if(this.result.length<14){
          if(this.onOp == ""){
            this.result +=''+c
          }
          else{
            this.result = c
            this.output += this.onOp
            this.onOp = ""
          }
        }
        else if(this.onOp != ""){
          this.result = c
          this.output += this.onOp
          this.onOp = ""
        }
      }
    },
    del(){
      if(this.result.length ==1 || (this.result.length == 2 && this.result[0] == '-')) this.result = '0'
      else this.result = this.result.substring(0,this.result.length-1)
    },
    calculate(){
      this.buttonclean()
      if(this.onOp == ""){
        this.output += "("+this.result+")"
      }
      this.result = parseFloat(eval(this.output).toFixed(10)).toString()
      
      var num = parseFloat(this.result);
      if(this.result.length > 14){
        this.result = `${num.toPrecision(1)}`
      }
      this.output = ""
      this.onOp = ""
    },
    sign(){
      if(this.onOp ==""){
        if(this.result[0] == '-'){
          this.result = this.result.substring(1,this.result.length)
        }
        else{
          this.result = '-' + this.result
        }
      }
      else{
        this.result = "-0"
        this.output += this.onOp
        this.onOp = ""
      }
      /*this.buttonclean()
      this.calculate()
      this.result += ''+'*-1'
      this.result = parseFloat(eval(this.result).toFixed(10)).toString()*/

    },
    clean(){
      this.output = ""
      this.buttonclean()
      this.result = '0'
    },
    buttonclean(){
      var target = document.getElementsByClassName("op")
      for(var i = 0;i<target.length-1;i++){
        target[i].style.backgroundColor = "coral"
        target[i].style.color = "white";
      }
    }
    

  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button{
  height: 50px;
  width: 100px;
  border-radius: 10px;
  margin: 2px;
  font-size: 1em;
  background-color:dimgrey;
  color : white;
}
.op{
  background-color:coral;
}
.mod{
  background-color:darkgray;
  color: black;
}
.calculator{
  width: 420px;
  background-color: black;
  border-radius: 15px;
  padding-bottom: 3px;
}
#zero{
  width: 202px;
}
.result{
  width: 400px;
  text-align: right;
  margin-bottom: 5px;
  font-size: 3em;
  color: white;
}
</style>
