# AI换衣应用实现计划

## 概述
创建一个时尚简约风格的AI虚拟换衣应用，用户可上传照片，选择服装，通过AI生成换装效果。

## 技术栈
- **前端**: React + TypeScript + Vite + Tailwind CSS + shadcn/ui
- **状态管理**: Zustand
- **动画**: Framer Motion
- **API**: Supabase Edge Functions + Replicate AI (IDM-VTON模型)

## 设计风格
- 时尚简约（类似ZARA/COS）
- 黑白灰主色调 + 金色点缀
- 直角设计（border-radius: 0）
- 优雅字体排版

---

## 实现步骤

### Phase 1: 设计系统配置

**1.1 更新 `src/index.css`**
- 添加时尚简约主题CSS变量
- 自定义工具类（.btn-fashion, .heading-display等）

**1.2 更新 `tailwind.config.ts`**
- 添加自定义颜色（fashion-gold, fashion-cream）
- 配置字体

### Phase 2: 核心组件开发

**2.1 布局组件**
```
src/components/layout/
├── Layout.tsx      # 全局布局
├── Header.tsx      # 导航栏
└── Footer.tsx      # 页脚
```

**2.2 首页组件**
```
src/components/home/
├── HeroSection.tsx     # 全屏大图+CTA
└── FeatureSection.tsx  # 三步骤介绍
```

**2.3 换衣核心组件**
```
src/components/tryon/
├── TryOnWorkspace.tsx       # 工作台主容器
├── PhotoDropzone.tsx        # 拖拽上传
├── GarmentGrid.tsx          # 服装网格
├── GarmentCard.tsx          # 服装卡片
├── ResultPanel.tsx          # 结果展示
└── ProcessingOverlay.tsx    # 处理中遮罩
```

### Phase 3: 状态管理

**3.1 创建 `src/store/tryOnStore.ts`**
- personImage: 用户照片
- garmentImage: 选中服装
- resultImage: 生成结果
- isProcessing: 处理状态
- history: 历史记录

### Phase 4: API集成

**4.1 Supabase Edge Function**
- 函数名: `virtual-tryon`
- 调用Replicate IDM-VTON模型
- 需要密钥: `REPLICATE_API_TOKEN`

**4.2 React Query Hook**
- `src/hooks/useTryOn.ts`
- 处理API调用和状态轮询

### Phase 5: 页面开发

**5.1 首页 `src/pages/Index.tsx`**
- HeroSection + FeatureSection
- "开始试穿" CTA按钮

**5.2 换衣页 `src/pages/TryOnPage.tsx`**
- 三栏布局：照片 | 服装 | 结果
- 底部服装轮播
- 生成按钮

### Phase 6: 路由配置

更新 `src/App.tsx`:
```tsx
<Route path="/" element={<Layout><Index /></Layout>} />
<Route path="/try-on" element={<Layout><TryOnPage /></Layout>} />
```

---

## 关键文件清单

| 文件路径 | 操作 | 说明 |
|---------|------|------|
| `src/index.css` | 修改 | 添加时尚主题CSS变量 |
| `src/App.tsx` | 修改 | 添加路由和Layout |
| `src/pages/Index.tsx` | 重写 | 首页 |
| `src/pages/TryOnPage.tsx` | 新建 | 换衣页面 |
| `src/components/layout/` | 新建 | 布局组件 |
| `src/components/home/` | 新建 | 首页组件 |
| `src/components/tryon/` | 新建 | 换衣组件 |
| `src/store/tryOnStore.ts` | 新建 | Zustand状态 |
| `src/hooks/useTryOn.ts` | 新建 | API调用Hook |
| `src/data/sample-garments.ts` | 新建 | 示例服装数据 |

---

## 示例服装数据

预置8-12件示例服装（使用Unsplash高质量时尚图片）：
- 上装：白T恤、黑衬衫、米色毛衣、灰色西装外套
- 下装：黑色长裤、牛仔裤、白色阔腿裤
- 连衣裙：黑色连衣裙、驼色风衣

---

## 用户交互流程

```
首页 → 点击"开始试穿" → 上传照片 → 选择服装 → 点击生成
→ 显示处理进度(30-60秒) → 展示结果 → 下载/重试
```

---

## 验证方式

1. **UI验证**
   - 访问首页，确认时尚简约设计
   - 检查响应式布局（移动端/桌面端）
   - 验证所有交互动画流畅

2. **功能验证**
   - 上传照片（支持拖拽和点击）
   - 选择服装（预设+自定义上传）
   - 生成换装效果（需配置Replicate API）
   - 下载结果图片

3. **错误处理验证**
   - 上传非图片文件时的错误提示
   - API调用失败时的友好提示
   - 处理超时的处理

---

## 注意事项

- 首次实现先使用模拟数据，确保UI完整
- API集成需要用户提供Replicate API密钥
- 图片上传前进行压缩（max 1024px）
- 结果图片支持对比滑块查看
