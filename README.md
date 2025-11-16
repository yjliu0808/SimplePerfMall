# SimplePerfMall

# 测试简单的mall项目实现CICD



## 1、github新建仓库 ：**[SimplePerfMall](https://github.com/yjliu0808/SimplePerfMall)**

 SSH地址：git@github.com:yjliu0808/SimplePerfMall.git

## 3、新增“**网络钩子**Webhook

github仓库 ：**[SimplePerfMall](https://github.com/yjliu0808/SimplePerfMall)** -> [Settings](https://github.com/yjliu0808/SimplePerfMall/settings) ->[Webhooks /](https://github.com/yjliu0808/SimplePerfMall/settings/hooks) Add webhook
可填写如下信息：

Payload URL * ：http://129.28.122.208:8081/github-webhook/

Content type *：application/x-www-form-urlencoded

Secret ：不用填写

------

### SSL verification

- [x]  Enable SSL verification 

------

### **Which events would you like to trigger this webhook?**

- [x] Just the `push` event.

------

- [x] ActiveWe
   will deliver event details when this hook is triggered.
   当这个 Webhook 被触发时，我们会发送事件的详细信息。

### jenkins新建任务

填写以下内容：
输入一个任务名称：SimplePerfMall
Select an item type:流水线

### General

描述：测试简单的mall项目实现CICD

- [x] GitHub 项目
  项目 URL：https://github.com/yjliu0808/SimplePerfMall

### Triggers

- [x] GitHub hook trigger for GITScm polling

### 流水线

定义

- [x] **Pipeline scriptPipeline script from SCM**

  SCM

- [x] **Git**

Repository URL ：**git@github.com:yjliu0808/SimplePerfMall.git**


Credentials：**git (从文件读取，GitHub SSH Key for TESTCICD)**

Branches to build

指定分支（为空时代表any）：***/main**

脚本路径:**Jenkinsfile**

- [x] 轻量级检出

**==填写以上信息save即可**

### 本地机器执行：

```
git clone git@github.com:yjliu0808/SimplePerfMall.git
```

根目录下新建文件：Jenkinsfile
内容：

```
pipeline {
    agent any

    triggers {
        githubPush()   // 👈 加上这一段
    }

    stages {
        stage('Triggered') {
            steps {
                echo '🎉 Jenkins CI/CD 已被 GitHub Push 成功触发!！'
            }
        }
    }
}

```

```
git add .
git commit -m "检查CICD流程是否正常"
git push
```

==========若没有自动构建，jenkins：首次先手动构建。

### 其他相关操作备注

服务器命令：

```
jmeter -n -t D:\repository\SimplePerfMall\SimplePerfMall.jmx ^
-l D:\repository\SimplePerfMall\result.jtl ^
-e -o D:\repository\SimplePerfMall\report_2025_04_11

```

更新服务器
