# vue3.x 公共变量设置
## 说明
在进行开发的时候，经常有很多的变量要共同使用，这时候就会有一个公共文件专门存放公共变量。  

## 使用
### 存放公共变量
vue3.x和vue2.x不同，在公共变量设置中不能使用`protoType.GLOBAL`，在vue3.x中其使用`app.config.globalProperties.foo = 'bar'`进行代替  

### 取用公共变量
mounted中可以直接使用`this.foo`获取到`bar`，但是在setup中无法获得。  
这时候就要用到[`getCurrentInstance`](https://v3.cn.vuejs.org/api/composition-api.html#getcurrentinstance)函数  
```js
const { proxy } = getCurrentInstance()
foo = proxy.foo
console.log(foo) // foo = 'bar'
```