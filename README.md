# 番茄画布 · 本地版下载

本仓库只发布**构建产物**，用于本地运行与更新，不包含源码。源码仓库保持私有。

在线版不需要下载，直接访问 <https://hb.fqzlr.com>。

## 下载

- 最新版：<https://github.com/fqzlr/fq-canvas-dist/releases/latest>
- 文件名：`fq-canvas-local-v<版本号>.zip`
- 更新清单：`https://github.com/fqzlr/fq-canvas-dist/releases/latest/download/update.json`

下载后解压到任意目录，解压后的结构如下：

```text
fq-canvas-local-v0.00.0/
  VERSION
  start.bat                  双击启动
  create-desktop-shortcut.bat 创建桌面快捷方式
  stop.bat                   停止本地服务
  serve.ps1                  本地静态服务
  fq-canvas.ico              图标
  web/dist/                  页面文件
```

## 使用步骤

1. 解压压缩包，双击 `start.bat`；
2. 稍等 2 秒，浏览器会自动打开 `http://localhost:3031`；
3. 在页面右上角的配置里填写 OpenAI 兼容接口的 Base URL 与 API Key，即可开始使用。

关闭浏览器后本地服务仍在后台运行；不需要时双击 `stop.bat` 停止。

### 创建桌面快捷方式

双击 `create-desktop-shortcut.bat`，会在桌面生成名为「番茄画布」的快捷方式。之后双击桌面图标即可启动并打开页面。

> 快捷方式指向当前解压目录。如果把文件夹移动或改名，需要重新运行一次这个脚本。

### 手动启动（不使用脚本）

任何静态服务器都可以，注意**必须通过 HTTP 打开**，直接双击 `web\dist\index.html` 会白屏：

```bash
cd web/dist
python -m http.server 3031
# 或 npx serve -l 3031
```

然后访问 `http://localhost:3031`。

## 更新

1. 到 Releases 下载新版本压缩包；
2. 解压后，用新的 `web\dist` 覆盖旧目录里的 `web\dist`，并用新的 `VERSION` 覆盖旧的 `VERSION`；
3. 重启本地服务（`stop.bat` 后重新 `start.bat`），刷新页面即可。

`start.bat`、桌面快捷方式和 `stop.bat` 一般不需要替换；如果更新说明里提到启动器变更，请整体替换目录。

## 数据说明

画布、我的素材、生成记录和 API Key 默认保存在**浏览器本地**，不会上传到服务器，也不在本仓库中。

- 数据按「协议 + 端口」隔离，请保持使用 `http://localhost:3031`，换端口或换访问地址会看不到原有数据；
- 清理浏览器缓存、更换浏览器或使用无痕窗口会导致数据丢失，重要画布请先在应用内导出备份。

## 常见问题

**双击 start.bat 后浏览器没有打开**

手动访问 `http://localhost:3031`；如果仍然打不开，说明端口被占用，先运行 `stop.bat`。

**端口 3031 已被其他程序占用**

`start.bat` 会直接打开已占用该端口的页面。请先关闭占用程序，或修改 `serve.ps1` 与 `start.bat` 中的端口（两端需一致）。改端口后浏览器数据会另起一份。

**页面白屏**

通常是直接双击了 `index.html`。请通过 `start.bat` 或任意 HTTP 服务访问。

**系统提示无法运行 PowerShell 脚本**

`start.bat` 已带 `-ExecutionPolicy Bypass`。若仍被拦截，检查安全软件是否阻止了脚本执行，或将解压目录加入信任列表。

**杀毒软件报警**

本地服务只是在本机 `127.0.0.1` 上读取 `web/dist` 静态文件，不联网、不写注册表。若被拦截，请将目录加入白名单。

## 更新清单格式

`update.json` 供本地启动器检查更新：

```json
{
  "version": "v0.00.0",
  "publishedAt": "2026-11-04",
  "notes": "fq-canvas v0.00.0",
  "repository": "https://github.com/fqzlr/fq-canvas-dist",
  "downloadUrl": "https://github.com/fqzlr/fq-canvas-dist/releases/download/v0.00.0/fq-canvas-local-v0.00.0.zip",
  "patch": {
    "url": "https://github.com/fqzlr/fq-canvas-dist/releases/download/v0.00.0/fq-canvas-local-v0.00.0.zip",
    "sha256": "…",
    "size": 12345678
  }
}
```
