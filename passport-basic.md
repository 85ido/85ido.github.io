# Passport基本使用及原理

Passport 是一个 Node.js 的认证中间件，特别灵活和模块化。可非常方便的用于基于 Express 的 Web 应用。支持用户名密码、Facebook 和  twitter 等认证，还有很多第三方的登录策略，支持QQ、微博、微信的登录。

## 概览

passport提供了一个用户验证的框架，具体的验证规则是靠额外的“策略(Strategy)”来实现的。策略会在下面详细描述。

## 验证流程

### Express启动时扩展Request

在引入passport时，默认HTTP模块的request对象会被扩展，加入以下方法：

* req.login/logIn: function(user, options, done) 标记当前请求已被user登录
* req.logout/logOut: void() 退出登录
* req.isAuthenticated(): bool, 检查是否登录（还有一个isUnAuthenticated(), 与这个方法反义）

在请求的处理方法里就可以用到以上的方法。

### 每个请求都执行`passport.initialize()`

该中间件负责初始化passport。其实就是为每个请求req上附加passport的对象实例（_passport）。
这个中间件是必须注册的，Express中使用以下代码注册：

	app.configure(function() {
	  ...
	  app.use(express.session({ secret: 'keyboard cat' }));
	  app.use(passport.initialize());
	  app.use(passport.session());
	  app.use(app.router);
	});

如果需要开启session的话，这个中间件需要放到session中间件的后面。

### 验证请求中的用户

passport统一使用以下方法验证用户：

**passport.authenticate( strategy, options, callback )**

这个方法返回一个function，符合`funciton(req, res, next)`的middleware格式。

对不同的验证场景，验证用户的时机不太一样。对于HTTP Basic Auth这类验证请求，每次请求的时候都要验证，这时候需要把以上方法注册为middleware（替换上一步骤代码中的`passport.session()`）。

对于网页登录这类验证场景，则需要在登录页的处理方法中调用该方法，代码示例：

	app.post('/login', passport.authenticate('local'));

这时候仅在登录时**验证**该登录请求，登录成功后，一般需要配合session策略，将登录状态保存在session中，这样每个请求的登录状态就可以通过session策略来获取了。

这种场景，需要确认`passport.authenticate`传入的options.session为true（默认值），并将`passport.session()`注册为middleware，以便在每个请求时都执行（注册在static和initialize之后）。

这个中间件会从每个req的session中取出'passport'的值，反序列化出user，并赋值给req.user（用于存储user对象的字段名是可配置的）。

### 执行具体的验证策略

策略集成了具体的用户验证逻辑。而`passport.authenticate(strategy, options, callback )`则是策略验证逻辑的调用入口。

策略（Strategy），是一个实现了`passport.Strategy`“接口”的**构造函数**。该接口只有一个需要实现的方法： `authenticate(req, options)`，另外一般需要一个name属性。

策略一般是在应用启动的时候使用`passport.use`方法在passport中注册的。验证时，passport会创建指定（authenticate的第一个参数，string）的策略实例，并在实例中添加以下方法供策略的authenticate方法使用：

* `success(user, info)`：用以通知passport验证通过，user为一个代表当前用户的对象
* `fail(challenge, status)`：用以通知passport验证失败了。challenge可选，用于记录错误原因（猜的），status一般默认是401。
* `redirect(url, status)`：因为不同版本的Express的redirect方法也不同，所以推荐使用这个redirect方法进行跳转，不然还要判断Express的版本。
* `pass()`：没有或不需要判断登录结果，通知passport跳过。
* `error(err)`：表示登录过程出现内部错误。

因为策略的authenticate方法传入了req（当前请求），策略可以从请求中获取form\query\header\证书甚至IP地址，来进行判断，并使用以上方法通知passport具体的验证结果。控制权交回到`passport.authenticate`。

### 根据策略的结果进行相应动作

策略执行完成之后，验证结果就出来了，根据结果的不同，passport要采取不同的动作，具体是由`passport.authenticate`中的options来确定的。

options的可选项：
- `session`   是否使用session存储验证结果，默认是 _true_
- `successRedirect`  如果登录成功，就跳到这个URL
- `failureRedirect`  如果登录失败，就跳到这个URL
- `assignProperty`   没看明白，貌似是要传到callback里去？

注意，如果指定了callback，那跳转逻辑什么的passport就不做了，全部交由callback来处理。

### 常用的Strategy

LocalStrategy，用于自己定义的基于用户名密码的验证规则。
