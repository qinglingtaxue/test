---
name: 42edu:ui
description: UI design skill for online education platforms with video player, course cards and learning progress components
---

# 界面设计技能 - 42edu:ui

## 技能概述

专为在线教育平台设计的UI技能，提供视频播放器、课程卡片、学习进度、直播间等教育场景专用组件的界面设计规范。

---

## 适用场景

- 在线教育平台界面设计
- MOOC平台UI组件库
- 企业培训系统界面
- 知识付费平台设计
- 教育小程序/App设计

---

## 核心能力

### 1. 教育场景组件
- 视频播放器组件
- 课程卡片组件
- 学习进度组件
- 直播间交互组件
- 作业提交组件

### 2. 交互设计
- 视频控制交互
- 课程选择流程
- 作业提交流程
- 证书展示交互
- 学习社区互动

### 3. 响应式设计
- PC端布局
- 平板适配
- 移动端优化
- 横竖屏切换

### 4. 主题定制
- 暗黑模式
- 品牌色定制
- 字体方案
- 无障碍设计

---

## 输入要求

### 必需文件
- `.42cog/real/real.md` - 用户体验约束
- `.42cog/cog/cog.md` - 业务流程定义

### Real约束示例
```markdown
<real>
- 视频播放器必须支持倍速播放和字幕
- 课程列表加载时间必须小于2秒
- 所有交互必须有明确的视觉反馈
- 界面必须符合WCAG 2.1 AA标准
</real>
```

---

## 输出内容

### 1. 视频播放器组件

**组件结构**：
```tsx
// VideoPlayer.tsx
interface VideoPlayerProps {
  videoUrl: string
  title: string
  duration: number
  progress?: number
  onProgressUpdate: (progress: number) => void
  onComplete: () => void
}

export function VideoPlayer({
  videoUrl,
  title,
  duration,
  progress = 0,
  onProgressUpdate,
  onComplete
}: VideoPlayerProps) {
  // 播放器实现
}
```

**功能特性**：
- 播放/暂停控制
- 进度条拖拽
- 倍速播放（0.5x, 1x, 1.25x, 1.5x, 2x）
- 音量调节
- 全屏/画中画
- 清晰度切换（360p, 720p, 1080p）
- 字幕开关
- 快进/快退（双击左右侧）
- 自动记忆播放进度

**设计规范**：
```css
.video-player {
  width: 100%;
  aspect-ratio: 16 / 9;
  background: #000;
  position: relative;
  border-radius: 8px;
  overflow: hidden;
}

.video-controls {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px;
  background: linear-gradient(transparent, rgba(0,0,0,0.8));
}

.progress-bar {
  height: 4px;
  background: rgba(255,255,255,0.3);
  cursor: pointer;
  border-radius: 2px;
}

.progress-bar-fill {
  height: 100%;
  background: #3b82f6;
  transition: width 0.1s;
}
```

### 2. 课程卡片组件

**组件结构**：
```tsx
// CourseCard.tsx
interface CourseCardProps {
  course: {
    id: string
    title: string
    cover: string
    teacher: string
    price: number
    rating: number
    studentCount: number
    duration: number
  }
  onClick: () => void
}

export function CourseCard({ course, onClick }: CourseCardProps) {
  return (
    <div className="course-card" onClick={onClick}>
      <div className="course-cover">
        <img src={course.cover} alt={course.title} />
        <div className="course-badge">热门</div>
      </div>
      <div className="course-info">
        <h3 className="course-title">{course.title}</h3>
        <div className="course-meta">
          <span className="teacher">{course.teacher}</span>
          <span className="rating">⭐ {course.rating}</span>
        </div>
        <div className="course-footer">
          <span className="price">¥{course.price}</span>
          <span className="students">{course.studentCount}人学习</span>
        </div>
      </div>
    </div>
  )
}
```

**布局变体**：
- 列表模式（大卡片，详细信息）
- 网格模式（小卡片，紧凑布局）
- 轮播模式（首页推荐）

**交互状态**：
- 默认态
- 悬浮态（显示更多信息）
- 点击态
- 已购买态（显示"继续学习"）

### 3. 学习进度组件

**进度展示**：
```tsx
// LearningProgress.tsx
interface LearningProgressProps {
  totalLessons: number
  completedLessons: number
  totalDuration: number
  learnedDuration: number
}

export function LearningProgress({
  totalLessons,
  completedLessons,
  totalDuration,
  learnedDuration
}: LearningProgressProps) {
  const progressPercent = (completedLessons / totalLessons) * 100
  
  return (
    <div className="learning-progress">
      <div className="progress-header">
        <span>学习进度</span>
        <span>{completedLessons}/{totalLessons} 课时</span>
      </div>
      <div className="progress-bar">
        <div 
          className="progress-fill" 
          style={{ width: `${progressPercent}%` }}
        />
      </div>
      <div className="progress-footer">
        <span>已学习 {Math.floor(learnedDuration / 60)} 分钟</span>
        <span>{progressPercent.toFixed(0)}%</span>
      </div>
    </div>
  )
}
```

**可视化样式**：
- 百分比环形图
- 线性进度条
- 里程碑节点
- 成就徽章

### 4. 课程目录组件

**结构设计**：
```tsx
// CourseCatalog.tsx
interface Lesson {
  id: string
  title: string
  duration: number
  isFree: boolean
  isCompleted: boolean
  isLocked: boolean
}

interface Chapter {
  id: string
  title: string
  lessons: Lesson[]
}

export function CourseCatalog({ chapters }: { chapters: Chapter[] }) {
  return (
    <div className="course-catalog">
      {chapters.map(chapter => (
        <div key={chapter.id} className="chapter">
          <div className="chapter-header">{chapter.title}</div>
          <div className="chapter-lessons">
            {chapter.lessons.map(lesson => (
              <div key={lesson.id} className="lesson-item">
                <div className="lesson-icon">
                  {lesson.isCompleted ? '✓' : 
                   lesson.isLocked ? '🔒' : '▶'}
                </div>
                <div className="lesson-info">
                  <span className="lesson-title">{lesson.title}</span>
                  <span className="lesson-duration">
                    {Math.floor(lesson.duration / 60)} 分钟
                  </span>
                </div>
                {lesson.isFree && (
                  <span className="free-badge">试看</span>
                )}
              </div>
            ))}
          </div>
        </div>
      ))}
    </div>
  )
}
```

### 5. 直播间组件

**功能模块**：
```tsx
// LivestreamRoom.tsx
export function LivestreamRoom() {
  return (
    <div className="livestream-room">
      {/* 视频流区域 */}
      <div className="video-area">
        <video className="live-video" />
        <div className="live-badge">直播中</div>
        <div className="viewer-count">👁 1,234 人在线</div>
      </div>
      
      {/* 聊天区域 */}
      <div className="chat-area">
        <div className="chat-messages">
          {/* 聊天消息列表 */}
        </div>
        <div className="chat-input">
          <input placeholder="说点什么..." />
          <button>发送</button>
        </div>
      </div>
      
      {/* 互动工具栏 */}
      <div className="toolbar">
        <button className="btn-like">👍 点赞</button>
        <button className="btn-gift">🎁 送礼</button>
        <button className="btn-share">📤 分享</button>
      </div>
    </div>
  )
}
```

### 6. 作业提交组件

**表单设计**：
```tsx
// HomeworkSubmit.tsx
export function HomeworkSubmit({ homeworkId }: { homeworkId: string }) {
  return (
    <div className="homework-submit">
      <div className="homework-info">
        <h3>作业标题</h3>
        <div className="homework-meta">
          <span>截止时间：2025-12-10 23:59</span>
          <span>满分：100分</span>
        </div>
      </div>
      
      <div className="homework-content">
        <h4>作业要求</h4>
        <div className="requirement">
          {/* Markdown渲染的作业要求 */}
        </div>
      </div>
      
      <div className="submit-form">
        <div className="form-group">
          <label>作业内容</label>
          <textarea 
            placeholder="请输入作业内容..."
            rows={10}
          />
        </div>
        
        <div className="form-group">
          <label>附件上传</label>
          <input type="file" multiple />
          <div className="file-list">
            {/* 已上传文件列表 */}
          </div>
        </div>
        
        <div className="submit-actions">
          <button className="btn-draft">保存草稿</button>
          <button className="btn-submit">提交作业</button>
        </div>
      </div>
    </div>
  )
}
```

---

## 设计系统

### 颜色规范

**主色调**：
```css
:root {
  /* 品牌色 */
  --primary: #3b82f6;
  --primary-hover: #2563eb;
  --primary-active: #1d4ed8;
  
  /* 功能色 */
  --success: #10b981;
  --warning: #f59e0b;
  --error: #ef4444;
  --info: #3b82f6;
  
  /* 中性色 */
  --gray-50: #f9fafb;
  --gray-100: #f3f4f6;
  --gray-200: #e5e7eb;
  --gray-300: #d1d5db;
  --gray-400: #9ca3af;
  --gray-500: #6b7280;
  --gray-600: #4b5563;
  --gray-700: #374151;
  --gray-800: #1f2937;
  --gray-900: #111827;
}
```

**暗黑模式**：
```css
[data-theme="dark"] {
  --bg-primary: #111827;
  --bg-secondary: #1f2937;
  --text-primary: #f9fafb;
  --text-secondary: #d1d5db;
  --border: #374151;
}
```

### 字体规范

```css
:root {
  /* 中文字体 */
  --font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", 
               "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei",
               sans-serif;
  
  /* 等宽字体（代码） */
  --font-mono: "SF Mono", "Monaco", "Inconsolata", "Fira Code",
               "Droid Sans Mono", "Source Code Pro", monospace;
  
  /* 字号 */
  --text-xs: 0.75rem;    /* 12px */
  --text-sm: 0.875rem;   /* 14px */
  --text-base: 1rem;     /* 16px */
  --text-lg: 1.125rem;   /* 18px */
  --text-xl: 1.25rem;    /* 20px */
  --text-2xl: 1.5rem;    /* 24px */
  --text-3xl: 1.875rem;  /* 30px */
}
```

### 间距规范

```css
:root {
  --spacing-1: 0.25rem;  /* 4px */
  --spacing-2: 0.5rem;   /* 8px */
  --spacing-3: 0.75rem;  /* 12px */
  --spacing-4: 1rem;     /* 16px */
  --spacing-5: 1.25rem;  /* 20px */
  --spacing-6: 1.5rem;   /* 24px */
  --spacing-8: 2rem;     /* 32px */
  --spacing-10: 2.5rem;  /* 40px */
  --spacing-12: 3rem;    /* 48px */
}
```

### 圆角规范

```css
:root {
  --radius-sm: 0.25rem;  /* 4px */
  --radius-md: 0.5rem;   /* 8px */
  --radius-lg: 0.75rem;  /* 12px */
  --radius-xl: 1rem;     /* 16px */
  --radius-full: 9999px; /* 圆形 */
}
```

---

## 响应式设计

### 断点规范

```css
/* 移动端 */
@media (max-width: 640px) { }

/* 平板端 */
@media (min-width: 641px) and (max-width: 1024px) { }

/* 桌面端 */
@media (min-width: 1025px) { }
```

### 布局适配

**课程卡片响应式**：
```css
.course-grid {
  display: grid;
  gap: 1.5rem;
  
  /* 移动端：1列 */
  grid-template-columns: 1fr;
  
  /* 平板端：2列 */
  @media (min-width: 641px) {
    grid-template-columns: repeat(2, 1fr);
  }
  
  /* 桌面端：3-4列 */
  @media (min-width: 1025px) {
    grid-template-columns: repeat(3, 1fr);
  }
  
  @media (min-width: 1280px) {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

---

## 交互规范

### 按钮状态

```css
.btn {
  /* 默认态 */
  background: var(--primary);
  color: white;
  transition: all 0.2s;
  
  /* 悬浮态 */
  &:hover {
    background: var(--primary-hover);
    transform: translateY(-1px);
    box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
  }
  
  /* 点击态 */
  &:active {
    background: var(--primary-active);
    transform: translateY(0);
  }
  
  /* 禁用态 */
  &:disabled {
    background: var(--gray-300);
    cursor: not-allowed;
    opacity: 0.6;
  }
  
  /* 加载态 */
  &.loading {
    pointer-events: none;
    position: relative;
  }
  
  &.loading::after {
    content: '';
    position: absolute;
    width: 16px;
    height: 16px;
    border: 2px solid white;
    border-top-color: transparent;
    border-radius: 50%;
    animation: spin 0.6s linear infinite;
  }
}
```

### 动画规范

```css
/* 淡入 */
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

/* 滑入 */
@keyframes slideIn {
  from { transform: translateY(20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

/* 旋转加载 */
@keyframes spin {
  to { transform: rotate(360deg); }
}

/* 使用动画 */
.fade-in {
  animation: fadeIn 0.3s ease-out;
}
```

---

## 无障碍设计

### 键盘导航
- Tab键切换焦点
- Enter/Space键激活按钮
- Esc键关闭弹窗
- 方向键导航列表

### ARIA标签
```tsx
<button 
  aria-label="播放视频"
  aria-pressed={isPlaying}
>
  {isPlaying ? '⏸' : '▶'}
</button>
```

### 颜色对比度
- 正文文本：至少4.5:1
- 大号文本：至少3:1
- 图标按钮：至少3:1

---

## 技术栈

### 推荐框架
- React 18+ / Vue 3+
- Next.js 14+ / Nuxt 3+
- TailwindCSS 3+
- Radix UI / Headless UI

### 视频播放
- Video.js
- Plyr
- HLS.js
- DPlayer

---

## 使用方法

```bash
# 生成UI设计规约
42cog g spec --role design --skill ui --plugin 42edu

# 输出文件：.42cog/spec/design/ui.spec.md
```

---

## 相关技能

- `42edu:devops` - 前端部署和CDN配置
- `42cog:design:graphic` - 平面设计和品牌视觉
- `42cog:tech:frontend` - 前端开发实现

---

**42edu:ui** - 专业的教育平台界面设计
