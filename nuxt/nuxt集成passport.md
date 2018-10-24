**passport -> local验证**
==========
> ## **1.安装依赖**
```
npm install passport
npm install passport-local
```
> ## **2.策略配置**
```
import passport from 'koa-passport'
import LocalStrategy from 'passport-local'
import UserModel from '../dbs/models/users'

passport.use(
  new LocalStrategy(async (username, password, done) => {
    let where = {
      username
    }
    let result = await UserModel.findOne(where)
    if (result != null) {
      if (result.password === password) {
        return done(null, result)
      } else {
        return done(null, false, 'password is wrong!')
      }
    } else {
      return done(null, false, 'user not exist!')
    }
  })
)
```
>passport本身不处理验证，验证方法在策略配置的回调函数里由用户自行设置，它又称为验证回调。验证回调需要返回验证结果，这是由done()来完成的。<br/>
>在passport.use()里面，done()有三种用法：<br/>
> * 当发生系统级异常时，返回done(err)，这里是数据库查询出错，一般用next(err)，但这里用done(err)，两者的效果相同，都是返回error信息。
> * 当验证不通过时，返回done(null, false, message)，这里的message是可选的，可通过express-flash调用。
> * 当验证通过时，返回done(null, user)。

> ## **3.在index.js中集成passport中间件**
```
app.use(passport.initialize())
app.use(passport.session())
```
> ## **4.使用方法**
```
router.post('/signin', async (ctx, next) => {
  return Passport.authenticate('local', (err, user, info, status) => {
    if (err) {
      ctx.body = {
        code: -1,
        msg: err
      }
    } else {
      if (user) {
        ctx.body = {
          code: 0,
          msg: '登录成功'
        }
        return ctx.login(user)
      } else {
        ctx.body = {
          code: 1,
          msg: info
        }
      }
    }
  })(ctx, next)
})
```
> 这里的passport.authenticate(‘local’)就是中间件，若通过就进入后面的回调函数，并且给res加上res.user，若不通过则默认返回401错误。<br/>
> authenticate()方法有3个参数，第一是name，即验证策略的名称，第二个是options，包括下列属性：<br/>
> * session：Boolean。设置是否需要session，默认为true。<br/>
> * successRedirect：String。设置当验证成功时的跳转链接。<br/>
> * failureRedirect：String。设置当验证失败时的跳转链接。<br/>
> * failureFlash：Boolean or String。设置为Boolean时，express-flash将调用use()里设置的message。设置为String时将直接调用这里的信息。<br/>
> * successFlash：Boolean or String。使用方法同上。<br/>

> ## **5.HTTP request操作**
注意上面的代码里有个ctx.logIn(user)，它不是http模块原生的方法，也不是express中的方法，而是passport加上的，passport扩展了HTTP request，添加了四种方法。

> *login(user, options, callback)：用login()也可以。作用是为登录用户初始化session。options可设置session为false，即不初始化session，默认为true。<br/>
> *logout()：别名为logout()。作用是登出用户，删除该用户session。不带参数。<br/>
> *isAuthenticated()：不带参数。作用是测试该用户是否存在于session中（即是否已登录）。若存在返回true。事实上这个比登录验证要用的更多，毕竟session通常会保留一段时间，在此期间判断用户是否已登录用这个方法就行了。<br/>
> *isUnauthenticated()：不带参数。和上面的作用相反。<br/>

**passport -> OAuth验证**
==========
> ## **待补充....**
