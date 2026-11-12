# 国家队重仓 ETF 与股指期货空头跟踪看板

**只需上传 1 个文件**：`index.html`（约 1.3 MB，ECharts 图表库和当日数据快照已全部内嵌）。
`README.md` 只是本说明，不上传也不影响页面。

## 为什么不会再出现「数据加载失败」

页面打开后**立即用内嵌数据渲染**图表和统计表，不请求任何接口、不依赖 `data.js`、不依赖 `?sharecode=`、不依赖 CDN。渲染完成后才在后台异步尝试同步在线最新数据；同步失败会静默保留内嵌快照，并在页面底部明确提示「在线同步暂不可用，当前显示内嵌快照」。因此无论本地双击打开、GitHub Pages、还是任何静态托管，都能完整显示。

## 发布步骤（全程网页操作）

1. 打开 https://github.com/new ，登录 `redblue1981`
2. `Repository name` 填 `etf-national-team-dashboard`，选 **Public**，点 **Create repository**
3. 点 **Add file → Upload files**，把本目录下的 `index.html`（可选加 `README.md`）拖进上传框
4. 点绿色按钮 **Commit changes**
5. **Settings → Pages → Deploy from a branch → Branch 选 `main` / `(root)` → Save**
6. 约 1–2 分钟后访问 https://redblue1981.github.io/etf-national-team-dashboard/

## 每天怎么自动更新

页面加载时会向在线服务请求当天最新数据（带 sharecode 的跨域接口，网关已允许），成功即覆盖内嵌快照，之后每 30 分钟重试一次。在线服务每天 **16 / 19 / 22 点** 自动抓取上交所 ETF 份额、新浪财经收盘价、中金所股指期货持仓并重算。

所以**上传一次即可，之后每天打开都是最新数据**，不需要 GitHub Actions，也不需要重新传文件。

在线动态版（数据最及时）：
https://ad009d399f724cf23.app.workbuddy.host/?sharecode=BUUGQDZD4G

## 数据来源与口径

- **ETF 份额**：上海证券交易所每日基金份额，通常 T+1 发布
- **收盘价**：新浪财经 ETF 日线
- **股指期货**：中金所每日持仓排名（IF / IH / IC / IM 四品种，前 20 名会员持卖单量）

国家队（中央汇金、证金等）具体持仓只在基金季报/半年报披露，无日度公开数据。本页以「ETF 全市场基金份额 × 收盘价」作为持有额度代理指标，以份额日变动作为净申购/净赎回。份额未发布的交易日按最近一次份额估算，主图末端用浅色区间标注，正式发布后自动替换为真实值；顶部「数据源状态」条分别列出三个数据源的最新日期。

## 可选：手动刷新内嵌快照

完整源码仓库提供 `fetch_data.py`、`fetch_futures.py`、`build_site.py`。在装有 Python 3.11 + akshare 的机器上依次运行，再把生成的 `index.html` 重新上传覆盖即可。日常使用不需要这一步。
