# 现在高度并加滚动条

## 为什么
有时候想要让一个内部元素不要无限制增加，这时候就要限制这个元素的高度，然后用滚动条进行滚动。

## 怎么办
```css
<div class="foot">
</div>
<style>
    .foot {
        height: 300px;
        overflow: auto;
    }
</style>
```