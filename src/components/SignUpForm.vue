<template>
  <div>
    <form @submit.prevent="signUp">
      <div class="row">
        <label>用户名</label>
        <input type="text" v-model="formData.username" required>
      </div>
      <div class="row">
        <label >密码</label>
        <input type="password" v-model="formData.password" required>
      </div>
      <div class="actions">
        <input type="submit" value="注册">
        <span>{{errorMessage}}</span>
      </div>
    </form>
  </div>
</template>

<script>
import AV from '../lib/leancloud'
import getErrorMessage from '../lib/getErrorMessage'
import getAVUser from '../lib/getAVUser'

export default {
  name:'SignUpForm',
  data(){
    return {
      formData: {
        username: '',
        password: ''
      },
      errorMessage:''
    }
  },
  created(){
  },
  methods:{
    signUp(){
      let {username, password} = this.formData
      var user = new AV.User();
      user.setUsername(username);
      user.setPassword(password);
      user.signUp().then((loginedUser) =>{
        this.$emit('success', getAVUser())
      }, (error)=> {
        this.errorMessage = getErrorMessage(error)
      });
    }
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
  .row
    margin-bottom 5px
    label
      width 50px
      display inline-block
</style>

