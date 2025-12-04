在render部署veloera并数据持久化存储


1.部署veloera(render平台)
ghcr.io/veloera/veloera:latest

保活

2.连接数据库
无论使用哪种外部数据库，在 Render 的配置都是一样的：

### 必需配置


# 数据库连接（核心配置）

SQL_DSN=postgresql://用户名:密码@主机地址:端口/数据库名?sslmode=require

# 会话密钥

SESSION_SECRET=your-random-32-character-secret-key




3.查看应用日志

1. Render Dashboard → 你的服务 → **Logs**
2. 查找数据库连接日志：


✓ 正在连接数据库...
✓ 数据库连接成功
✓ Ping database: 
OK
✓ 初始化数据库表...

