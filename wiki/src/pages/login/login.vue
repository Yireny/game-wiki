<template>
  <div class="login">
    <div class="wrap" :style="{'transform':styleObject}">
      <form class="wrap__interface wrap__login" action="">
        <div class="wrap__title">欢迎回来</div>
        <input class="wrap__input" type="text" v-model="account.id" placeholder="ID">
        <input class="wrap__input" type="password" v-model="account.password" placeholder="密码">
        <div
        class="wrap__btn"
        @click="login"
        >
        登录</div>
      </form>
      <form class="wrap__interface wrap__register" action="">
        <div class="wrap__title">加入我们</div>
        <input class="wrap__input" type="text" v-model="account.name" placeholder="用户名">
        <input class="wrap__input" type="password" v-model="account.password" placeholder="密码">
        <input class="wrap__input" type="password" v-model="account.confirm" placeholder="确认密码">
        <div
        class="wrap__btn"
        @click="register"
        >
        注册
        </div>
      </form>
      <div class="wrap__tip" v-show="showTip">{{loginTip}}</div>
      <div class="wrap__tip wrap__tip-back" v-show="showTip">{{loginTip}}</div>
    </div>
    <div class="switch">
      <div class="switch__title">没有/已有账号？</div>
      <div class="switch__btn" @click="rotate">切换</div>
    </div>
  </div>
</template>

<script>
import { post } from '@/utils/request'

  export default {
    name:'login',
    data() {
      return {
        deg:0,
        isAllowed:false,
        account:{
          name:'',
          id:'',
          account:'',
          password:'',
          confirm:''
        },
        loginTip:'',
        showTip:false
      }
    },
    computed: {
      styleObject:function(){
        return 'rotateY('+this.deg+'deg)'
      }
    },
    methods: {
      hideTip(str){
        this.loginTip = str
        this.showTip = true
        setTimeout(()=>{
          this.showTip = false
        },1500)
      },
      rotate(){
        if(this.deg==0){
          this.deg+=180
        }else{
          this.deg-=180
        }
      },
      register(){
          if(this.account.name && this.account.password && this.account.confirm){
            if(this.account.password == this.account.confirm){
              post('/register',{...this.account})
              this.hideTip('注册成功')
            }else{
              this.hideTip('两次密码不一致')
            }
          }else{
            this.hideTip('请填写完整信息')
          }
      },
      login(){
        if(this.account.id && this.account.password){
          post('/login',{...this.account}).then(res=>{
            if(res.data.status == 1002){
              this.hideTip(res.data.mag)
            }
            if(res.data.status == 1001){
              this.hideTip(res.data.msg)
            }
            if(res.data.status == 1000){
              // alert(res.data.msg)
              this.$store.commit('login')
              this.$store.commit('setUser',res.data.data)
              this.hideTip('登录成功')
              setTimeout(()=>{
                this.$router.push('/profile')
              },2000)
            }
          })
        }else{
          this.hideTip('请输入账号和密码')
        }
      }
    },
  }
</script>

<style lang='scss' scoped>
.login{
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  height: calc(100vh - 70px);
  min-height: 600px;
  perspective: 800px;
}
.wrap{
  position: relative;
  box-sizing: border-box;
  width: 400px;
  height: 400px;
  border-radius: 10px;
  background-color: rgba(255, 255, 255,.7);
  // transform: rotateY(0deg);
  transition: all 1.5s ease;
  transform-style: preserve-3d;
  &__title{
    height: 60px;
    color: #999;
    font-size: 25px;
    font-weight: 600;
  }
  &__interface{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  &__register{
    transform: rotateY(180deg);
  }
  &__input{
    background-color: rgba($color: #fff, $alpha: .08);
    width: 70%;
    height: 48px;
    border-radius: 8px;
    margin-bottom: 20px;
    padding: 0 16px;
    font-size: 18px;
    color: #999;
    font-weight: 500;
    outline: none;
    border: none;
    box-shadow: 0 4px 8px 0 rgba(78, 73, 79, .2);
    &:hover{
      box-shadow: 0 4px 8px 0 rgba(78, 73, 79, .5);
    }
    &:focus{
    box-shadow: 0 4px 8px 0 rgba(78, 73, 79, .5);
    }
  }
  &__btn{
    background-color: rgba($color: #00c3ff, $alpha: .5);
    width: 280px;
    height: 40px;
    padding: 6px 0;
    box-sizing: border-box;
    font-size: 24px;
    font-weight: 600;
    color: white;
    text-align: center;
    border-radius: 8px;
    margin-top: 15px;
    cursor: pointer;
    user-select: none;
    &:hover{
      background-color: rgba($color: #00c3ff, $alpha: .7);
    }
  }
  &__tip{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%) rotateY(0);
    padding: 10px;
    font-size: 15px;
    color: #fff;
    background-color: rgba($color: #000000, $alpha: .7);
    border-radius: 5px;
    &-back{
      transform: translate(-50%,-50%) rotateY(180deg);
    }
  }
}

.switch{
  width: 400px;
  height: 150px;
  border-radius: 10px;
  background-color: rgba(255, 255, 255,.7);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  &__title{
    width: 300px;
    height: 50px;
    color: #999;
    line-height: 50px;
    text-align: center;
    font-size: 20px;
    font-weight: 600;
  }
  &__btn{
    width: 300px;
    height: 50px;
    background-color: rgba($color: #00c3ff, $alpha: .5);
    border-radius: 15px;
    text-align: center;
    line-height: 50px;
    font-size: 20px;
    font-weight: 600;
    color: rgb(255, 255, 255);
    user-select: none;
    cursor: pointer;
    &:hover{
      background-color: rgba($color: #00c3ff, $alpha: .7);
    }
  }
}

</style>