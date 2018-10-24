**mangoDB集成nuxt**
=====

> ## **1.安装依赖**
`npm install mongoose`
> ## **2.配置文件**
```
export default {
  dbs: 'mongodb://127.0.0.1:27017/mumutea',
  redis: {
    get host() {
      return '127.0.0.1'
    },
    get port() {
      return 6379
    }
  }
}
```
> ## **3.index.js中的配置**
> mongoose配置<br/>
```
mongoose.connect(
  dbConfig.dbs,
  {
    useNewUrlParser: true
  }
)
```
> redis和session配置<br/>
```
app.keys = ['mumutea', 'keyskeys']
//Session config
app.proxy = true
app.use(
  session({
    key: 'mumutea',
    prefix: 'mumutea:uid',
    store: new Redis()
  })
)
```