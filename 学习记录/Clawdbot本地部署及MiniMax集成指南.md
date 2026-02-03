# Clawdbot本地部署及MiniMax集成指南

## 一、产品简介

### 1.1 项目概述

Clawdbot（现更名为Moltbot）是由PSPDFKit创始人Peter Steinberger开发的开源个人AI助理项目。项目在GitHub发布后迅速获得广泛关注，数周内星标数量突破6万+。

### 1.2 名称变更

- **原名称**：Clawdbot

- **新名称**：Moltbot（取自"龙虾蜕壳"，象征持续进化）

- **变更时间**：2025年1月27日

- **变更原因**：避免与Anthropic的"Claude"名称混淆

## 二、系统要求

### 2.1 硬件要求

- **推荐设备**：Mac mini或类似低功耗设备

- **运行时间**：支持24小时持续运行

- **存储空间**：至少10GB可用空间

### 2.2 软件要求

- **操作系统**：CentOS 8/9或Ubuntu 20.04+

- **Node.js版本**：v22或更高版本

- **内存**：至少4GB RAM

## 三、环境准备

### 3.1 安装Node.js

bash

 #1. 更新系统包管理器

sudo yum update -y

 #2. 启用EPEL仓库

sudo yum install -y epel-release

 #3. 安装NodeSource仓库（Node.js 22）

curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -

 #4. 安装Node.js

sudo yum install -y nodejs

 #5. 验证安装

node --version
npm --version

**注意事项**：

- CentOS 7不支持Node.js 22

- VMware用户如无CentOS 9选项，可选择CentOS 8

## 四、Clawdbot安装与配置

### 4.1 下载安装脚本

bash

#使用官方一键安装脚本

curl -fsSL https://clawd.bot/install.sh | bash

### 4.2 项目结构说明

安装完成后，主要目录结构：

/root/.clawdbot/     # 配置文件目录
/usr/lib/node_modules/clawdbot/  # 主程序目录

## 五、MiniMax API集成

### 5.1 注册MiniMax账号

1. 访问 [platform.minimaxi.com](https://platform.minimaxi.com/)

2. 创建账号并登录

3. 进入API密钥管理页面

### 5.2 获取API密钥

1. 在MiniMax控制台创建新应用

2. 生成API Key

3. 复制并保存API密钥（格式如：`sk-api-xxxxxxxxxxxx`）
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-49-29-image.png)
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-49-36-image.png)

### 5.3 测试API连接

bash

#测试MiniMax API连通性

curl -s -X POST 'https://api.minimax.chat/v1/chat/completions' \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "MiniMax-M2.1",
    "messages": [
      {"role": "user", "content": "Hello"}
    ]
  }'

## 六、依赖安装

### 6.1 安装Node.js依赖

bash

npm install ws zod

### 6.2 安装Signal CLI

bash

#1.下载Signal CLI

wget https://github.com/AsamK/signal-cli/releases/download/v0.13.23/signal-cli-0.13.23.tar.gz

#2.解压文件

tar -xzf signal-cli-0.13.23.tar.gz

#3.移动到合适位置

sudo mv signal-cli-0.13.23 /opt/signal-cli

## 七、配置向导

### 7.1 启动配置向导

bash

clawdbot onboard

### 7.2 配置步骤

1. **选择语言模型提供商**：选择MiniMax

2. **输入API密钥**：粘贴从MiniMax获取的API Key

3. **配置消息网关**：选择Signal或Telegram

4. **设置本地存储路径**：默认`/root/.clawdbot/data`

5. **完成配置**：确认所有设置
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-52-57-image.png)
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-53-09-image.png)
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-53-18-image.png)
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-53-25-image.png)
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-53-30-image.png)
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-53-37-image.png)
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-53-43-image.png)
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-53-50-image.png)
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-53-55-image.png)
   
   <img src="file:///C:/Users/Administrator/AppData/Roaming/marktext/images/2026-01-30-14-54-03-image.png" title="" alt="" width="608">

## 八、运行与测试

### 8.1 启动服务

bash

cd /root/.clawdbot
node /usr/lib/node_modules/clawdbot/dist/entry.js gateway 2>&1

### 8.2 验证运行状态

bash

#检查进程是否运行

ps aux | grep clawdbot

#检查端口监听

netstat -tlnp | grep 18789

### 8.3 测试对话功能

1. 通过配置的消息应用（如Signal）发送消息

2. 验证是否收到AI回复

3. 检查本地日志文件

## 九、Windows客户端访问

### 9.1 SSH端口转发

powershell

#在Windows PowerShell中执行

ssh -N -L 18789:127.0.0.1:18789 root@服务器IP地址

![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-55-24-image.png)

### 9.2 浏览器访问

1. 打开浏览器

2. 访问 `http://localhost:18789`

3. 查看Web管理界面
   
   ![](C:\Users\Administrator\AppData\Roaming\marktext\images\2026-01-30-14-55-32-image.png)

## 十、故障排除

### 10.1 常见问题

#### 问题1：机器人无响应

bash

#检查MiniMax API余额

curl -s -X POST 'https://api.minimax.chat/v1/chat/completions' \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model":"MiniMax-M2.1","messages":[{"role":"user","content":"Hi"}]}'

 可能原因：

- API密钥无效

- MiniMax账户余额不足

- 网络连接问题

#### 问题2：依赖安装失败

bash

#清理npm缓存并重新安装

npm cache clean --force
rm -rf node_modules package-lock.json
npm install

#### 问题3：端口冲突

bash

#修改服务端口

cd /root/.clawdbot
vi config.json  # 修改端口号

### 10.2 日志查看

bash

#查看实时日志

tail -f /root/.clawdbot/logs/app.log

#查看错误日志

tail -f /root/.clawdbot/logs/error.log

## 十一、数据管理

### 11.1 数据存储位置

- **对话记录**：`/root/.clawdbot/data/conversations/`

- **配置文件**：`/root/.clawdbot/config.json`

- **知识库**：`/root/.clawdbot/data/knowledge/`

### 11.2 备份与恢复

bash

#备份数据

tar -czf clawdbot-backup-$(date +%Y%m%d).tar.gz /root/.clawdbot/

#恢复数据

tar -xzf clawdbot-backup.tar.gz -C /

## 十二、高级配置

### 12.1 自定义技能插件

bash

#插件目录

cd /root/.clawdbot/plugins/

#创建自定义插件

mkdir my-plugin
cd my-plugin
npm init -y

### 12.2 多模型支持

#修改`config.json`支持多个AI模型：

json

{
  "ai_providers": {
    "minimax": {
      "api_key": "your-minimax-key",
      "model": "MiniMax-M2.1"
    },
    "openai": {
      "api_key": "your-openai-key",
      "model": "gpt-4"
    }
  }
}

## 十三、安全建议

### 13.1 安全配置

1. **修改默认端口**：避免使用18789默认端口

2. **启用防火墙**：限制访问IP

3. **使用HTTPS**：配置SSL证书

4. **定期更新**：保持软件最新版本

### 13.2 API密钥保护

bash

#使用环境变量存储API密钥

export MINIMAX_API_KEY="your-api-key"

## 十四、性能优化

### 14.1 内存优化

bash

#调整Node.js内存限制

node --max-old-space-size=4096 entry.js

### 14.2 启动脚本

#创建systemd服务：

ini

[Unit]
Description=Clawdbot AI Assistant
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root/.clawdbot
ExecStart=/usr/bin/node /usr/lib/node_modules/clawdbot/dist/entry.js gateway
Restart=always

[Install]
WantedBy=multi-user.target

## 十五、更新与维护

### 15.1 检查更新

bash

#更新Clawdbot

npm update -g clawdbot

#更新依赖

cd /root/.clawdbot
npm update

### 15.2 版本回滚

bash

#安装特定版本

npm install -g clawdbot@版本号

---

## 附录

### A. 常用命令速查

bash

#启动服务

clawdbot start

#停止服务

clawdbot stop

#查看状态

clawdbot status

#重新配置

clawdbot reconfigure

### B. 技术支持

- **GitHub仓库**：[https://github.com/clawdbot/clawdbot]()

- **官方文档**：[https://docs.clawdbot.com](https://docs.clawdbot.com/)

- **社区论坛**：[https://community.clawdbot.com](https://community.clawdbot.com/)

### C. 注意事项

1. 确保服务器时间同步

2. 定期备份重要数据

3. 监控API使用量和费用

4. 关注项目更新和安全公告
