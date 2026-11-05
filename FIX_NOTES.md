# 修复说明（FIX NOTES）

- 项目：Depression-PA-KB-12
- 修复日期：2026-11-04
- 部署日期：2026-11-05
- 线上地址：https://blinkhor.github.io/depression_pa_kb/

## 一、逻辑修复

### Step1 Baseline information

- 修复年龄键名不匹配：兼容 `18` / `＜18` / `<18` 和 `65` / `＞65` / `>65`，`<18`、`>65` 条件现在生效。
- 修复 `'null'` 字符串绕过校验：性别、年龄改为有效值判断，未选择时不能进入 Step2。
- 补齐 BMI 默认匹配：PMID `2096117`、`34649098` 加入全部 4 个 BMI 分类。

### Step2 Depression and comorbidity information

- 修复多选逻辑：Underlying diseases 改为“分类内并集”，符合用户多选为“或”的需求。
- 修复抑郁分类：`subthreshold depression` 不再错误合并 `depressive symptoms`；仅 `depressive disorder/depression` 合并。
- 修复基础疾病校验：未勾选时不能进入下一步。

### Step4 Exercise preparticipation health screening

- 新增筛查结果弹窗（`n-modal` + OK）：
  - 勾选任一症状：弹窗 → OK 后清空 Step4 并重新开始。
  - YES + 有疾病：弹窗 → OK 后进入 Step5。
  - NO + 有疾病：弹窗 → OK 后清空 Step4 并重新开始。
  - YES/NO + 无疾病：直接进入 Step5。
- 未选择 Current activity 时禁用 Next 并提示。

### Step5 Desired exercise goals

- 修复目标数据源：由错误的 `Effect_class_count.json` 改为 `Effect_class.json`，运动目标筛选恢复生效。
- 修复多目标逻辑：改为“分类内并集”，符合多选为“或”的需求。
- 分页选项由 `[3, 5, 7]` 改为 `[3, 4, 5]`，符合每页 3–5 个的要求。

## 二、部署修复

- 新增 `404.html`，解决 GitHub Pages history 路由直接访问 `/pe` 返回 404 的问题。
- 新增 `.nojekyll`。
- 部署分支：`main`

## 三、验证结果

- `npm run build`：通过。
- 本地 10 组数据测试：10/10 通过，PMID 集合完全一致。
- 本地 Step4 弹窗与分支测试：18/18 通过。
- 线上 10 组数据测试：10/10 通过。
- 线上 Step4 弹窗与分支测试：18/18 通过。
- 线上 `/pe` 直接访问正常。
- 结果页默认每页 3 条，按 Scores 降序展示。

## 四、注意事项

- GitHub Pages 首次加载部分 JSON 可能较慢，页面暂无 loading 提示，可能短暂空白；不影响匹配逻辑。
