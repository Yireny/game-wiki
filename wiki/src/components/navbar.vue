<template>
  <div class="navbar">
    <div class="navbar__inner">
      <div class="navbar__logo" @click="setPath(navItems[0])">
        <img src="@/assets/images/logo.png" alt="">
      </div>
      <div class="navbar__title" @click="setPath(navItems[0])">Arknights·明日方舟</div>
      <div class="navbar__wrap">
        <span
        class="navbar__item"
        :class="{'navbar__item--active': $store.state.navID==item.id}"
        @click="setPath(item)"
        v-for="item in navItems.slice(0,4)"
        :key="item.id"
        >
          {{item.content}}
        </span>
      </div>
      <div class="navbar__aside">
        <div class="navbar__item">消息</div>
        <div class="navbar__login" :class="{'navbar__login--active': $store.state.navID===5}" @click="setPath(navItems[4])">
          <div class="navbar__avatar">
            <img :src="avatar" alt="">
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'navbar',
  data() {
    return {
      user:{},
      //导航栏item
      navItems:[
        {
          id:1,
          path:'/home',
          content:'首页'
        },
        {
          id:2,
          path:'/wiki',
          content:'百科'
        },
        {
          id:3,
          path:'/strategy',
          content:'攻略'
        },
        {
          id:4,
          path:'/community',
          content:'社区'
        },
        {
          id:5,
          path:'/profile',
          content:'用户'
        },
        {
          id:6,
          path:'/community',
          content:'消息'
        }
      ]
    }
  },
  computed:{
    avatar(){
      return this.$store.getters.getUser.avatar?this.$store.getters.getUser.avatar:'https://prts.wiki/ak.png?8efd0'
    }
  },
  methods: {
    setPath(item){
      if(this.$route.path!=item.path){
        this.$router.push({
          path:item.path
        })
      }
      if(this.$store.state.navID!=item.id){
        this.$store.commit('changeNav', item.id)
      }
    },
  },
  mounted () {
    // console.log(this.avatar)
  }
}
</script>

<style lang='scss' scoped>
.navbar {
  width: 100%;
  height: 60px;
  font-size: 0;
  color: rgb(253, 255, 255);
  background-color: #00c3ff;
  z-index: 99;
  user-select: none;
  &__inner{
    position: relative;
    width: 1200px;
    height: 60px;
    // line-height: 60px;
    margin: 0 auto;
    display: flex;
    align-items: center;
  }
  &__logo{
    width: 90px;
    cursor: pointer;
    box-sizing: border-box;
    padding: 0 15px;
    & img{
      width: 76px;
      vertical-align: middle;
    }
  }
  &__title{
    position: relative;
    width: 180px;
    margin-left: 10px;
    padding-left: 10px;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    &::before{
      position: absolute;
      top: 0;
      left: 0;
      height: 15px;
      border-left: 1px solid rgba(255, 255, 255, .3);
      content: '';
    }
  }
  &__wrap{
    height: 100%;
  }
  &__item{
    display: inline-block;
    width: 68px;
    height: 60px;
    line-height: 20px;
    padding: 20px 0;
    box-sizing: border-box;
    font-size: 14px;
    text-align: center;
    cursor: pointer;
    user-select: none;
    vertical-align: middle;
    &--active{
      font-size: 16px;
      font-weight: bold;
      background-color: #2ECEFF;
    }
    &:hover{
      background-color: #2ECEFF;
    }
  }
  &__aside{
    position: absolute;
    top: 0;
    right: 0;
    display: flex;
    cursor: pointer;
  }
  &__login{
    width: 76px;
    height: 60px;
    padding: 12px 20px;
    box-sizing: border-box;
    &--active{
      font-weight: 600px;
      background-color: rgb(46, 206, 255);
    }
    &:hover{
      background-color: rgb(46, 206, 255);
    }
  }
  &__avatar{
    width: 36px;
    height: 36px;
    padding: 2px;
    border-radius: 100%;
    box-sizing: border-box;
    background-color: #fff;
    & img{
      width: 36px;
      height: 36px;
      vertical-align: top;
      border-radius: 100%;
    }
  }
}
</style>