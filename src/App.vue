<template>
  <div id="app">
    <h2>User Roles Management</h2>
      <div class="actions">
        <label class="actions__label" for="filter">Role status</label>
        <div class="actions__input">
          <input type="text" placeholder="Search" v-model="searchText">
          <font-awesome-icon class="icon" icon="search" />
        </div>
        <select class="actions__select" name="filter" v-model="selectedFilter">
          <option value="1">Active and Inactive</option>
          <option value="2">Active</option>
          <option value="3">Inactive</option>
        </select>
        <button class="actions__button" v-on:click="createNewRole()">CREATE NEW ROLE</button>
      </div>
    <div class="role-list">
      <div v-for="role in filteredList" :key="role.index">
        <RoleCard :role="role" :initEdit="false" :index="role.index" @roleEdited="changeRole" @roleDelete="roleDeleted" />
      </div>
    </div>
    
  </div>
</template>

<script>
import RoleCard from './components/RoleCard.vue'
import fake_data from './data/fake_data.json'

export default {
  name: 'App',
  components: {
    RoleCard
  },
  data(){
    return{
      roleList:fake_data,
      selectedFilter:"1",
      searchText:""
    }
  },
  computed:{
    filteredList(){
      console.log(this.selectedFilter)
      let filterList = this.roleList.filter((a)=>{
        if(a.name.toLowerCase().includes(this.searchText.toLowerCase()) || a.type.toLowerCase().includes(this.searchText.toLowerCase()))
          return a
      })
      if(this.selectedFilter == "1"){
        return filterList
      }
      else if(this.selectedFilter == "2"){
        return filterList.filter((a)=>{
          if(a.active)
            return a
        })
      }
      else if(this.selectedFilter == "3"){
        return filterList.filter((a)=>{
          if(!a.active)
            return a
        })
      }
      
      return filterList
    }
  },
  mounted(){
    console.log(this.roleList)
    this.roleList = fake_data
  },
  methods:{
    changeRole(changedRole){
      let index = changedRole.index
      let realIndex = this.roleList.findIndex(a=>{
        if(index==a.index)
          return a
      })
      this.roleList[realIndex].name = changedRole.name
      this.roleList[realIndex].type = changedRole.type
      this.roleList[realIndex].description = changedRole.description
      if(changedRole.modified)
        this.roleList[realIndex].modified = changedRole.modified
    },
    roleDeleted(index){
      let realIndex = this.roleList.findIndex(a=>{
        if(index==a.index)
          return a
      })
      this.roleList.splice(realIndex,1)
    },
    createNewRole(){
      alert('This button does not work!')
    }
  }
}
</script>

<style>
body{
  margin: 0;
  --gray: rgb(128,128,128);
  --darker-gray: rgb(71, 71, 71);
  --darkest-gray: rgb(46, 46, 46);
  --light-gray: rgb(216, 216, 216);
  --white: white;
  --blue: rgb(75, 166, 219);
  --darker-blue: rgb(17, 119, 179);
  --red-alert: rgb(255, 96, 68);
  --red-delete: rgb(246, 56, 22);
  background-color: var(--light-gray);
  padding: 2.5em 4em;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  padding: 2.5em 4em;
  background-color: var(--white);
  border-radius: 15px;
}
button{
  cursor: pointer;
}
h2{
  text-align: left;
  color: var(--gray)
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
  font-size: 0.9em;
  text-align: left;
  justify-self: flex-start;
  align-self: flex-end;
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
  font-size: 1.05em;
  border: none;
  border-bottom: 2px solid var(--gray);
  transition: border 0.5s ease;
}
.actions__button{
  grid-area: button;
  background-color: var(--blue);
  border: none;
  font-size: 0.9em;
  font-weight: bold;
  color: white;
  border-radius: 5px;
  padding:5px 10px;
  transition: background 0.4s ease;
}
.actions__button:hover{
  background-color: var(--darker-blue);
}
.actions__input input{
  width: 100%;
  border: none;
  border-bottom: 2px solid var(--gray);
  padding: 5px 5px 5px 0;
  transition: border 0.5s ease;
  font-size: 1.1em;
  color: var(--blue);
}
.actions__input input:focus,.actions__select:focus{
  outline: none;
  border-bottom: 2px solid var(--blue);
}
.actions__input:focus-within .icon{
  color: var(--blue);
}
.actions__input .icon{
  position: absolute;
  right: 0;
  transition: color 0.5s ease;
}
.role-list{
  margin-top: 2em;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  column-gap: 2em;
  row-gap: 2em;
}
@media screen and (max-width:1300px) {
  body{
    padding: 1em 2em;
  }
  #app{
    padding: 2em 2.5em;
  }
  .actions{
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(2, 1fr);
    grid-template-areas:". label ."
                        "input select button";
    column-gap: 2.5em;
    row-gap: 0px;
  }
  .role-list{
    grid-template-columns: repeat(2, 1fr);
  }
}

@media screen and (max-width:750px) {
  body{
    padding: 0em 0em;
  }
  #app{
    padding: 2em 1em;
    border-radius: 0;
    min-height: 100vh;
  }
  .actions{
    display: grid;
    grid-template-columns: repeat(1, 1fr);
    grid-template-rows: repeat(5, 1fr);
    grid-template-areas:"input"
                        "label"
                        "select"
                        "."
                        "button";
    column-gap: 2.5em;
    row-gap: 0px;
  }
  .role-list{
    grid-template-columns: repeat(1, 1fr);
  }
}

</style>
