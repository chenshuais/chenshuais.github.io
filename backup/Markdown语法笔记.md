# 简单记一下markdown的语法

## 1.标题
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
最多有六级标题，只需要前面加上 # 即可


## 2.字体
```
**加粗字体**
*斜体*
***斜体加粗***
~~删除线字体~~
```
效果如下

>**加粗字体**
>
>*斜体*
>
>***斜体加粗***
>
>~~删除线字体~~



## 3.引用
可以套好多层引用下去
```
>一级引用
>>二级引用
>>>三级引用
>>>>>>>>>>n级引用
```
效果如下
>一级引用
>>二级引用
>>>三级引用
>>>>>>>>>n级引用

## 4.分割线
三个或三个以上的 - 或 *
```
---
----
***
****
```
效果如下，好像看不出啥区别

>---
>----
>***
>****

## 5.图片
```
![图片下的文字](图片地址 "图片标题")
```
图片标题是指当鼠标移动到图片上时显示的内容，标题可以不加。如下所示：
```
![hello](https://upload.wikimedia.org/wikipedia/commons/d/d7/Android_robot.svg "hi")
```
效果如下
>`Gmeek-html<img src="https://upload.wikimedia.org/wikipedia/commons/d/d7/Android_robot.svg">`

### 6.超链接
```
[显示的文字](链接地址 "标题")
```
标题为当鼠标指向的时候显示的文字，标题也可以不加，如下所示
```
[我的博客](http://ian2018.cn "hello")
```
效果如下
>[我的博客](http://ian2018.cn "hello")

还有一种方式是通过a标签的方式，这种方式可以在新页面打开网址，如下
```
<a href="http://ian2018.cn" target="">我的博客</a>
```
显示效果相同

>`Gmeek-html<a href="http://ian2018.cn" target="">我的博客</a>`

## 7.列表
### 7.1无序列表
用 - + * 任何一个都可以，注意，它和后面的文字之间有个空格
```
- 无序列表内容
+ 无序列表内容
* 无序列表内容
```
效果如下
> - 无序列表内容
> + 无序列表内容
> * 无序列表内容

### 7.2有序列表
直接数字加. 就可以，注意它后面也有空格
```
1. 有序列表内容
2. 有序列表内容
```
效果如下

>1. 有序列表内容
>2. 有序列表内容

## 8.代码块
单行代码用一个(`)包起来，多行代码用(```)包起来即可，在键盘的左上角，数字1左边那个按键。下面的/只是为了转义 ```

```
`这里写单行代码 `
```

```
/```
// 这里面写代码
class Main {
    public void main(String[] ages) {
        System.out.println("Hello World");
    }
}
/```
```
效果如下
>`这里写单行代码 int a`
>
>```
>// 这里面写代码
> class Main {
>     public void main(String[] ages) {
>         System.out.println("Hello World");
>     }
> }
>```

<!-- ##{"timestamp":1543727940}## -->