<template>
  <div class="publish">
    <!-- <div class="publish__title" v-model="postMsg.theme">选择话题</div> -->
    <!-- <m-input v-model="value"></m-input> -->
    <input
    type="text"
    class="publish__title"
    v-model="postMsg.title"
    placeholder="输入你的标题"
    >
    <textarea
    class="publish__content"
    v-model="postMsg.content"
    placeholder="请输入你想发布的内容"
    ></textarea>
    <div class="publish__tip" v-show="showTip">{{tip}}</div>
    <div class="publish__toolbar">
      <div>
        <span class="publish__tools iconfont">&#xe601;</span>
        <span class="publish__tools iconfont">&#xe8ba;</span>
      </div>
      <div class="publish__btn" @click="publish">发布</div>
    </div>
  </div>
</template>

<script>
import mInput from '@/components/m-input'
import { post } from '@/utils/request'

  export default {
    name:'publish',
    components:{
      mInput
    },
    data() {
      return {
        postMsg:{
          theme:'',
          title:'',
          content:'',
          img:[],
          id:''
        },
        tip:'',
        showTip:false
      }
    },
    methods: {
      hideTip(str){
        this.tip = str
        this.showTip = true
        setTimeout(()=>{
          this.showTip = false
        },1500)
      },
      publish(){
        if(this.$store.getters.isLogin){
          if(this.postMsg.title&&this.postMsg.content){
            this.postMsg.id = this.$store.getters.getUser.id
            post('/publish',{...this.postMsg})
            post('/addPost',{...this.$store.getters.getUser})
            this.hideTip('发布成功')
            this.postMsg.content = ''
            this.postMsg.title = ''
            this.$emit('updPost')
          }else{
            this.hideTip('标题或内容不能为空')
          }
        }else{
          this.hideTip('请先登录')
        }
      }
    }
  }
</script>

<style lang='scss' scoped>
.publish{
  position: relative;
  width: 100%;
  padding: 24px;
  border-radius: 5px;
  box-sizing: border-box;
  background-color: rgba($color: #fff, $alpha: .7);
  &__title{
    width: 100%;
    height: 24px;
    color: #99a2aa;
    background-color: transparent;
    outline: none;
    border: none;
  }
  &__tip{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    padding: 5px 10px;
    color: #fff;
    background-color: rgba($color: #000000, $alpha: .7);
    border-radius: 5px;
  }
 
  &__toolbar{
    display: flex;
    justify-content: space-between;
    height: 32px;
    margin-top: 16px;
  }
  &__tools{
    display: inline-block;
    width: 32px;
    height: 32px;
    line-height: 32px;
    font-size: 24px;
    color: #999;
    margin-right: 10px;
    cursor: pointer;
  }
  &__btn{
    width: 70px;
    height: 32px;
    font-size: 13px;
    background-color: #00a1d6;
    line-height: 32px;
    color: #fff;
    border: none;
    border-radius: 4px;
    text-align: center;
    opacity: .5;
    // cursor: not-allowed;
  }
  &__content{
    position: relative;
    width: 100%;
    font-size: 14px;
    letter-spacing: 1px;
    line-height: 22px;
    box-sizing: border-box;
    color: #222;
    outline-style: none;
    border: 0;
    background-color: transparent;
    resize: none;
    white-space: pre-line;
    word-break: break-all;
    &::-webkit-scrollbar{
      display: none;
    }
  }
}
</style>