---
title: "EOF in SHELL"
date: 2019-08-05T21:29:48+08:00
Keywords: ["EOF","here document"]
draft: false
---

　　SHELL中打印字符时用`echo`, 当打印N多行字符时，显然ECHO有点吃力。这时候就需要EOF上场了。

```bash
cat <<"EOF"
 /$$   /$$           /$$ /$$                 /$$$$$$$$  /$$$$$$  /$$$$$$$$
| $$  | $$          | $$| $$                | $$_____/ /$$__  $$| $$_____/
| $$  | $$  /$$$$$$ | $$| $$  /$$$$$$       | $$      | $$  \ $$| $$      
| $$$$$$$$ /$$__  $$| $$| $$ /$$__  $$      | $$$$$   | $$  | $$| $$$$$   
| $$__  $$| $$$$$$$$| $$| $$| $$  \ $$      | $$__/   | $$  | $$| $$__/   
| $$  | $$| $$_____/| $$| $$| $$  | $$      | $$      | $$  | $$| $$      
| $$  | $$|  $$$$$$$| $$| $$|  $$$$$$/      | $$$$$$$$|  $$$$$$/| $$      
|__/  |__/ \_______/|__/|__/ \______/       |________/ \______/ |__/      
EOF
```

<!--more-->

## Here Document
　　专业术语Here Document,是一种特殊的IO重定向，EOF=`End of File`。     
1. `EOF`作为分隔符代号，可以写成任意字符，只需要保证前后一样即可。    
    ```bash
    cat <<abcdefo
    Any char is
    OK!
    abcdefo
    ```

2. 当不需要转义，即想输出变量值是。去掉双引号。  下面输出`echo HELLO WORLD`
    ```bash
    VAR="HELLO WORLD"
    cat <<var
    echo ${VAR}
    var
    ```

3. 高级用法可以输出template文件。这们可以输出文件到`template.txt`。

    ```bash
    NAME=wxdlong
    AGE=18
    cat <<template>template.txt
    Hello ${NAME}
    Your age is ${AGE}
    template
    ``` 
4. 定义带换行的变量值。 `echo "${variable}"`试试看输出？

    ```bash
    variable=$(cat <<SETVAR
    This variable
    runs over multiple lines.
    SETVAR
    )
    ```
5. 当注释用. 

    ```bash
    : <<COMMENTBLOCK
    echo "This line will not echo."
    This is a comment line missing the "#" prefix.
    This is another comment line missing the "#" prefix.

    &*@!!++=
    The above line will cause no error message,
    because the Bash interpreter will ignore it.
    COMMENTBLOCK
    ```
6. 替换交互输入。

