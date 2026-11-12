# NexusBot

个人数字档案 · 单文件静态站点（构建产物仓库）。

- `index.html` — 公开版：仅含已批准公开条目，可直接浏览
- `vault/index.html` — 加密档案版：AES-256-GCM 密文 + 密码门，口令不在本仓库内

本仓库仅存放构建产物与发布页，不含源数据、不含任何私密信息。

## 在线访问

| 页面 | 地址 | 说明 |
| --- | --- | --- |
| 公开档案 | <https://xuange-hu.github.io/NexusBot/> | 直接可读 |
| 加密档案 | <https://xuange-hu.github.io/NexusBot/vault/> | 需要口令；密文在页面内，解密在浏览器本地完成 |

两页都托管在 GitHub Pages 上。`.nojekyll` 的作用是让 Pages 原样发布，不走 Jekyll —— 否则下划线开头的路径会被当成静态站模板处理。

## 本地预览

没有构建步骤，任意静态服务器都可以：

```bash
python -m http.server 8000
# 公开版  http://127.0.0.1:8000/
# 加密版  http://127.0.0.1:8000/vault/
```

直接双击 `index.html` 也能看，但加密版依赖 Web Crypto（`crypto.subtle`）：多数浏览器只在
`https://` 与 `http://localhost` 这类安全上下文里暴露它，`file://` 下会拿不到。

## 结构

```
.
├── index.html        公开版：单文件，内联 CSS / JS / 图标 / manifest
├── vault/
│   └── index.html    加密版：密文 + 口令门，同样单文件
├── .nojekyll         让 Pages 原样发布
└── README.md
```

单文件是刻意的：零外部请求、零 CDN，CSP 里的 `default-src 'self'` 才能真正收紧，离线打开也不缺样式。

## 隐私

- 公开版只收录已批准公开的条目，内容在构建时就已确定，不存在"前端隐藏"。
- 加密版：明文不进仓库；页面里只有密文与解密逻辑，口令由使用者输入，解密在本地内存中完成。
- 加密版带 `noindex,nofollow,noarchive` 与 `referrer: no-referrer`，不被搜索引擎收录，也不把来源页带出去。
- 仓库历史中没有源数据，也没有口令 —— 这是这个仓库能公开的前提。

## 说明

这是构建产物的镜像仓库。内容的修改在生成端完成，本仓库不直接接受对 `index.html` /
`vault/index.html` 的编辑：下次发布会覆盖它们。
