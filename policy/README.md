# 政策正文（GitHub 上的源文件）

这个目录是《隐私政策》和《用户协议》的**源文件**，发布地址是 GitHub Pages：

```
https://magpie0817.github.io/imag-privacy/policy/privacy.json
https://magpie0817.github.io/imag-privacy/policy/terms.json
```

iMag 服务器（`/api/policy`）会定期把这两个文件抄一份到自己的缓存里，客户端只找服务器要，
所以国内网络不需要能打开 GitHub。

## 改政策的流程

1. 改 JSON：`privacy.json`（隐私政策）或 `terms.json`（用户协议）。
   **一定要把 `version` 改成新日期**，客户端靠它判断"有没有更新"；
   `updated` 是页面上显示的"最后更新"，一般与 `version` 相同。
2. 提交并推送：

   ```bash
   cd Server/gh-pages
   git add policy/privacy.json policy/terms.json
   git commit -m "隐私政策：补充 xxx 说明（2026-12-01）"
   git push origin main
   ```

3. 等 GitHub Pages 生效（通常 1–2 分钟），浏览器打开上面那个 JSON 地址确认能看到新内容。
4. 服务器最多 10 分钟后会自动换成新版；想立刻生效就手动刷一次（需要后台密码）：

   ```
   http://154.12.39.49:8090/api/policy?doc=privacy&refresh=1&key=<ADMIN_PASSWORD>
   ```

5. 客户端下一次启动会提示"隐私政策已更新"，用户看过后同版本不再打扰。

## JSON 格式

```json
{
  "doc": "privacy",
  "title": "iMag 隐私政策",
  "version": "2026-11-07",
  "updated": "2026-11-07",
  "blocks": [
    { "k": "p", "t": "开头段落" },
    { "k": "h", "t": "1. 我们处理的信息" },
    { "k": "l", "t": "列表项（客户端会自动加 •）" }
  ]
}
```

- `k` 只有三种：`h` 小标题、`p` 段落、`l` 列表项。
- 不要写 HTML 标签，客户端是纯文本渲染。
- 保存为 UTF-8、不要带 BOM（带 BOM 服务器会尝试兼容，但别依赖它）。

## 其它

- 同一份 JSON 在服务器上还有一份兜底副本：`Server/php/policy/`，GitHub 完全拿不到时用它。
  平时不用手动同步，只有在首次部署或长期断网时才需要复制过去。
- 仓库若没开 GitHub Pages：Settings → Pages → Source 选 `Deploy from a branch`，分支 `main`、目录 `/ (root)`。
