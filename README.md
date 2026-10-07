# 花桥生活指南（huacao-site）

面向「花桥.cn」的 MVP 静态站点。纯 HTML/CSS/JS，无后端、无构建步骤，可直接推送到 GitHub Pages。

## 目录结构



```
huacao-site/

├── index.html        首页（定位 + 数据条 + 板块入口）

├── commute.html      通勤攻略（11号线花桥段三站、苏沪双11号线换乘）

├── directory.html    生活黄页（分类商家名录，示例数据 + 筛选）

├── join.html         商家入驻（免费/置顶/认证三档，示例定价）

├── about.html        关于与合规（数据来源、备案说明）

├── assets/

│   ├── style.css     全站样式（移动优先）

│   └── favicon.svg   站点图标

├── CNAME             内容为“花桥.cn”（GitHub Pages 自定义域名声明）

└── .nojekyll         阻止 GitHub Pages 执行 Jekyll
```

## 本地预览



```
python -m http.server 8080

\# 浏览器打开 http://localhost:8080
```

注意：`directory.html` 的商家数据内嵌在页面脚本中，直接双击打开（file://）也可用；不需要外部服务。

## 部署到 GitHub Pages（两步）

### 第 1 步：推送仓库



1. 在 GitHub 新建仓库，推荐名为 `huacao-cn`（公开仓库，Pages 免费）。

2. 把本目录全部文件推上去：



```
git init

git add .

git commit -m "花桥生活指南 MVP"

git remote add origin https://github.com/<你的用户名>/huacao-cn.git

git push -u origin main
```



1. 仓库 Settings → Pages → Source 选 `main` 分支 /root，Save。

### 第 2 步：绑定自定义域名



1. 仓库 Settings → Pages → Custom domain 填入 `花桥.cn` → Save。

   （仓库根目录已含 CNAME 文件，内容为 `花桥.cn`；在面板设置会自动生成 / 校验该文件。）

2. 勾选 Enforce HTTPS（GitHub 会自动签发证书，生效需几分钟到数小时）。

3. 在你的域名注册商 / DNS 托管商处添加解析记录：



| 主机记录  | 记录类型  | 记录值                 | 说明                             |
| ----- | ----- | ------------------- | ------------------------------ |
| `@`   | A     | `185.199.108.153`   | 根域（花桥.cn）指向 GitHub Pages 任一 IP |
| `@`   | A     | `185.199.109.153`   | 冗余 IP，建议四条全加                   |
| `@`   | A     | `185.199.110.153`   | 同上                             |
| `@`   | A     | `185.199.111.153`   | 同上                             |
| `www` | CNAME | `<你的用户名>.github.io` | www 子域指向你的 Pages 站点            |



* 若你的 DNS 托管商支持 CNAME 扁平化（ALIAS/ANAME），`@` 也可用 CNAME 指向 `<用户名>.github.io`，否则用 A 记录。

* DNS 生效通常需数分钟至 48 小时；之后访问 `https://花桥.cn` 与 `https://www.花桥.cn` 应均能打开站点。

## 备案与合规（重要）



* **域名实名**：.cn 域名需完成 CNNIC 实名认证（注册时一般已做，请确认状态）。

* **ICP 备案**：备案义务与境内服务器绑定。GitHub Pages 托管在境外，**无法备案**；大陆访问速度一般、稳定性受国际链路影响，且存在政策与可用性风险。当前版本仅作评估 / 试运行。

* **正式运营路径**：面向大陆用户正式运营时，将站点迁移至阿里云或腾讯云 OSS 静态托管 + CDN（月成本约几十元），完成 ICP 备案后再把 DNS 切换过去，切换不影响域名与内容。

* **广告合规**：付费收录 / 置顶均属广告推广，需按《互联网广告管理办法》标注 “广告”。

## 内容更新机制

纯静态站点无后台。建议流程：人工收集 / 核实 → 修改 `directory.html` 内数据或新增页面 → 提交推送 → Pages 自动发布。

如需自动化：可配置 GitHub Actions 定时构建（如每天抓取公开通勤数据生成静态页），MVP 阶段不建议引入。

## 数据说明

通勤数据（三站、日均 5–7 万人次、2013/2023 里程碑）来自新华网、央广网、嘉定区政府网、昆山轨交公司等公开报道，详见 `about.html`。黄页条目为示例数据，上线前需逐条核验替换。