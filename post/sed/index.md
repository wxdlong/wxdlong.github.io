

- [基础语法](#%E5%9F%BA%E7%A1%80%E8%AF%AD%E6%B3%95)
- [替换sed默认匹配符`/`, 任意字符前转义如@= `\@`, &= `\#`](#%E6%9B%BF%E6%8D%A2sed%E9%BB%98%E8%AE%A4%E5%8C%B9%E9%85%8D%E7%AC%A6-%E4%BB%BB%E6%84%8F%E5%AD%97%E7%AC%A6%E5%89%8D%E8%BD%AC%E4%B9%89%E5%A6%82)
- [`&` 引用匹配变量](#%E5%BC%95%E7%94%A8%E5%8C%B9%E9%85%8D%E5%8F%98%E9%87%8F)
- [打印行数](#%E6%89%93%E5%8D%B0%E8%A1%8C%E6%95%B0)
- [大小写替换](#%E5%A4%A7%E5%B0%8F%E5%86%99%E6%9B%BF%E6%8D%A2)
- [圆括号匹配](#%E5%9C%86%E6%8B%AC%E5%8F%B7%E5%8C%B9%E9%85%8D)
- [合并两行内容](#%E5%90%88%E5%B9%B6%E4%B8%A4%E8%A1%8C%E5%86%85%E5%AE%B9)
- [打印行数](#%E6%89%93%E5%8D%B0%E8%A1%8C%E6%95%B0-1)
- [删除重复行](#%E5%88%A0%E9%99%A4%E9%87%8D%E5%A4%8D%E8%A1%8C)
- [删除所有空行](#%E5%88%A0%E9%99%A4%E6%89%80%E6%9C%89%E7%A9%BA%E8%A1%8C)
- [压缩文件中的连续空行为一行，包括文件开头和结尾（类似cat -s）](#%E5%8E%8B%E7%BC%A9%E6%96%87%E4%BB%B6%E4%B8%AD%E7%9A%84%E8%BF%9E%E7%BB%AD%E7%A9%BA%E8%A1%8C%E4%B8%BA%E4%B8%80%E8%A1%8C%E5%8C%85%E6%8B%AC%E6%96%87%E4%BB%B6%E5%BC%80%E5%A4%B4%E5%92%8C%E7%BB%93%E5%B0%BE%E7%B1%BB%E4%BC%BCcat--s)
- [每五行后添加一行空行](#%E6%AF%8F%E4%BA%94%E8%A1%8C%E5%90%8E%E6%B7%BB%E5%8A%A0%E4%B8%80%E8%A1%8C%E7%A9%BA%E8%A1%8C)
- [`addr, +N` 从匹配行，到+N行。](#addr-n-%E4%BB%8E%E5%8C%B9%E9%85%8D%E8%A1%8C%E5%88%B0n%E8%A1%8C)
- [`addr, ~N` 从匹配行及~N行。](#addr-n-%E4%BB%8E%E5%8C%B9%E9%85%8D%E8%A1%8C%E5%8F%8An%E8%A1%8C)
- [`addr1, addr2` 从第一个匹配行至第二个匹配行](#addr1-addr2-%E4%BB%8E%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%8C%B9%E9%85%8D%E8%A1%8C%E8%87%B3%E7%AC%AC%E4%BA%8C%E4%B8%AA%E5%8C%B9%E9%85%8D%E8%A1%8C)
- [多个命令集合`{}`](#%E5%A4%9A%E4%B8%AA%E5%91%BD%E4%BB%A4%E9%9B%86%E5%90%88)
- [反序内容 `sed '1!G;h;$!d'`](#%E5%8F%8D%E5%BA%8F%E5%86%85%E5%AE%B9-sed-1ghd)


## 基础语法

 命令 | 功能 
 --- | ---
 `a\`  |  在当前行后添加一行或多行。多行时除最后一行外，每行末尾需用“\”续行
 `c\`  |  用此符号后的新文本替换当前行中的文本。多行时除最后一行外，每行末尾需用"\"续行
 `i\`  |  在当前行之前插入文本。多行时除最后一行外，每行末尾需用"\"续行
 `d`  |  删除行
 `h`  |  把模式空间里的内容<font color="red">**复制**</font>到暂存缓冲区
 `H`  |  把模式空间里的内容<font color="red">**追加**</font>到暂存缓冲区
 `g`  |  把暂存缓冲区里的内容<font color="red">**复制**</font>到模式空间，覆盖原有的内容
 `G`  |  把暂存缓冲区的内容<font color="red">**追加**</font>到模式空间里，追加在原有内容的面
 `l`  |  列出非打印字符
 `p`  |  打印行
 `=`  |  打印当前行号
 `N`  |   读入下一输入行，并从下一条命令而不是第一条命令开始对其的处理
 `q`  |  结束或退出sed
 `r`  |  从文件中读取输入行
 `!`  |   对所选行以外的所有行应用命令
 `s`  |   用一个字符串替换另一个
 `g`  |   在行内进行全局替换
 `w`  |   将所选的行写入文件
 `x`  |   交换暂存缓冲区与模式空间的内容
 `y`  |   将字符替换为另一字符（不能对正则表达式使用y命令）

> 以下例子中引用的**文本**  

```text
<html>
 <title>Hello sed</title>
 <Switch>
     <username>admin</username>
     <password>482@@lflfdiow</password>
 </Switch>
 <Hello>
     <username>postgres</username>
     <password>hello</password>
 </Hello>
</html>
```

## 替换sed默认匹配符`/`, 任意字符前转义如@= `\@`, &= `\#`
1. 分隔符替换为`@`, 打印匹配`</Switch>`行  
   
   ```bash
    [root@pgmaster home]# sed -n '\@</Switch>@p' test.xml
    </Switch>
   ```
2. 替换@@为##， 紧接着s后面的分隔符不用转义.  
   
   ```bash
    sed 's&@@&##&g' test.xml
    <html>
    <title>Hello sed</title>
    <Switch>
        <username>admin</username>
        <password>482##lflfdiow</password>
    </Switch>
    </html>
   ```
   
## `&` 引用匹配变量
> 在`word.*word`前后加`[]`
```bash
[root@pgmaster home]# sed 's@word.*word@[&]@g' test.xml
<html>
 <title>Hello sed</title>
 <Switch>
     <username>admin</username>
     <pass[word>482@@lflfdiow</password]>
 </Switch>
 <Hello>
     <username>postgres</username>
     <pass[word>hello</password]>
 </Hello>
</html>
```
## 打印行数
```
[root@pgmaster home]# sed -n '$=' test.xml
11
```

## 大小写替换
> 使用\u表示大写，\l表示小写

1.  把每个单词的第一个小写字母变大写：  
`sed 's/\b[a-z]/\u&/g' filename`   
2. 把所有小写变大写：   
`sed 's/[a-z]/\u&/g' filename`   
3. 大写变小写：   
`sed 's/[A-Z]/\l&/g' filename`  

## 圆括号匹配
>\1, \2分别匹配第一，第二个  

```bash
[root@pgmaster home]# sed 's@word>\(.*\)<\(.*\)word@\1-------\2@g' test.xml
<html>
 <title>Hello sed</title>
 <Switch>
     <username>admin</username>
     <pass482@@lflfdiow-------/pass>
 </Switch>
 <Hello>
     <username>postgres</username>
     <passhello-------/pass>
 </Hello>
</html>
```

## 合并两行内容

```bash
[root@pgmaster home]# sed '$!N;s/\n/ /' test.xml
<html>  <title>Hello sed</title>
 <Switch>      <username>admin</username>
     <password>482@@lflfdiow</password>  </Switch>
 <Hello>      <username>postgres</username>
     <password>hello</password>  </Hello>
</html>
```


## 打印行数

```bash
[root@pgmaster home]# sed '=' test.xml | sed 'N;s/s*\ns*/\t/'
1       <html>
2        <title>Hello sed</title>
3        <Switch>
4            <username>admin</username>
5            <password>482@@lflfdiow</password>
6        </Switch>
7        <Hello>
8            <username>postgres</username>
9            <password>hello</password>
10       </Hello>
11      </html>
```

## 删除重复行

```bash

[root@pgmaster home]# cat cf.log    
aaaa                                
aaaa                                
bb                                  
bbb                                 
cccc                                
cccc                                
def                                 
ahis                                

[root@pgmaster home]# sed -n '$!N;/^\(.*\)\n\1$/!P;D' cf.log
aaaa
bb
bbb
cccc
def
ahis
```

## 删除所有空行

`sed '/^$/d' test.txt`
或者
`sed '/./!d' test.txt`  
> 解释：都比较简单，第一个匹配空行，第二个匹配非空行。  

## 压缩文件中的连续空行为一行，包括文件开头和结尾（类似cat -s）

`sed '/^$/N;/\n$/D' test.txt`


## 每五行后添加一行空行

`sed 'n;n;n;n;G'`  
或者

`sed '0~5G'` test.txt
>解释：很简单，每次读5行，再添加一个空行。第二种方法利用了GNU Sed的功能 addr~step，查看man sed  


## `addr, +N`   从匹配行，到+N行。  
>打印<Switch>及以下2行内容   
   
```bash
[root@pgmaster home]# sed -n '\@<Switch>@,+2 p' test.xml
<Switch>
    <username>admin</username>
    <password>482@@lflfdiow</password>
```
    
## `addr, ~N`   从匹配行及~N行。
>打印</Switch>至2行内容   
   
```bash
[root@pgmaster home]# sed -n '\@</Switch>@,~2 p' test.xml
<Switch>
    <username>admin</username>
```
 
## `addr1, addr2` 从第一个匹配行至第二个匹配行
>打印两个匹配行之间的内容  
```bash
[root@pgmaster home]# sed -n '\@<Switch>@,\@</Switch>@ p' test.xml
 <Switch>
     <username>admin</username>
     <password>482@@lflfdiow</password>
 </Switch>
```

## 多个命令集合`{}`
>在`Switch`之间
>替换`<username>admin</username>` 为<username>root</username>  
>在`password`行下增加`<type>lock</type>`  
>`c\`, `a\` ,其中`\`是转义空格。

```bash
[root@pgmaster home]# sed  '\@<Switch>@,\@</Switch>@{
/username/c\     <username>root</username>
/password/a\     <type>lock</type>
}' test.xml
<html>
 <title>Hello sed</title>
 <Switch>
     <username>root</username>
     <password>482@@lflfdiow</password>
     <type>lock</type>
 </Switch>
 <Hello>
     <username>postgres</username>
     <password>hello</password>
 </Hello>
</html>
```


## 反序内容 `sed '1!G;h;$!d'`
```bash
[root@pgmaster home]# sed '1!G;h;$!d' test.xml
</html>
 </Hello>
     <password>hello</password>
     <username>postgres</username>
 <Hello>
 </Switch>
     <password>482@@lflfdiow</password>
     <username>admin</username>
 <Switch>
 <title>Hello sed</title>
<html>
```


