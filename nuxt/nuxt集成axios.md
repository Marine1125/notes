> ## **1.配置文件**
```
import axios from 'axios'

const instance = axios.create({
  baseURL: `http://${process.env.HOST || 'localhost'}:${process.env.PORT ||
    '3000'}`,
  timeout: 1000,
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  },
  withCredentials: true
})

export default instance
```
> ## **2.vue中的请求方式**
>  由于我们在nuxt.config.js中已经配置了axios模块：<br/>
```
modules: [
  '@nuxtjs/axios',
],
```
>  所以我们可以直接在vue的模板文件中使用this.$axios来调用axios的方法<br/>
>  axios发起post请求<br/>
```
this.$axios
  .post('/users/signup', {
    username: username,
    password: password,
    email: email,
    code: code
  })
  .then((status, data) => {
    if (status === 200) {
      if (data && data.code === 0) {
      } else {
        console.log('...')
      }
    } else {
      console.log('...')
    }
  })
```
>  axios发起get请求<br/>
```
this.$axios
  .get('/users/signup')
  .then((status, data) => {
    if (status === 200) {
      if (data && data.code === 0) {
        console.log('...')
      } else {
        console.log('...')
      }
    } else {
      console.log('...')
    }
  })
```
> ## **3.关于跨域请求报错的解决方案**
>  在前后分离的场景下开发，经常会遇到下面场景：<br/>
`The 'Access-Control-Allow-Origin' header has a value 'http://xxx.com' that is not equal to the supplied origin. Origin 'http://localhost:3000' is therefore not allowed access.`<br/>
> 解决方案： 安装@nuxtjs/axios 和 @nuxtjs/proxy 官方模块<br/>
`npm install @nuxtjs/axios  @nuxtjs/proxy `<br/>
> 在 nuxtjs.config.js 配置文件最后添加下面模块，并且设置代理<br/>
```  
        modules: [
          '@nuxtjs/axios',
          '@nuxtjs/proxy'
        ],
        proxy: [
          [
            '/api', 
            { 
              target: 'http://localhost:3000', // api主机
              pathRewrite: { '^/api' : '/' }
            }
          ]
        ]
  ```
> 通过上面配置后，每次在项目中访问通过axios访问api的时候就会去localhost:3001主机服务去查询<br/>
> 通过url访问的时候直接由nuxtjs来处理，当然在浏览器上面写api/article 也会走代理哦<br/>
