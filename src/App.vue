<template>
  <div id="app">
    <h2>User Roles Management</h2>
      <div class="actions">
        <label class="actions__label" for="filter">Role status</label>
        <div class="actions__input">
          <input type="text" placeholder="Search" >
          <font-awesome-icon class="icon" icon="search" />
        </div>
        <select class="actions__select" name="filter" v-model="selectedFilter">
          <option value="1">Active and Inactive</option>
          <option value="2">Active</option>
          <option value="3">Inactive</option>
        </select>
        <button class="actions__button">CREATE NEW ROLE</button>
      </div>
    <div v-for="role in roleList" :key="role.name">
      <RoleCard />
    </div>
  </div>
</template>

<script>
import RoleCard from './components/RoleCard.vue'

export default {
  name: 'App',
  components: {
    RoleCard
  },
  data(){
    return{
      roleList:[],
      selectedFilter:1
    }
  }
}
</script>

<style>
body{
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  padding: 2.5em 5em;
}
.actions{
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(2, 1fr);
  grid-template-areas:". label . ."
                      "input select . button";
  column-gap: 2.5em;
  row-gap: 0px; 
}
.filter-div{
  display: flex;
  flex-direction: column;
}
.actions__label{
  grid-area: label;
}
.actions__input{
  grid-area: input;
  position: relative;
  display: flex;
  flex-direction: row;
  align-items: center;
}
.actions__select{
  grid-area: select;
}
.actions__button{
  grid-area: button;
}
.actions__input input{
  width: 100%;
}
.actions__input .icon{
  position: absolute;
  right: 0;
}
</style>
