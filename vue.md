## 1、环境准备

### 1、阿里云环境

[2024最新版Node.js下载安装及环境配置教程【保姆级】_nodejs下载-CSDN博客](https://blog.csdn.net/WHF__/article/details/129362462)

镜像源修改为阿里云，  C盘下面.nxx什么文件，也可以进行修改，但是命令行修改后，检测下是否有效，感觉更可靠。

环境配置很重要的， 上面那个链接是非常全面的，主要的还是**cnpm的安装。**

### 2、下载vue

cnpm install -g @vue/cli 

使用cnpm下载很快， 之后输入  vue --version  输出版本的话，就可以了

而且 注意是-g  全局安装， 这样在全局环境下，都可以使用vue命令了

### 3、使用vue创建项目

vue create todo-app   直接在shell中输入这个就行，

*windows 的shell中 输入 e:  就会切换到e盘路径下， 之后使用cmd就可以了*

cd todo-app
npm run serve  这两条命令是可以直接执行的，

当然了，如果需要打开VScode编辑器的话，直接code .  就打开了。

### 4、运行下 检测是否出错

npm run server就行了，

这个核心其实还是![image-20241110131501016](E:\ANJUOtherFiles\TyporaPics\image-20241110131501016.png)

这个package.json就是一个管理自动化的脚本，便捷 高效。  vue会自动创建这些东西。

## 2、学习资源

### 1、chatgpt

重点是合适的提问方式， 它会给出很好的答案。

而且  40-carve代码写的是真的漂亮，漂亮极了

学着 问着，

给的例子**，“麻雀虽小，五脏俱全”**

### 2、官方教程

## 3、代码运行逻辑

![image-20241110132212733](E:\ANJUOtherFiles\TyporaPics\image-20241110132212733.png)

public文件夹下面有个index.html文件

![image-20241110132240699](E:\ANJUOtherFiles\TyporaPics\image-20241110132240699.png)

那个文件有这个部分，

然后从main.js文件开始运行

![image-20241110132323992](E:\ANJUOtherFiles\TyporaPics\image-20241110132323992.png)

这是里面的几行代码， 可以看到先是引入了一些内容，这当成“代码块”，“模块”就行。

然后从第4行开始执行，

App这是一个“模块”“模板”的名字，

![image-20241110132527996](E:\ANJUOtherFiles\TyporaPics\image-20241110132527996.png)

在*App.vue*这个文件中， 将这个模块导出为App， 所以*createApp(App).mount('#app')*， 就是创建这么一个模板，然后挂载到index.html文件的*app DOM*下。

## 4、核心语法



![image-20241110132136040](E:\ANJUOtherFiles\TyporaPics\image-20241110132136040.png)

