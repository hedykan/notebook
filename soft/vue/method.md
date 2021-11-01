# 组件调用方法

## 方法
1. provide/inject  
    这种方法用于父组件向子组件传递
2. import/ref
    这种方法用于父组件调用子组件方法
```html
<child ref="child">
...
setup() {
    const child = ref();
    return {
        child
    }
}
```