<template>
  <div id="topbar">
    <div class="wrapper">
      <span class="user">Resumer</span>
      <div class="actions">
        <div v-if="logined" class="userActions" >
          <span class="welcome">欢迎，{{user.username}}</span>
          <a class="button" href="#" @click.prevent="signOut">登出</a>
        </div>
        <div v-else class="userActions">
          <a class="button primary" href="#" @click.prevent="signUpDialogVisible = true">注册</a>
          <a class="button" href="#" @click.prevent="signInDialogVisible = true">登入</a>
        </div>
        <MyDialog title="注册" :visible="signUpDialogVisible" v-show="signUpDialogVisible" @close="signUpDialogVisible = false">
          <SignUpForm @success="signIn($event)"/>
        </MyDialog>
        <MyDialog title="登陆" :visible="signInDialogVisible" v-show="signInDialogVisible" @close="signInDialogVisible = false">
          <SignInForm @success="signIn($event)"></SignInForm>
        </MyDialog>
      </div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  import MyDialog from './MyDialog'
  import SignUpForm from './SignUpForm'
  import SignInForm from './SignInForm'

  import AV from '../lib/leancloud'

  export default {
    name:'Topbar',
    data(){
      return{
        signUpDialogVisible:false,
        signInDialogVisible:false
      }
    },
    computed: {
      user(){
        return this.$store.state.user
      },
      logined(){
        return this.user.id
      },
    },
    methods:{
      signIn(user){
        this.signUpDialogVisible = false
        this.signInDialogVisible = false
        this.$store.commit('setUser',user)
      },
      signOut(){
        AV.User.logOut()
        this.$store.commit('removeUser')

      }
    },
    components:{MyDialog,SignUpForm,SignInForm},
  }
</script>

<style scoped lang='stylus' rel='stylesheet/stylus'>
  #topbar
    background: #ffffff
    box-shadow: 0 1px 3px 0 rgba(0,0,0,0.25)
    >.wrapper
      min-width: 1024px
      max-width: 1440px
      margin: 0 auto
      height: 64px

    >.wrapper
      display: flex
      padding: 0 16px
      justify-content: space-between
      align-items: center
      .logo
        font-size: 24px
        color: #000
      .button
        width:72px
        height: 32px
        border: none
        cursor: pointer
        font-size: 18px
        background: #ddd
        color: #222
        text-decoration none
        display inline-flex
        justify-content center
        align-items center
        vertical-align center
        &:hover
          box-shadow: 1px 1px 1px hsla(0,0,0,0.50)
        &.primary
          background: #02af5f
          color: #fff
      .actions
        display flex
        .userActions
          .welcome
            margin-right .5em

</style>

