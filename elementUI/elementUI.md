**Vue中ElementUI的使用**
==========
> ## **1.表单字段验证**
> HTML示例
```
<el-form ref="ruleForm" :model="ruleForm" :rules="rules" status-icon label-width="100px" class="demo-ruleForm">
  <el-form-item label="昵称" prop="name">
    <el-input v-model="ruleForm.name"/>
  </el-form-item>
  <el-form-item label="邮箱" prop="email">
    <el-input v-model="ruleForm.email"/>
  </el-form-item>
  <el-form-item label="密码" prop="password">
    <el-input v-model="ruleForm.password"/>
  </el-form-item>
  <el-form-item label="确认密码" prop="cpassword">
    <el-input v-model="ruleForm.cpassword"/>
  </el-form-item>
</el-form>

```
> JS示例
```
 data() {
    return {
      statusMsg: '',
      ruleForm: {
        name: ''
      },
      rules: {
        //简单表单校验
        name: [
          {
            required: true,
            type: 'string',
            message: '请输入昵称',
            trigger: 'change'
          }
        ],
        email: [
          {
            required: true,
            type: 'email',
            message: '请输入邮箱',
            trigger: 'change'
          }
        ],
        password: [
          {
            required: true,
            message: '请输入密码',
            trigger: 'change'
          }
        ],
        cpassword: [
          {
            required: true,
            message: '请输入密码',
            trigger: 'change'
          },
          {
            //自定义表单校验
            validator: (rule, value, callback) => {
              if (value == '') {
                callback(new Error('请输入确认密码'))
              } else if (value !== this.ruleForm.password) {
                callback(new Error('两次输入的密码不一致，请重新输入'))
              } else {
                callback()
              }
            }
          }
        ]
      }
    }
  },
```
>validator一共有三个参数：<br/>
> * rule：校验规则的名称。
> * value：当前校验器校验的字段的值。
> * callback：回调函数，当校验通过是返回callback()，当校验不通过的时候返回callback(new Error('...'))。<br/>

>注意：<br/>
> <font color="#dd0000">\<el-form-item>标签必须有prop属性，且这prop属性要与rule中的字段一致，要与\<el-form-item>中的输入字段的v-model中的值一致，否则验证会有问题，坑！！！！ ！！！！！！</font>。<br/>
> 数字类型的验证需要在 v-model 处加上 .number 的修饰符，这是 Vue 自身提供的用于将绑定值转化为 number 类型的修饰符。
```
<el-input v-model.number="addCategoryForm.sort"/>
```
> 注意:<br/>
> <font color="#dd0000">如果你用数字校验，也用了v-model.number，你原来的校验规则里，如果有trigger: 'blur'，需要去掉。</font><br/>

> 对部分表单字段进行校验的方法
```
this.$refs['formName'].validateField('name', valid => {
  namePass = valid
})
```
> 此方法一共有三个参数：<br/>
> * formName：校验的表单的ID。
> * name：校验的字段，即HTML中的prop属性。
> * valid：校验结果即errorMessage，校验通过为null，校验不通过为错误信息。

> ## **2.文件上传**
```
<el-upload
  v-model="step.stepImg"
  :file-list="fileList"
  :show-file-list="false"//是否显示文件列表
  :on-success="onFileSuccess"//上传成功时回调函数
  class="avatar-uploader"
  action="/files/upload">
  <img
    v-if="step.stepImg"
    :src="step.stepImg"
    class="avatar">
  <i
    v-else
    class="el-icon-plus avatar-uploader-icon"/>
</el-upload>

<script>
export default {
  data() {
    return {
      fileList: [],
      itemForm: {
        steps: [{ stepImg: '', stepText: '' }]
      },
      imageUrl: '',
      rules2: {}
    }
  },
  methods: {
    onFileSuccess(res, file ) {
      console.log(index)
      this.itemForm.steps[index].stepImg = file.response.file
    },
    uploadItem: function() {
      console.log(this.itemForm)
    }
  }
}
</script>
```
> 多个Upload组件时，如何让文件上传时间绑定上传组件：<br/>
```
on-success="(res,file)=>{return handleHeadSuccess(res,file, index)}
```
> 这样就可以获取到每条数据的索引：<br/>

> ## **3.Vue+ElementUI动态生成新表单并添加验证**
```
<div
  v-for="(step,index) in itemForm.steps"
  :key="index">
  <el-form-item
    :prop="'steps.'+ index + '.stepImg'"
    :label="'步骤' + (index+1)">
    <el-upload
      v-model="step.stepImg"
      :file-list="fileList"
      :show-file-list="false"
      :on-success="(res,file)=>{return onFileSuccess(res,file,index)}"
      class="avatar-uploader"
      action="/files/upload">
      <img
        v-if="step.stepImg"
        :src="step.stepImg"
        class="avatar">
      <i
        v-else
        class="el-icon-plus avatar-uploader-icon"/>
    </el-upload>
    <el-input
      v-model="step.stepText"
      :rows="8"
      type="textarea"
      class="step-text"/>
  </el-form-item>
  <el-form-item>
    <el-button
      type="danger"
      @click="removeStep(index)">删除步骤</el-button>
  </el-form-item>
</div>
<script>
export default {
  data() {
    return {
      fileList: [],
      itemForm: {
        steps: [{ stepImg: '', stepText: '' }]
      },
      imageUrl: '',
      rules2: {}
    }
  },
  methods: {
    addStep: function() {
      this.itemForm.steps.push({
        stepImg: '',
        stepText: ''
      })
    },
    removeStep: function(index) {
      if (this.itemForm.steps.length === 1) {
        this.$alert('请至少有一个步骤', '错误', {
          confirmButtonText: '确定'
          // callback: action => {
          //   this.$message({
          //     type: 'info',
          //     message: `action: ${action}`
          //   })
          // }
        })
        return
      }
      console.log(index)
      this.$confirm('确认删除此步骤吗？')
        .then(_ => {
          this.itemForm.steps.splice(index, 1)
        })
        .catch(_ => {})
    }
  }
}
</script>
```
> 通过遍历循环组件所绑定的数据来生成表单控件，：<br/>
> 增加方法，即给绑定的数组添加一个元素：，：<br/>
```
this.itemForm.steps.push({stepImg: '', stepText: ''})
```
> 删除方法：即删除绑定数据对应的数组元素，：<br/>
```
this.itemForm.steps.splice(index, 1)
```

> ## **4.ElementUI弹出提示框和确认框**
> 弹出提示框：<br/>
```

this.$confirm('确认删除此步骤吗？')
  .then(_ => {
    this.itemForm.steps.splice(index, 1)
  })
  .catch(_ => {})
```
> 弹出提示框：<br/>
```
this.$alert('请至少有一个步骤', '错误', {
confirmButtonText: '确定'
 callback: action => {
   this.$message({
     type: 'info',
     message: `action: ${action}`
   })
 }
})
```

> ## **5.ElementUI绑定vue原生事件**
> 需要添加.native否则不生效：<br/>
```
<el-input v-model="id" placeholder="ID" @keyup.enter.native="handleClick"></el-input>
```