https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz


### 初始安装
```
# 下载插件
pip install jupyter # -i https://pypi.tuna.tsinghua.edu.cn/simple/ --trusted-host pypi.douban.com
# 添加环境变量
export PATH="$PATH:/opt/Python-3.6.9/bin"
# 生成配置
jupyter notebook --generate-config
# 修改配置ip为机器的地址
vim /root/.jupyter/jupyter_notebook_config.py
c.NotebookApp.ip = '内网ip'
# 启动
nohup jupyter notebook --allow-root &
```
- 打开网页

http://120.79.36.202:8888?token=c55fb8b5cb1f2308bb603952fcd017995cc03f7d23979eba


### 安装插件
```
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user
pip install jupyter_nbextensions_configurator
jupyter nbextensions_configurator enable --user
``` 
重启jupyter, 进入主页标签页`Nbextensions`, 
- 勾选`hinterland` 启用代码自动补全
- 勾选`Select CodeMirror Keymap` 启用键盘,可选vim

### 安装主题
pip3 install jupyterthemes
```
# 查看可用的主题列表
jt -l
# 这个是设置主题
jt -t gruvboxd -T -N
# 这个是重置，即还原成默认的主题
jt -r
```

https://www.jianshu.com/p/a85bc2a8fa56
https://www.lefer.cn/posts/15473/
https://www.jianshu.com/p/8197845602b1
