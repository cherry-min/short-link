
![wechat2](https://img.shields.io/badge/微信公众号-科妙知行-brightgreen?style=flat-square)

## 介绍

一个使用 Cloudflare Pages 创建的 URL 缩短器

_Demo_ : [https://s.yoyou.org/](https://s.yoyou.org/)

### 利用 Cloudflare pages 部署

1. fork 本项目
2. 登录到[Cloudflare](https://dash.cloudflare.com/)控制台.
3. 在帐户主页中，选择`pages`> ` Create a project` > `Connect to Git`
4. 选择你创建的项目存储库，在`Set up builds and deployments`部分中，全部默认即可。
5. 点击`Save and Deploy`，稍等片刻，你的网站就部署好了。
6. 创建 D1 数据库参考[这里](https://github.com/x-dr/telegraph-Image/blob/main/docs/manage.md)
7. 执行 sql 命令创建表（在控制台输入框粘贴下面语句执行即可）

```sql
DROP TABLE IF EXISTS links;
CREATE TABLE IF NOT EXISTS links (
  `id` integer PRIMARY KEY NOT NULL,
  `url` text,
  `slug` text,
  `ua` text,
  `ip` text,
  `status` int,
  `create_time` DATE
);
DROP TABLE IF EXISTS logs;
CREATE TABLE IF NOT EXISTS logs (
  `id` integer PRIMARY KEY NOT NULL,
  `url` text ,
  `slug` text,
  `referer` text,
  `ua` text ,
  `ip` text ,
  `create_time` DATE
);

```

8. 选择部署完成 short 项目，前往后台依次点击`设置`->`函数`->`D1 数据库绑定`->`编辑绑定`->变量名称填写：`DB` 命名空间 `选择你提前创建好的D1` 数据库绑定

9. 重新部署项目，完成。

### API

#### 短链生成

```bash
# POST /create
curl -X POST -H "Content-Type: application/json" -d '{"url":"https://yoyou.org"}' https://s.yoyou.org/create

# 指定slug
curl -X POST -H "Content-Type: application/json" -d '{"url":"https://yoyou.org","slug":"casd23"}' https://s.yoyou.org/create

```

> response:

```json
{
  "slug": "<slug>",
  "link": "http://s.yoyou.org/<slug>"
}
```
