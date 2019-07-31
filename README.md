# VUE_payPasswordComponent
vue支付密码组件,实现6位数字支付密码弹窗,兼容IOS,Android 

------使用方法------
@hidePwd隐藏组件事件
@forgetPwd忘记密码事件
@sendPwd(password)输入6位最后请求事件(参数)


父组件中:
  v-model="show" //动态绑定组件显示隐藏
  @forgetPwd="forget"  //忘记密码事件
  @sendPwd="sendPwd"  //填写完6位密码触发事件
  @hidePwd='hide' //隐藏组件时触发事件
	
	
//引入组件
import payPwd from "xxxxx/components/payPwd.vue";


//注册组件
components: {
    payPwd,
},


data(){
  return{
    show:false
  }
},
methods: {
  click(){this.show=true},
  forget(){忘记密码事件},
  sendPwd(){填写完6位密码触发事件},
  hide(){
    this.show=false//必写
    ...
  },
},
