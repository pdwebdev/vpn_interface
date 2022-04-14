<template>
  <div class="location" :class="{ active: location.id === activeLocation  }" @click="this.$emit('change', this.location)">
    <div class="image">
      <img :src="require('@/assets/images/'+ location.url +'.png')" alt="">
    </div>
    <p>{{ location.title }}</p>
  </div>
</template>

<script>
export default {
  name: "location",
  props: {
    location: {
      type: Object,
      required: true
    },
    activeLocation: {
      type: Number,
      required: true
    }
  }
}
</script>

<style scoped lang="scss">
.location{
  @include flex(column, center, flex-start);
  cursor: pointer;
  transition: .5s ease-in-out;
  flex-shrink: 0;

  .image{
    display: flex;
    border-radius: 50%;
    transition: .5s ease-in-out;
    width: 25px; height: 25px;
    border: 1px solid transparent;
    overflow: hidden;

    img{
      width: 100%; height: 100%;
      border-radius: 50%;
    }
  }

  p{
    visibility: hidden;
    opacity: 0;
    font-size: 14px;
    margin: 8px 0 0;
    transition: .5s ease-in-out;
  }

  &:nth-child(1), &:nth-child(7){
    opacity: .3;
  }

  &:nth-child(2), &:nth-child(6){
    opacity: .5;
  }

  &:nth-child(3), &:nth-child(5){
    opacity: .7;
  }

  &:hover{
    opacity: 1;
  }
}

.location.active{
  padding-top: 0;
  opacity: 1;
  margin-top: -15px;

  .image{
    padding: 1px;
    border: 1px solid $white;
    width: 50px; height: 50px;
  }

  p{
    visibility: visible;
    opacity: 1;
  }
}
</style>