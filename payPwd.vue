 <template>
  <div class="payPwd" id="pwdBox_bg" v-show="show">
    <div class="pay-tool" id="pwdBox">
      <div class="pay-tool-title">
        <i class="iconback" @click="back"></i>
        <strong>{{title}}</strong>
      </div>
      <div class="pay-tool-content">
        <div class="pay-tool-inputs">
          <div class="item" v-for="i in items" :key="i.index">
            <span class="icon_dot" v-if="password[i]"></span>
          </div>
        </div>
        <div class="pay-tool-clear" @click="password=[]">×</div>
        <div class="pay-tool-link">
          <span class="link" @click="forgetPwd">忘记密码？</span>
        </div>
      </div>
      <div class="pay-tool-keyboard">
        <ul>
          <li @click="keyUp($event)" v-for="val in keys" :key="val.index">{{ val }}</li>
          <li class="del" @click="del">&lt;</li>
        </ul>
      </div>
    </div>
  </div>
  <!--
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
  -->
</template>

<script>
//
const keys = () => [1, 2, 3, 4, 5, 6, 7, 8, 9, "", 0];
// let sendFlag = true // 防止重复发送密码
export default {
  data() {
    return {
      items: [0, 1, 2, 3, 4, 5],//长度为密码框个数
      keys: keys(),
      password: [],
      show: this.value
    };
  },
  // props: ["show",'showshow'],
  props: {
    value: Boolean,
    title: {
      type: String,
      default: "请输入支付密码"
    }
    // show:{
    //   type: Boolean,
    //   default: false
    // }
  },
  watch: {
    show(val) {
      console.log(val, 1);
      if (val) {
        setTimeout(() => {
          this.comeon();
        }, 1);
      } else {
        setTimeout(() => {
          this.getout();
        }, 1);
      }
    },
    value(val) {
      console.log(val, 0);
      this.show = val;
    }
  },
  computed: {},
  mounted() {},
  methods: {
    //返回
    back() {
      this.getout();
      // this.value=false
      setTimeout(() => {
        this.$emit("hidePwd"); // 调父组件hidePwd隐藏方法
      }, 200);
    },
    //进场动画
    comeon() {
      this.password = []; //进场时清空密码
      document.getElementById("pwdBox_bg").style.opacity = 1;
      document.getElementById("pwdBox").style.transform = "translateY(0%)";
    },
    //出场动画
    getout() {
      this.password = []; //进场时清空密码
      document.getElementById("pwdBox").style.transform = "translateY(100%)";
      document.getElementById("pwdBox_bg").style.opacity = 0;
      setTimeout(() => {
        this.show = false;
      }, 200);
    },
    //忘记密码
    forgetPwd() {
      this.getout; // 返回时清除password
      this.$emit("forgetPwd"); // 调父组件forgetPwd忘记密码
    },
    //数据操作
    keyUp(e) {
      let text = e.currentTarget.innerText; //获取元素内容
      let len = this.password.length;
      if (!text || len >= 6) return;
      this.password.push(text);
      this.ajaxData();
    },
    //删除
    del() {
      if (this.password.length <= 0) return false;
      this.password.shift();
    },
    //输入完密码后请求父组件sendPwd事件
    ajaxData() {
      if (this.password.length >= 6) {
        let password = this.password.join("");
        this.$emit("sendPwd", password);
      }
      return false;
    }
  }
};
</script>

<style lang="less" scoped>
.payPwd {
  position: fixed;
  width: 100%;
  height: auto;
  top: 0;
  left: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 999;
  opacity: 1;
  transition: opacity ease 300ms;
  -moz-transition: opacity ease 300ms;
  -webkit-transition: opacity ease 300ms;
  -o-transition: opacity ease 300ms;
  .pay-tool {
    position: fixed;
    width: 100%;
    bottom: 0;
    left: 0;
    height: 6.8rem;
    background-color: #fff;
    overflow: hidden;
    transform: translateY(100%);
    transition: all ease 300ms;
    -moz-transition: all ease 300ms;
    -webkit-transition: all ease 300ms;
    -o-transition: all ease 300ms;
    &-title {
      width: 100%;
      height: 0.8rem;
      padding: 0 0.2rem;
      line-height: 0.8rem;
      text-align: center;
      overflow: hidden;
      position: relative;
      border-bottom: 1px solid #eee;
      margin-bottom: 0.3rem;
      .iconback {
        position: absolute;
        top: -0.1rem;
        left: 0.2rem;
        width: 10px;
        height: 10px;
        border-left: 1px solid #aeaeae;
        border-top: 1px solid #aeaeae;
        transform: rotate(-45deg);
        margin-top: 0.4rem;
        margin-left: 0.3rem;
      }
      strong {
        font-size: 0.3rem;
        margin-left: -0.4rem;
      }
    }
    &-content {
      .pay-tool-inputs {
        width: 4.8rem;
        height: 0.8rem;
        margin: 0rem auto;
        border: 1px solid #b9b9b9;
        border-radius: 2px;
        box-shadow: 0 0 1px #e6e6e6;
        display: flex;
        box-sizing: border-box;
        .item {
          width: 0.8rem;
          height: auto;
          border-right: 1px solid #b9b9b9;
          line-height: 0.8rem;
          text-align: center;
          display: flex;
          flex-direction: column;
          justify-content: center;
          &:last-child {
            border-right: none;
          }
          // .icon_dot {
          //   display: inline-block;
          //   width: 100%;
          //   height: 100%;
          //   background: url("../assets/img/home/index/icon_dot.png") no-repeat;
          //   background-size: 100% 100%;
          // }
          .icon_dot {
            margin: 0 auto;
            display: inline-block;
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background: #000;
          }
        }
      }
      .pay-tool-clear {
        position: absolute;
        width: 15px;
        height: 15px;
        top: 69px;
        right: 33px;
        font-size: 16px;
        border-radius: 50%;
        border: 1px solid #aeaeae;
        color: #888;
        text-align: center;
        display: flex;
        flex-direction: column;
        justify-content: center;
      }
      .pay-tool-link {
        padding: 0.2rem 0.3rem 0.2rem;
        text-align: right;
        .link {
          font-size: 0.26rem;
          color: #3c8cfb;
        }
      }
    }
    .pay-tool-keyboard {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
      ul {
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        li {
          box-sizing: border-box;
          width: 33.3333%;
          height: 1rem;
          line-height: 1rem;
          text-align: center;
          border-right: 1px solid #aeaeae;
          border-bottom: 1px solid #aeaeae;
          font-size: 0.28rem;
          font-weight: bold;
          &:nth-child(1),
          &:nth-child(2),
          &:nth-child(3) {
            border-top: 1px solid #eee;
          }
          &:nth-child(3),
          &:nth-child(6),
          &:nth-child(9),
          &:nth-child(12) {
            border-right: none;
          }
          &:nth-child(10),
          &:nth-child(11),
          &:nth-child(12) {
            border-bottom: none;
          }
          // &:nth-child(10), &:nth-child(12), &:active {
          //   background-color: #d1d4dd;
          // }
          &:nth-child(12):active {
            background-color: #d8d8d8;
          }
          img {
            width: 0.5rem;
            vertical-align: middle;
            margin-bottom: 0.06rem;
          }
        }
      }
    }
  }
}
</style>
