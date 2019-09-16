
## 项目源码显示乱码 ##
设置工程编码为UTF-8
settings->editor->file Encodings->UTF-8

## 控制台,IDE本身 ##
修改IDE本身编码为UTF-8

```
# custom Android Studio VM options, see http://tools.android.com/tech-docs/configuration
-Dfile.encoding=UTF-8
```

## 中文输入框乱码 ##
sudo vim /opt/android-studio/bin/studio.sh 

```
export XMODIFIERS="@im=fcitx"
export GTK_IM_MODULE="fcitx"
export QT_IM_MODULE="fcitx"

```



