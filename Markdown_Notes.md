1.  空格:  
    半角空格使用 `&ensp;`  
    全角空格使用 `&emsp;`  
    手动空行使用HTML标签 `<br>` (不要随便使用) 
    一般使用两个全角空格就可以实现首行四个缩进
<br><br>

2.  换行：  
    每个段落之间用空行隔开，可以实现换行  
    段落之内强制换行： 在上一行后面加入两个以上空格就可以，然后回车
<br><br>

3.  标题：  
    使用 `Setext 形式`的，一级标题在下面加上===，二级标题在下面加上一行--- 

    一级标题
    ===
    二级标题
    ---
    使用 `Atx 形式`的，在前面加上#，还有空格一共六级标题  

    ### 三级标题

    #### 四级标题
<br>

4.  引用区块：  
    使用>在段首实现区块，区块可以实现嵌套，使用两个>>获知多个
<br><br>

5.  代码区块：  
    前面四个缩进或者一个制表符  
    当只是一行代码的时候，使用 \` \` 括起来可以实现  
    ex：`printf("Hello, World!")`
<br><br>

6.  代码高亮区块：    
    三个\``` + 编程语言+```
    ```C  
    struct {
        int year;
        int month;
        int day;
    }bdate;
    ```
<br>

7.  插入链接：  
    行内式：  [深的小栈](https://www.blog.caishenpc.top "深的小栈")  
    参考式：  [深的小栈][id]

    [id]:https://www.blog.caishenpc.top "深的小栈"
<br>

8.  插入图片：(注意链接辨别标签没有区分大小写)  
    和插入链接一样，在链接的前面加入一个惊叹号！  
    行内式：  
    ![深的小栈的头像](https://www.blog.caishenpc.top/touxiang.jpg "头像")

    参考式：  
    ![深的小栈头像][id2]

    [id2]: https://www.blog.caishenpc.top/touxiang.jpg "头像"
<br>

9.  强调：  
    加粗使用一对**括起来   
    **加粗字体**  
    斜体使用一对*括起来  
    *斜体*  
<br>

10. 自动链接   
    直接使用尖括号括起一个链接可以实现快速链接  
    <https://www.google.com>  
    电子邮件也有类似的功能  
    <caishenpc@gmail.com>
<br><br>


11. 分割线：  
    使用三个以上`*`、`-`、`_`三种弄成一行  
    ***  
    ---
    ___   
<br>

12. 无序列表：  
    前面加`*`、`-`、`+`  ，列表同意可以实现嵌套关系
    * 第一项
    * 第二项
    * 第三项
<br><br>

13. 有序列表: (可以不用区别标号的，会自动从第一个标号开始编号)  
    >0. First
    >3. Then
    >9. Last

<br>

14. 反斜杠 `\` 可以实现转义 
<br><br>

15. 使用 `HTML` 来进行控制字体以及字体大小和颜色

    <font face="黑体">我是黑体字</font>  
    <font face="宋体">我是宋体字</font>  
    <font color=#0099ff size=3 face="黑体">我是蓝色的三号黑体</font>  
    <font color=gray size=3>灰色的字体</font>

    Size：规定文本的尺寸大小。可能的值：从 1 到 7 的数字。浏览器默认值是 3

<br>

16. 使用 `LaTex` 数学公式  
    
    行内公式：使用两个 `$` 符号引用公式: `$公式$`   
    行间公式：使用两对 `$$` 符号引用公式： `$$公式$$` 