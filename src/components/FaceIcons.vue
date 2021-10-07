<template>
  <div class="face-icons">
    <div v-for="face in faceList" :key="face">
      <img :src="require('@/assets/'+face+'.jpg')" :alt="'person '+face">
    </div>
    <div class="plus" v-if="plusMore>0">
      +{{ plusMore }}
    </div>
  </div>
</template>

<script>
export default {
  name:'FaceIcons',
  props:['faceList','plusMore']
}
</script>

<style>
.face-icons{
  display: flex;
  flex-direction: row;
  padding: 5px 0px;
}
img{
  width: 45px;
  height: 45px;
  border-radius: 50%;
  margin: 0 2.5px;
}
.plus{
  width: 45px;
  height: 45px;
  border-radius: 50%;
  color: rgba(255,255,255,0.8);
  background-color: gray;
  font-size: 1.1em;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>