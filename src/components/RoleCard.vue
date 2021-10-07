<template>
  <div class="role-card">
    <p class="inactive" v-if="!role.active">Inactive</p>
    <div class="body">
      <h3>{{ role.name }}</h3>
      <p>{{ role.type }}</p>
      <p>{{ description }}</p>
      <faceIcons :faceList="role.images.slice(0,2)" :plusMore="role.images.length>2 ? role.images.length-2 : 0" />
    </div>
    <div class="footer">
      <p>{{ role.created }}</p>
      <div class="footer__actions" v-if="role.editable">
        <button>EDIT</button>
        <button>DELETE</button>
      </div>
      <font-awesome-icon v-if="!role.editable" class="icon" icon="lock" />
    </div>
  </div>
</template>

<script>
import FaceIcons from './FaceIcons.vue'

export default {
  name:"RoleCard",
  props:['role'],
  components:{
    FaceIcons
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
  mounted(){
    console.log(this.role.images)
  }
}
</script>

<style scoped>
.role-card{
  position: relative;
  height: 300px;
  /*width: 500px*/
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
}
.footer{
  display: flex;
  flex-direction: row;
  height: 50px;
  justify-content: space-between;
  align-items: center;
}
.footer__actions button{
  margin: 0 10px;
}
.icon{
  margin: 10px;
}

</style>