<template>
  <div class="fashion">
    <div class="fashion__wrap">
      <fashion-item
      v-for="(item,index) in fashion"
      :key="index"
      :fashion='item'
      >
      </fashion-item>
    </div>
  </div>
</template>

<script>
import fashionItem from './fashionItem'
import {get} from '@/utils/request'

  export default {
    name:'fashion',
    components: {
      fashionItem
    },
    data() {
      return {
        fashion:[]
      }
    },
    methods: {
      getFashionData(){
        get('/fashions',{}).then(res=>{
          this.fashion=res.data.data
        })
      }
    },
    created () {
      this.getFashionData()
    }
  }
</script>

<style lang='scss' scoped>
.fashion{
  width: 100%;
  &__wrap{
    display: grid;
    grid-template-columns: repeat(auto-fit,minmax(120px,1fr));
    grid-gap: 17px;
  }
}
</style>