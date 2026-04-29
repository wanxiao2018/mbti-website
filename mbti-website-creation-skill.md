---
name: mbti-website-creation
description: 创建MBTI 16种人格类型网站的完整流程——深色主题、Canvas动画、个性化详情页、GitHub Pages部署
triggers:
  - MBTI网站
  - 人格类型页面
  - 类型详情页
  - MBTI部署
---

# MBTI 16种人格类型网站

## 项目概况

深色主题静态网站，包含主页（16种人格概览）和16个独立详情页。
路径：~/mbti/
在线：https://wanxiao2018.github.io/mbti-website/
仓库：https://github.com/wanxiao2018/mbti-website

## 技术栈

- 纯HTML/CSS/JS，无构建工具
- Google Fonts（Inter + Noto Sans SC）
- Canvas矩阵雨动画
- IntersectionObserver滚动动画
- 自定义光标（mix-blend-mode:difference）

## 设计规范

### 配色
- 背景：#0a0a0f
- 文字：#f0ece4
- 暗色：#6b6b7b
- 每种类型独立accent色（见下方）

### 16种类型accent色
分析家：INTJ #6366f1, INTP #22d3ee, ENTJ #ef4444, ENTP #f97316
外交家：INFJ #8b5cf6, INFP #a78bfa, ENFJ #eab308, ENFP #f5c542
守护者：ISTJ #3b82f6, ISFJ #ec4899, ESTJ #dc2626, ESFJ #f472b6
探险家：ISTP #94a3b8, ISFP #22c55e, ESTP #3b82f6, ESFP #f59e0b

### Emoji设计
见 ~/mbti/mbti-emoji-map.md
类别：🧠分析家 🕊️外交家 ⚓守护者 🗺️探险家

## 页面结构（每个详情页）

1. **Hero区**：类型代码大字 + Canvas雨特效 + badge
2. **解码区**：标题"解码 XXX" + 四个字母卡片（上下字数对齐，各8字）
3. **引言区**：定制名言（不要斜体）
4. **核心特质**：6个卡片（emoji+名+描述，3行内，两端对齐，图标必须匹配文字）
5. **光与影**：定制标题（不要通用"每种光都有自己的影子"）+ 定制介绍 + 闪光点/成长区各5条（按字数短到长排列）
6. **个人寄语**：给XXX的你（个性化内容，不是通用模板）
7. **Footer + JS**：光标、Canvas、滚动动画

## 关键内容规则

### 必须个性化的部分（不能用通用模板）
- section-desc：不要"这不是四个标签的简单叠加"
- 光与影标题：每种类型不同
- 光与影介绍：不要"同一枚硬币的两面"
- 特质描述：直击本质，不泛泛而谈
- 个人寄语：针对该类型的具体困扰

### 字数对齐规则
- 四个字母卡片：每行8字，上下一致
- 闪光点/成长区：按字数从短到长排列

### CSS要点
- .trait-text：text-align:justify + -webkit-line-clamp:3 + overflow:hidden
- .quote-text：不要font-style:italic
- .hero：max-width:100%; padding:0（否则Canvas不全屏）
- 光与影介绍：white-space:nowrap（不换行）
- 字母描述：white-space:nowrap（如果需要）

## 常见坑

1. **Canvas雨不显示**：检查chars变量是否有`.split('').split('')`重复
2. **成长区丢失**：写文件时确保不截断，用cat读取而不是read_file（行号格式干扰）
3. **字母描述不对齐**：用Python len()验证，不要手动数
4. **trait-icon不匹配**：每个emoji必须和trait-name含义一致
5. **hero不全屏**：section有max-width:1200px，hero需要覆盖
6. **line-clamp截断**：详情页用3行，主页卡片用2行

## GitHub Pages部署

```bash
cd ~/mbti
git add -A && git commit -m "message"
gh repo create mbti-website --public --source=. --remote=origin --push
gh api -X POST repos/wanxiao2018/mbti-website/pages --input - <<< '{"source":{"branch":"main","path":"/"}}'
```

地址：https://wanxiao2018.github.io/mbti-website/
