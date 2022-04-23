
# 关于服务器暂停或者其他的重要通知

# 2021-12-09

更新v2ray到xray

> printf,  fprintf,  dprintf,  sprintf,  snprintf, vprintf, vfprintf, vdprintf, vsprintf, vsnprintf 

```c
0x0.07fcf47895ap-1022

```

`%hhn`一次写1个字节，相当于是`byte/char`型，最大范围是`0xff`，所以`(arg1-arg0+0x100)%0x100`这样的溢出写法，可以在不排序的情况下准确覆盖变量。

```python
context.arch='amd64'
fmt = fmtstr_payload(offset,{addr:data},write_size='byte')  # hhn
fmt = fmtstr_payload(offset,{addr:data},write_size='short') # hn
fmt = fmtstr_payload(offset,{addr:data},write_size='int')   # n
```



>其中10是offset，用的时候改一下

* 篡改`malloc_hook`/`free_hook`，使用`%100000c`触发`malloc`/`free`，劫持程序控制流
* 和堆相关利用结合，利用`printf`隐试调用`malloc`

?> 那么可以理解为`*`相当于c语言取值符号...嘛？

刨根问底一下，查看`man`文档，找到`*`的用方法：

![](http://image.taqini.space/img/20200405185627.png)

根据文档写个栗子：

```c
int main(){
    int width=10;
    int num=2333;
    puts("\ncase1:");
    printf("%*d", width, num);    
    puts("\ncase2:");
    printf("%2$*1$d", width, num);
}
```

运行得到如下输出：

```
case1:
      2333
case2:
      2333
```

## House of Husk

[todo](https://ptr-yudai.hatenablog.com/entry/2020/04/02/111507)...

### 小结

* `*`如果单独使用，则**按顺序**取参数列表中的参数
* `*`如果配合`$`使用，则取参数列表中**相应位置**的参数，如`*1$`
* 取出的参数将格式化为**十进制数**，用作限制**字符串宽度**

