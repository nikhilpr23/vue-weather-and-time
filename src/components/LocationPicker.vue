<template>
  <div 
    class="Card"
    :class="{Dual : IsHome}"
    @click="$emit('click', place.id, place.timezone)"
  >
    <img :alt="place.name" :src='require(`../assets/Images/${place.name}.jpg`)'/>
    <p>{{place.name}}</p>
  </div>
</template>

<script>

export default {
  name: 'CustomLocation',
  props: ['place'],
  data: function() {
    return {
      IsHome: this.place.name === 'Home' ? true : false,
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.Card {
  width:29%;
  height: 140px;
  box-shadow: gray;
  cursor: pointer;
  margin: 10px 2px;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
}

.Card:hover {
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}

img{
  width: 100%;
  height: 83%;
  filter: grayscale(50%);
}

p {
  padding: 0;
  text-align: center;
  margin: 0;
  font-weight: bold;
  opacity: 0.8;
}

@media only screen and (min-width: 1600px) {
  .Card {
  width:30%;
  height: 140px;
  box-shadow: gray;
  cursor: pointer;
  margin: 10px 2px;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  }
}

@media only screen and (max-width: 600px){
  .Card {
    height: 120px;
  }
}

.Dual {
  width:70%;
  height: 195px;
}
</style>
