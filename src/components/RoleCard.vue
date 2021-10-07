<template>
  <div class="role-card">
    <p class="inactive" v-if="!role.active">Inactive</p>
    <div class="body" v-show="!editMode">
      <h3>{{ role.name }}</h3>
      <p>{{ role.type }}</p>
      <p class="body__desc">{{ description }}</p>
      <faceIcons :faceList="role.images.slice(0,2)" :plusMore="role.images.length>2 ? role.images.length-2 : 0" />
    </div>
    <div class="editable" v-show="editMode">
      <div class="editable__div">
        <label for="roleName">Role Name: </label>
        <input type="text" name="roleName" v-model="editName">
      </div>
      <div class="editable__div">
        <label for="roleType">Role type: </label>
        <input type="text" name="roleType" v-model="editType">
      </div>
      <div class="editable__div ">
        <label for="description">Description: </label>
        <textarea name="description" rows='4'  v-model="editDesc">
        </textarea>
      </div>
      <faceIcons :faceList="role.images.slice(0,2)" :plusMore="role.images.length>2 ? role.images.length-2 : 0" />
    </div>
    <div class="footer">
      <p>{{ dateText }}</p>
      <div class="footer__actions" v-if="role.editable">
        <div v-show="!editMode">
          <button v-on:click="clickEdit(false)">EDIT</button>
          <button v-on:click="deleteRole">DELETE</button>
        </div>
        <button v-show="editMode" v-on:click="clickEdit(true)">SAVE</button>
      </div>
      <font-awesome-icon v-if="!role.editable" class="icon" icon="lock" />
    </div>
  </div>
</template>

<script>
import FaceIcons from './FaceIcons.vue'

export default {
  name:"RoleCard",
  props:['role','initEdit','index'],
  components:{
    FaceIcons
  },
  data(){
    return{
      editMode: this.initEdit,
      dateText: 'Created: '+this.role.date,
      editName: '',
      editType: '',
      editDesc: ''
    }
  },
  computed:{
    description(){
      let desc = this.role.description
      if(this.role.description.length>125){
        desc = this.role.description.substring(0,126)+'...'
      }
      return desc
    }
  },
  methods:{
    clickEdit(isSave){
      if(isSave){
        let edition = { name:this.editName, type:this.editType, description:this.editDesc, index:this.index }
        if(this.editName == this.role.name && this.editType == this.role.type && this.desc == this.role.description){
          this.editMode = !this.editMode
          return
        }
        let date = new Date()
        edition.modified = `${date.getDay() < 10 ? '0'+date.getDay() : date.getDay() }/${ date.getMonth() < 10 ? '0'+date.getMonth() : date.getMonth()}/${date.getFullYear()}`
        this.$emit('roleEdited',edition)
      }
      else{
        this.editName = this.role.name;
        this.editType = this.role.type;
        this.editDesc = this.role.description;
      }
      this.editMode = !this.editMode
    },
    deleteRole(){
      this.$emit('roleDelete',this.role.index)
    }
  },
  watch:{
    role: {
      handler(n) {
        this.dateText = 'Modified: '+n.modified
      },
      deep: true
    }
  }
}
</script>

<style scoped>
.role-card{
  position: relative;
  height: 300px;
  display: flex;
  flex-direction: column;
  padding: 25px 15px 10px 15px;
}
.inactive{
  position: absolute;
  top: 5px;
  right: 20px;
}
.body h3{
  text-align: justify;
}
.body p{
  text-align: justify;
  text-overflow: ellipsis;
}
.body__desc{
  min-height: 54px;
}
.footer{
  display: flex;
  flex-direction: row;
  height: 50px;
  justify-content: space-between;
  align-items: center;
}
.footer p{
  font-size: 0.9em;
}
.footer__actions button{
  margin: 0 10px;
}
.icon{
  margin: 10px;
  color: rgb(134, 134, 134)
}
.editable__div{
  display: flex;
  flex-direction: row;
  margin-top: 10px;
  margin-bottom: 2px;
}
.editable__div label{
  width: 30%;
}
.editable__div input{
  margin-left: 10px;
  flex:1;
}
.editable :nth-child(3){
  display: flex;
  flex-direction: column;
}
.editable__div label{
  text-align: left;
}
.editable__div textarea{
  resize: none;
  margin-top: 5px;
}
</style>