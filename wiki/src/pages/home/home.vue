<template>
  <div class="home">
    <div class="content">
      <div class="content__swiper">
        <swiper :slide='info.swiper'></swiper>
      </div>
      <div class="content__information" v-show="!isShow">
        <card
        v-for="item in post"
        :key="item.id"
        :post='item'
        @dblclick.native="showPost(item.id)"
        ></card>
      </div>
      <posts
      v-show="isShow"
      :postDetail='postDetail'
      :showPost='showPost'
      ></posts>
    </div>
    <div class="aside">
      <home-news :newsMsg='info'></home-news>
    </div>
  </div>
</template>

<script>
  import swiper from '@/components/swiper'
  import card from '@/components/card'
  import posts from '@/components/post'
  import homeNews from '@/pages/home/components/homeNews'
  import { get,post } from '@/utils/request'

  export default {
    name:'home',
    components: {
      swiper,
      card,
      homeNews,
      posts
    },
    data () {
      return {
        post:[],
        info:{},
        slide:[],
        postDetail:[],
        isShow:false
      }
    },
    methods: {
      getHomeData(){
        get('/posts',{}).then(res=>{
          this.post=res.data.data
        })
        get('/info',{}).then(res=>{
          this.info=res.data.data[0]
        })
      },
      showPost(id){
        this.isShow = !this.isShow
        if(this.isShow){
          this.postDetail = this.post.filter(item=>item.id == id)
          post('/addView',{id:id})
        }
      }
    },
    created () {
      this.getHomeData()
    },
  }
</script>

<style lang='scss' scoped>
.home{
  display: flex;
}
.content{
  width: 700px;
  margin-right: auto;
  &__swiper{
    width: 700px;
    height: 250px;
  }
  &__information{
    width: 100%;
    border-radius: 10px;
  }
}
.aside{
  width: 280px;
}
</style>