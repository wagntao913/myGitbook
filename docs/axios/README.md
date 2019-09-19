# axios

###### 1.axios请求方式

- get  获取数据
- post  提交数据(表单提交 +文件上传)
- put  更新数据(所有数据推到后端)
- patch  更新数据(修改的数据推到后端)
- delete  删除数据

###### 2.axios请求 formData数据

```
  let data ={  key: value }
  let formData = new FormData()
  for(let key in data){
    formData.append(key,data[key])
  }
  axios.post(
    'url',
    formData
  ).then(res=>{})
```

###### 3.并发请求

同时处理多个请求，同时处理返回值
- axios.all() 进行多个请求
- axios.spread() 分割多个返回参数
```
  axios.all([
    axios.get('url1'),
    axios.get('url2')
  ]).then(
    axios.spread((url1Res,url2Res) =>{

    })
  )
```

###### 4. axios实例

- axios 常用配置
```
  import axios from 'axios'
  axios.create({
    baseURL: '', // 请求的域名。
    timeout: 1000, // 请求超时时常
    url: '' // 请求路径，
    method: 'get/post/put', // 请求方法
    headers: { //请求头
      token: ''
    }，
    params: {}, // 请求参数拼接在url
    data:{},// 请求参数拼接在请求体内
  })
```
- axios 配置  
开发过程中，通常实例配置使用较多
  - 全局配置
  ```
    axios.defaults.timeout = 1000   
    axios.defaults.baseURl = ''
    ...
  ```
  - 实例配置  
  ```
    let instance = axios.create({})  
    instance.defaults.timeout = 1000
    ...
  ```
  - 请求配置
  ```
    axios.get('url',{
      timeout: 1000
      ...
    })
  ```

###### 5.axios拦截器

请求/响应被处理前拦截

- 请求拦截器

```
axios.intesrceptors.request.use(
  config => {// 请求发送前处理
    return config
  },
  err => {// 请求错误处理
    // http状态码 401 超时， 404 not
    return Promise.reject(err)
  }
)

```
- 响应拦截器

```
  axios.intesrceptors.response.use(
    res => { // 请求成功对响应数据处理
      return res
    },
    err => {//响应错误处理
      //HTTP状态码  500系统错误 502 系统重启
      return Promise.reject(err)
    }
  )
```

###### 6.axios封装

```
import axios from 'axios'

//创建axios实例
let instance = axios.create({
  baseURL: '',
  timeout: 1000
})

//请求拦截器
instance.intesrceptors.request.use(
  config => {
    return config
  },
  err => {
    console.log('请求错误')
  }
)

// 响应拦截器
instance.intesrceptors.response.use(
  res => {
    return res.data
  },
  err => {
    console.log('请求错误')
  }
)
export default instance
```
