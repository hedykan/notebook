# 栅格
## 是什么
canvas元素默认被网格所覆盖。通常来说网格中的一个单元相当于canvas元素中的一像素。  
栅格的起点为左上角（坐标为（0,0））。所有元素的位置都相对于原点定位。

## 路径开始
```js
// 表示开始路径绘制
context.beginPath();
// 表示结束路径绘制, 会自动闭合路径线到开始点
context.closePath();
```

## 绘制/填充
```js
// 自动填充路径线内部，可以不用闭合路径
context.fill();
// 自动连接路径线，若要闭合需要使最后的点处于开始点或使用closePath()
context.storke();
```

## 移动
```js
// 移动到目标坐标，而不留下路径
context.moveTo(x, y);
```

## 画线
```js
// 直线，从当前坐标直线画到目标坐标
context.lineTo(x, y);

// 画弧线
context.arc();
```