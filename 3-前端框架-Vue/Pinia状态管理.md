
| 通信场景                                                   | 方式                           |
| ------------------------------------------------------ | ---------------------------- |
| 父子组件间                                                  | props，emits [[父子组件传参]]       |
| 深层嵌套组件间                                                | inject，provide [[跨组件层级依赖注入]] |
| 大型项目中 全局状态管理（不相关组件的状态共享），比如用户认证状态、购物车数据、全局主题设置、自动刷新计数器 | vuex或pinia [[Pinia状态管理]]     |

pinia--vue的状态管理库，负责不同组件间通信

store：pinia数据仓库
- 用defineStore定义，包含name和options
- 每次调用store时进行实例化 --为了调用此API时获取最新的用户状态。但**频繁实例化影响性能，考虑将其放入vue的生命周期钩子中**。[[生命周期钩子]]
- options中设置store属性：
	- state：数据
		- 别的组件使用store内数据时，直接解构会失去响应性，需要使用storeToRefs
		- 重置 $reset,  批量修改 $patch
		- 在getters和setters中通过this访问state
	- getters：定义和访问computed计算属性对象
		- 其他组件直接调用，比如 password:userStore.getDecodedPwd()
		- 如果存用户数据，可以在这里调用解密函数get密码。
			- ![[Pasted image 20250324170332.png]]
		- [[getter写法纠错]]
			- 
	- actions：可写性
		- 修改state中的数据
		- 类比于java中setter ，可以放一堆对state中数据的set方法
		- 如果存用户数据，可以在这里调用加密函数set密码
			- ![[Pasted image 20250324170318.png]]
	- persist：持久化存储
		- 配置pinia-plugin-persistedstate插件后，
		- 设置**persist:true可将state中所有字段保存在localstorage中**，
		- 或者可以对persist进行配置，pick需要持久化的字段。
		- 未被持久化的字段，会在浏览器重载时重新由state指定。
		- ![[8b6bc4ca0b66b2adf181dbda774c0b6.jpg]]


实例代码 user.js 如下：
```js
import {defineStore} from "pinia";
import CryptoJS from 'crypto-js'
const KEY = "dfjoiefqi421"
export const useUserStore = defineStore("userState",{
    state:()=> { 
        return {
            user:{},
            avatar:'',
            nickName:'',
            token:'',
            uuid:'',
            isLogin:false,
            userName:'aa@wzu.edu.cn',
            password:"",
        }
    },
    getters:{
        getDecodedPwd(){
            return decryptText(this.password)
        }
    },
    actions:{
        setUser(userData){
            this.user = userData
        },
        setToken(token){
            this.token = token
        },
        setLoginState(isLogin){
            this.isLogin = isLogin
        },
        setUserName(name){
            this.userName = name
        },
        setPassword(pwd){
            this.password = encryptText(pwd)
        },
        setUserDetail(userDetail){
            this.avatar = userDetail?.avatar ?? ""
            this.nickName = userDetail?.name ?? ""
            this.uuid = userDetail?.uuid ?? ""
        },
    },
    // persist:true,
    persist:{
        serializer:{
            serialize:(state)=>encryptObject(state),
            deserialize:(data)=>decrypObject(data)
        }
    }
})

function encryptText(text){
    return CryptoJS.AES.encrypt(text,KEY).toString();
}

function decryptText(encoded_text){
    let bytes = CryptoJS.AES.decrypt(encoded_text,KEY);
    return bytes.toString(CryptoJS.enc.Utf8)
}

function encryptObject(object){
    return CryptoJS.AES.encrypt(JSON.stringify(object),KEY).toString()
}
function decrypObject(encoded_object){
    let bytes = CryptoJS.AES.decrypt(encoded_object, KEY);
    return JSON.parse(bytes.toString(CryptoJS.enc.Utf8))
}
```

store的state在其他组件的导入
- 使用**storeToRefs** 方法使store实例 内部state属性 转化为具有 `.value` 的 ref 对象集合
	- 此方法不处理getter或action
- [[响应式数据与watch监听]]
```js
import { useUserStore } from '@/stores/user';
const useUser = useUserStore()
const userRef = storeToRefs(useUser)

userRef.value.isLogin
```


