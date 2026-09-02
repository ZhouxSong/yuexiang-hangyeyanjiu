# 方法论操作手册（Methodology）

## 一、检索话术模板（分模块）

### 监管政策
- 中国：`国家药监局 NMPA 人工智能医疗器械 三类证 获批数量 <年份>`、`国务院 人工智能+ 行动 医疗健康 实施意见 <年份>`、`国家医保局 人工智能 辅助诊断 医疗服务价格 立项 指南 <年份>`
- 美国：`FDA AI/ML-enabled medical devices total number <年份> list`、`FDA draft guidance artificial intelligence drug development <年份> lifecycle`
- 欧盟：`EU AI Act healthcare high-risk medical devices implementation timeline <年份>`

### 市场规模（必须多口径）
- `中国 <行业> 市场规模 预测 <年份> 亿元`（国内咨询口径）
- `global <industry> market size forecast <年份>`（海外口径）
- 同一指标至少收集2个机构口径，差异超过30%时必须在报告正文并列呈现并说明选择逻辑。

### 学术文献
- `Nature Medicine large language model clinical trial <年份> review`
- `AI drug discovery review Nature Reviews Drug Discovery <年份> clinical pipeline`
- 文献条目必须记录：作者（第一作者+et al.）、期刊、年份、卷期页码（能查到才写，查不到标"待核"）、核心结论一句话。

### 上市公司（财务连续化）
- 年报：`<公司名> <代码> <年份>年 年度业绩 收入 净利润`
- 中期：`<公司名> <代码> <年份>年中期业绩 上半年 收入`（freshness 用 m3-m6）
- 美股：`<ticker> Q<季度> <年份> results revenue net loss`
- 港股盈利预告：`<公司名> 盈利预告 正面盈利预告 <年份>`

### 非上市公司融资
- `<公司名> funding valuation <年份>`、`<公司名> 融资 <轮次> 估值 <年份>`

## 二、字数核算脚本（Word口径）

组装全文Markdown后，用以下脚本核算是否达标（中文字符+中文标点+英文词+数字串）：

```python
import re
with open('final_draft.md','r',encoding='utf-8') as f:
    t = f.read()
lines = t.split('\n')
body = []
for line in lines:
    s = line.strip()
    if re.match(r'^[|\s:]+$', s): continue      # 表格分隔行
    s = re.sub(r'^#+\s*', '', s)                # 标题井号
    s = re.sub(r'^>\s*', '', s)                 # 引用
    s = re.sub(r'^[-*]\s+', '', s)              # 列表
    s = s.replace('**','').replace('~~','')
    body.append(s)
body_text = '\n'.join(body)
cjk = len(re.findall(r'[\u4e00-\u9fff]', body_text))
cjk_punct = len(re.findall(r'[\u3000-\u303f\uff00-\uffef\u2014\u2018\u2019\u201c\u201d]', body_text))
en_words = len(re.findall(r'[A-Za-z]+', body_text))
digits = len(re.findall(r'[0-9]+', body_text))
print('Word字数口径:', cjk + cjk_punct + en_words + digits)
```

经验值：3万字报告 ≈ 可见字符4万左右；每次补写扩展节后必须重新组装核算。

## 三、图表制作规范（matplotlib）

### 全局样式
```python
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt
from matplotlib import font_manager
for f in ['Microsoft YaHei', 'SimHei', 'DengXian']:
    if any(f == x.name for x in font_manager.fontManager.ttflist):
        plt.rcParams['font.family'] = f; break
plt.rcParams['axes.unicode_minus'] = False
NAVY = '#1F4E79'; ORANGE = '#E87722'; GRAY = '#808080'
RED = '#C0504D'; GREEN = '#548235'   # 涨红跌绿仅用于股价/涨跌
```

### 图表类型选择
| 数据形态 | 推荐图形 |
|---|---|
| 年度序列（审批数/管线数） | 柱线复合图（柱=年度值，线=累计值） |
| 构成占比 | 环形图/堆叠条 |
| 多公司同期对比 | 分组柱状图（量级差大时用对数坐标） |
| 时间演进 | 时间轴节点图 |
| 市场规模预测 | 折线+面积图（标注CAGR） |
| 估值/融资分布 | 气泡图（气泡=融资额） |
| 疗效对比 | 成对柱状图（治疗组vs安慰剂） |

### 图注三要素（缺一不可）
1. 图题（黑体深蓝，含时间范围）；
2. 数据来源（机构+文件）；
3. 口径说明（哪些点是估计值、折算汇率、同比基数处理）。

## 四、图表插入正文的锚点方法

用 python-docx 按"数据首次出现的段落"定位插入：
1. 遍历 `doc.paragraphs`，用数据关键词（如"1451"）定位锚点段落；
2. `anchor._p.addnext(图片段落)` + 图题图注段落（图题黑体深蓝10.5pt、图注灰色9pt）；
3. 图片宽度统一 `Cm(15.0)`；
4. 插入完成后校验：`len([rel for rel in doc.part.rels.values() if 'image' in rel.reltype])` 应等于计划图表数；
5. 图号按正文出现顺序重排（不沿用图表册编号）。

## 五、Critic自审清单

- [ ] 每个数字能否在检索记录中找到出处？推算值是否标注？
- [ ] 章节编号连续无跳号（警惕4.5直接跳到第五章、无4.4）？
- [ ] 图号连续、图注齐全、估计值已标注？
- [ ] 正文引用编号[1]-[N]与参考文献表一一对应、无孤儿引用？
- [ ] 财务数据的币种、报告期、审计状态是否全部标注？
- [ ] "待核"条目是否集中列入发表前核对清单？
- [ ] 逐章对抗审查的用户采纳项是否全部修正落实？

## 六、初版交付规范（1.1版：仅初版，用户改终版）

1.1版不再产出三档版本。交付物为**单一初版报告**，供用户修订为终版。

### 初版交付要求
1. **完整性**：初版必须是完整报告（正文+参考文献+数据说明表+对抗裁决记录附录），不留"此处待补"占位；
2. **数据一致性**：正文、图表、附录中同一指标数值必须完全一致；
3. **可修订性**：章节结构清晰、编号连续，方便用户直接在此基础上修改；
4. **封面标注**：注明"初版（供修订）"及生成日期；
5. **交付提示**：交付时提醒用户"您改为终版后提交给我，我将执行终版diff学习（阶段6）"。

### 交付动作
- 用 present_files 呈递初版 Word 文档；
- 附一句话说明初版结构与下一步（等待用户终版）。
