<template>

  <table>
    <tr><td class="elevator_sh"> </td><td><button class = "btn" @click="callElev(5)">call</button> </td></tr>
    <tr><td class="elevator_sh"> </td><td><button class = "btn" @click="callElev(4)">call</button> </td></tr>
    <tr><td class="elevator_sh"> </td><td><button class = "btn" @click="callElev(3)">call</button> </td></tr>
    <tr><td class="elevator_sh"> </td><td><button class = "btn" @click="callElev(2)">call</button> </td></tr>
    <tr><td class="elevator_sh"> </td><td><button class = "btn" @click="callElev(1)">call</button> </td></tr>
    <div id="elev" class="elev">
     <div id="elevator" class="elevator" @transitionend="stopElev()"></div>
    </div>
  </table>
    
</template>

<script>
export default {
  name: 'App',
  components: { },
  data(){
    return{
      calls: [],
      call: 0,
    }
  },
  mounted(){ 
    console.log(localStorage)
    if(localStorage.getItem('call') != 0){
      this.call = localStorage.getItem('call')
    }else{
      this.call = 0
    }
    console.log('localStorage.calls=', localStorage.calls)
    if(JSON.parse(localStorage.getItem('calls')).length != 0){
      this.calls = this.calls.concat(JSON.parse(localStorage.getItem('calls')))
    }else{
      this.calls.length = 0
    }
    this.moveElev()
  },
  methods: {
    callElev(floor){
      if(this.calls.length > 0){
        for(let i=0; i < this.calls.length; i++){
          if(floor == this.calls[i]){
            return;
          }
        }
      }
      this.calls.push(floor)
      this.commandCalls()
    },
    commandCalls(){
      
      if(this.calls.length > 0){
        this.call = this.calls[0]
        localStorage.setItem('call', this.call)
        setTimeout(this.moveElev, 500)
      }
    },

    moveElev() {
      let options = { 
        5: '65px',
        4: '225px',
        3: '380px',
        2: '540px',
        1: '700px'
      }
      document.getElementById('elevator').style.top = options[this.call]
    }, 
    stopElev(){
      this.calls.shift()
      localStorage.setItem('calls', JSON.stringify(this.calls))
      document.getElementById('elev').style.display = 'block'
      setTimeout(this.commandCalls, 3000)
    },

  }

}
</script>

<style>

#app {

  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}

.btn{
  background:#eee;
  color:#088;
  text-decoration:none;
  
  width:50px;
  margin:20px auto;
  padding:10px 10px;
  transition:all 0.3s
}
.btn:hover{
  box-shadow:0px -5px 0 #088 inset;
}
table {
    width: 1500px;
    height: 500px;
    text-align: left;
}
tr{
  border: 1px blue;
  width: 1000px;
}
td {
   border: 1px solid grey;
}
td.elevator_sh{
  width: 100px;
  height: 153px;
}

.elevator{
  opacity: 1;
  background: blue;
  width: 100px;
  height: 150px;
  position: absolute;
  transition: all 2.7s ease-in-out;
  -webkit-transition: all 2.7s ease-in-out;
  -moz-transition: all 2.7s ease-in-out;
  -o-transition: all 2.7s ease-in-out;
  top: 699px;
  left: 15px;
}

</style>
