>是以圣人处无为之事,行不言之教,万物作焉而不辞. 

想必你之前听过[Travis](!https://travis-ci.org),[Circleci](!https://circleci.com/). 这些是三方免费CI(continous intergation)支持Github网站, 本网站现在一直是用travis自动发布的Blog. 现在Github终于有自己的CI工具(Action)了.现在来体验一波.

<!--more-->


1. 创建一个Action [Repo](https://github.com/wxdlong/hello-action)
2. 点击Action创建第一个action   
![IO](/jpg/201908/start_action.png)
3. Action比其它CI人性化的地方就是你的第一个Action,他会给你列出一系列workflow模版. 选第一个SimpleWorkflow.     
![IO](/jpg/201908/simpleWorkflow.png)   

    >这里每一个Action就是一个workflow. 配置文件是YAML格式.创建在项目根目录的.workflow目录下.


4. 编辑Action: 左边文本编辑,右边文档说明. 一切这么自然.什么都不用改,点start commit.它就会自动Build. 这里我稍微更改了一下输出为Hello Action! 以增加成就感.  
![IO](/jpg/201908/editAction.png)

5. 查看Action结果. 从结果来看Action配置文件的含义.是不是很简单. 简单翻译一下: 当push代码时, 开始在ubuntu-latest上构建,步骤1-> checkout代码,2-> 运行单行脚本-输出`hello Wolrd!`,3-> 输出- `Hello Action!`
![IO](/jpg/201908/ActionRes.png)
    ```yaml
    name: CI

    on: [push]

    jobs:
      build:

        runs-on: ubuntu-latest
        
        steps:
        - uses: actions/checkout@v1
        - name: Run a one-line script
          run: echo Hello, world!
        - name: Run a multi-line script
          run: |
            echo Hello Action!
    ```


> 官方文档: https://help.github.com/en/articles/workflow-syntax-for-github-actions