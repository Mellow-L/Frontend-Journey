**"二次封装"** 主要指 **对已有的工具库、组件、方法等进行封装，使其更易用、更通用，提高代码的复用性和可维护性**

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

## **💡 总结**

> **"二次封装" 就是把重复代码抽离成** 🔥 **更通用、更易维护的封装** 🔥，让项目开发更高效。

在 Vue3 中，常见的**二次封装**包括：
✅ **封装 `axios` 请求**（如 `request.js` 和 `apiUtils.js`）  
✅ **封装通用组件**（如 `BaseButton.vue`）  
✅ **封装 Vue 指令**（如 `v-copy` 复制指令）  
✅ **封装全局状态**（如 `useAuthStore`）

