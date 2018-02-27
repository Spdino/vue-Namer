<template>
  <div id="sideSelect">
    <el-card class="box-card">
      <el-switch
        v-model="Camel"
        @change="CamelChange"
        active-text="驼峰式">
      </el-switch>
      <el-switch
        v-model="Underline"
        @change="UnderlineChange"
        active-text="下划线_">
      </el-switch>
      <el-switch
        v-model="Separator"
        @change="SeparatorChange"
        active-text="分隔符-">
      </el-switch>
      <el-switch
        v-model="customize"
        @change="customizeChange"
        :active-text="Custom">
      </el-switch>
    </el-card>
    <el-dialog
      title="自定义符号"
      :visible.sync="centerDialogVisible"
      width="30%"
      center>
      <el-input v-model="inputSymbol" placeholder="请输入您想要的符号"></el-input>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="confirm">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: 'sideSelect',
    data() {
      return {
        Custom: '自定义',
        Camel: true,
        customize: false,
        Separator: false,
        Underline: false,
        centerDialogVisible: false,
        inputSymbol: ''
      }
    },
    methods: {
      CamelChange(v) {
        if (v === true) {
          this.customize = false
          this.Separator = false
          this.Underline = false
          this.Custom = '自定义'
          this.emitChange('Camel')
        }
      },
      UnderlineChange(v) {
        if (v === true) {
          this.customize = false
          this.Separator = false
          this.Camel = false
          this.Custom = '自定义'
          this.emitChange('Underline')
        }
      },
      SeparatorChange(v) {
        if (v === true) {
          this.customize = false
          this.Camel = false
          this.Underline = false
          this.Custom = '自定义'
          this.emitChange('Separator')
        }
      },
      customizeChange(v) {
        if (v === true) {
          this.Camel = false
          this.Separator = false
          this.Underline = false
          this.centerDialogVisible = true
        }
      },
      confirm() {
        this.Custom = this.inputSymbol
        this.emitChange('customize')
        this.centerDialogVisible = false
      },
      emitChange(v) {
        if (v === 'customize') {
          this.$emit('selectCange', this.inputSymbol)
        } else {
          this.$emit('selectCange', v)
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  #sideSelect {
    margin-top: 5px;
  }

  #sideSelect .el-switch {
    margin: 30px auto;
  }
</style>
