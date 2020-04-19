<template>
  <div>
    <div class="talk-list">
      <ul>
        <li v-for="(item,index) in talkList " :key="index">
          <div class="question">
            <span>{{item.question}}</span>
          </div>
          <div class="answer">
            <span>{{item.answer}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="talk-form">
      <el-form :inline="true" class="demo-form-inline">
        <el-form-item>
          <el-input v-model="talkText" placeholder="审批人"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="sendMseeage">发送</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
import md5 from "blueimp-md5";
export default {
  data() {
    return {
      talkText: "",
      talkList: []
    };
  },
  mounted() {
    this.sendMseeage();
  },
  methods: {
   //获取时间戳
    getTimeStap() {
      let timer = new Date();
      timer = Date.parse(timer);
      timer = timer.toString().substr(0, 10);
      timer = Number.parseInt(timer);
      return timer;
    },
    //生成随机字符串
    getNonceStr() {
      var str = Math.random();
      str = str.toString(16);
      str = str.substr(2);
      return str;
    },
    //接口鉴权
     getSign(obj) {
      let arr = Object.keys(obj).sort();
      let str = "";
      arr.map(val => {
        str += val + "=" + encodeURI(obj[val]) + "&";
      });
      str += "app_key=cmwPPo4L0aAgEoF2";
      str = md5(str).toUpperCase();
      return str
    },
    //发送聊天信息
    sendMseeage() {
      let baseUrl = "https://bird.ioliu.cn/v2?url=";
      let requestUrl = "https://api.ai.qq.com/fcgi-bin/nlp/nlp_textchat";
      let params = {
        app_id: 2134842408,
        time_stamp: this.getTimeStap(),
        nonce_str: this.getNonceStr(),
        session: "1001",
        question: this.talkText
      };
      params.sign = this.getSign(params);
      axios
        .get(baseUrl + requestUrl, {
          params
        })
        .then(res => {
          this.talkList.push({
            answer: res.data.data.answer,
            question: this.talkText
          });
        });
    }
  }
};
</script>
<style scoped>
.talk-form {
  position: fixed;
  bottom: 1rem;
  width: 100%;
  height: 1rem;
  background-color: #ddd;
  text-align: center;
}
.answer {
  text-align: left;
  font-size: .5rem;
}
.question {
  text-align: right;
  font-size: .5rem;
}
/* .question span {
  background-color: red;
}
.answer span {
  background-color: yellow;
} */
</style>