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
> * callback：回调函数，当校验通过是返回callback()，当校验不通过的时候返回callback(new Error('...'))。


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
