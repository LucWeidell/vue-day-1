<template>
  <div class ="text-center mt-4">
    <img alt="Vue logo" src="./assets/logo.png">
    <form @submit.prevent="addTarget">

        <!--  NOTE two way data binding with a v-model  -->
      <input type="text" placeholder="name" v-model="state.newTarget.name">
      <input type="text" placeholder="role" v-model="state.newTarget.role">
      <input type="text" placeholder="health" v-model="state.newTarget.health">
      <button type="submit" class="btn btn-primary">Add</button>
    </form>
    <!-- NOTE v-for itterates over a collection where the first word is the variable to represent each element, in teh collection
    :key is the property on that object that can be used to uniquely identify it -->
    <div class='col-md-4' v-for="t in state.targets" :key="t.id">
      <!-- //No longer state.target.name / health -->
      <h1 v-if="t.health < 1">{{t.role}}</h1>
      <h3 v-else> Unconsious</h3>
      <!-- FIXME  -->
      <h1 :class="{'text-danger': t.health < 1, strickthrough: t.health < 10 }"> {{ t.name }} - {{t.health}} <h1>
      <button :disabled="t.health < 1" class ="btn btn-primary" @click ="attack( 'slap', 1, t)">Slap </button>
      <button :disabled="t.health < 1" class ="btn btn-warning" @click ="attack( 'kick', 2, t)">Kick </button>
      <button :disabled="t.health < 1" class ="btn btn-danger" @click ="attack( 'punch', 3, t)">Punch </button>

      <button v-show="t.health < 1" class ="btn btn-info" @click="t.health = t.maxHealth" >defib</button>
      <!--NOTE cool if
       <button v-if="t.health < 1" class ="btn btn-info" @click="t.health = t.maxHealth" >defib</button> -->
    </div>
    <h1 v-if="state.allDead">All Targets Eliminated You are now the Owner of CodeWorks
  </div>
</template>

<script>
import { reactive } from '@vue/reactivity'
import { computed } from '@vue/runtime-core'
export default {
  name: 'App',
  setup(){
    // NOTE reactive creates object where all properties are subscriped
    const state = reactive({
      targets: [
         {
        id: 1,
        role: 'Instructor',
        name: 'Mark',
        health: 100,
        maxHealth: 100,
        // dead: computed(()=> this.health < 1)
        },
        {
        id: 2,
        role: 'Instrucotr',
        name: 'Jake',
        health: 120,
        maxHealth: 120,
        // dead: computed(()=> this.health < 1)
        }, {
        id: 3,
        role: 'TA',
        name: 'Justin',
        health: 80,
        maxHealth: 80,
        // dead: computed(()=> this.health < 1)
        }
        ],
        allDead: computed(()=>{
          let dead = true;
          state.targets.forEach(t => {
            if(t.health > 1){
              dead = false;
            }
          })
          return dead
        }),

        // NOTE two way data binding with a Form element
        newTarget: {}
    })
   // const target = {
    //   role: 'Instructor',
    //   name: 'Mark',
    //   health: 100 not in state
    return {
      attack(descr, val, target){
        console.log('')
        target.health -= val
        if(target.health < 0) {
          target.health = 0
        }--
        // draw()
      },
      addTarget(){
        this.newTarget.maxHealth = state.target.health
        state.target.push(state.newTarget)
        this.newTarget = {}
      },
      state
      //target moved
    }
  }
}
</script>

<style scoped>
.strikethorugh{
  text-decoration: line-through;
}
</style>