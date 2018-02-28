<template>
  <div id="mainContent">
    <el-card class="box-card">
      <el-input
        type="textarea"
        placeholder="请输入需要转换的变量中文名"
        clearable
        :autosize="{ minRows: 4}"
        v-model="fromValue">
      </el-input>
      <div>
        <el-button type="danger" @click="cancel">清除</el-button>
        <el-button type="primary" :loading="loading" @click="handleSubmit">命名</el-button>
      </div>
      <el-input
        type="textarea"
        :autosize="{ minRows: 4}"
        v-model="toValue"></el-input>
    </el-card>
  </div>
</template>

<script>
  import $ from 'jquery';
  import MD5 from '../md5';

  export default {
    name: "mainContent",
    props: {
      seletedVal: {
        type: String
      }
    },
    data() {
      return {
        fromValue: '',
        toValue: '',
        loading: false,
        appid: '',  //添加你自己百度翻译API的appid
        key: '',    //添加你自己百度翻译API的key
        Type: 'Camel'
      }
    },
    watch: {
      seletedVal(val) {
        if (val === 'Separator') {
          this.Type = '-'
        } else if (val === 'Underline') {
          this.Type = '_'
        } else {
          this.Type = val
        }
      }
    },
    methods: {
      cancel() {
        this.fromValue = ''
      },
      handleSubmit(e) {
        this.loading = true
        if (this.LimitZh()) {
          let Data = {
            from: 'zh',
            to: 'en',
            appid: this.appid,
            salt: new Date().getTime(),
            q: this.fromValue
          }
          let sign = MD5(Data.appid + Data.q + Data.salt + this.key)
          Data.sign = sign
          $.ajax({
            url: 'http://api.fanyi.baidu.com/api/trans/vip/translate',
            type: 'get',
            dataType: 'jsonp',
            data: Data,
            success: (data) => {
              if (data.trans_result) {
                let result = data.trans_result[0].dst
                if (this.Type === 'Camel') {
                  result = this.ucfirst(result)
                } else {
                  result = this.Connector(result, this.Type)
                }
                this.toValue = result
              }
              this.loading = false
            }
          });
        } else {
          this.$message.error('这里只能输入中文哦！');
          this.loading = false
        }
      },
      ucfirst(str) {
        let Str = str.toLowerCase();
        Str = str.replace(/\b\w+/g, function (word) {
          return word.substring(0, 1).toUpperCase() + word.substring(1);
        });
        Str = Str.replace(/[-]/g, "");
        Str = Str.replace(/[']/g, "");
        return Str.replace(/\s+/g, "");
      },
      Connector(str, type) {
        let Str = str.toLowerCase();
        Str = Str.replace(/[-]/g, type);
        Str = Str.replace(/[']/g, "");
        return Str.replace(/\s+/g, type);
      },
      LimitZh() {
        let reg = /[\u4e00-\u9fa5]/
        if (reg.test(this.fromValue)) {
          return true
        }else{
          this.fromValue = ''
        }
      }
    }
  }
</script>

<style scoped>
  #mainContent .el-button {
    margin: 30px auto;
  }
</style>
