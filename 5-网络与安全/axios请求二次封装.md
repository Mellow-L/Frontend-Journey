Axios网络请求库，本质是对XMLHttpREquest的封装
- 直接使用 
- 或创建实例使用（不同实例不同配置，多个实例可对接不同后端服务）
	- axios.create创建实例，可配置baseURL，timeout等
- **配置优先级**：axios.default 全局默认属性配置 《 axios实例属性配置 《 请求级配置
- 常用GET，POST，PUT，PATCH，DELETE
	- PUT 提交少量数据
	- PATCH 推送所有数据

**二次封装** 主要指 对已有的工具库、组件、方法等进行封装，使其更易用、更通用，提高代码的复用性和可维护性

一个`vue`项目中处理登录操作时，在`<script>中`用`js`编写`login`函数，发送`axios`请求，用`post`方法提交表单数据：
```js
<button @click="login">登录<button>
-------
<script setup lang="js">
function login() {
  axios.post('auth/jwt/login', qs.stringify(loginData))
    .then(res => {
    data1.value = res.data;
    axios. defaults. headers['authorization' ]='Bearer '+res.data.access_token;
    })
    .catch(err => {
    data1.value = err;
    })
}
```
将`login`方法中发送请求的代码封装到`utils/apiUtils.js`（即项目工具库的api请求工具模块）的`apiLogin`方法中，

```js
export async function apiLogin(loginData){
    userStore = useUserStore()
    try{
        let res = await axiosClient.post('auth/jwt/login',qs.stringify(loginData))
        if(res){
            let token = 'Bearer '+res?.data?.access_token
            userStore.setToken(token)
            userStore.setLoginState(true)
            // 同步更新pinia中的用户名和密码为最近注册，登录的账户
            userStore.setUserName(loginData.username)
            userStore.setPassword(loginData.password)
            await apiGetProfile()
            return Promise.resolve(res?.data)
        }else{
            userStore.setLoginState(false)
            alertFail(apiLogin.name,"登录操作服务端无响应")
        }      
    }catch (e) {
        userStore.setLoginState(false)
        alertFail(apiLogin.name,e?.message)
    }
}
```

则原`login`函数简化为：

```js
async function login(){
  let data  = await apiLogin(formData)// 这里就是我们封装掉的东西
  if(data){
    await apiGetDetailProfile()
    console.log('back',back)
    if(back == 1){
      gotoBack()
    }else{
      gotoHome()
    } 
  }
}
```



