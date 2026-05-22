基于 React Native (Expo) 构建的通用刷题 App，支持从 Excel 文件导入题库，可用于各类资格考试备考练习和模拟考试。

## 功能特性

- **Excel 题库导入** — 支持 `.xlsx` / `.xls` 文件导入，自动检测列映射，支持多级别题库管理
- **分级学习** — 题库按等级分类（基础、高级工、技师、高级技师等），支持自定义题库名称和隐藏不需要的等级
- **练习模式** — 按等级逐题练习，即时反馈正确/错误，支持收藏题目
- **随机练习** — 从所有级别随机抽题，打乱顺序练习
- **模拟考试** — 30 题限时（30 分钟）考试模式，按题型比例均匀抽题，模拟真实考试场景
- **错题复习** — 自动记录错题，支持错题重练和手动移除已掌握的错题
- **收藏复习** — 收藏重点题目，集中复习
- **学习统计** — 记录总答题数、正确率、各题型掌握情况
- **进度续接** — 练习和考试均支持中断后恢复上次进度
- **深色模式** — 支持浅色/深色主题切换
- **应用名称自定义** — 可修改首页显示的应用名称

## 技术栈

| 类别  | 技术  |
| --- | --- |
| 框架  | React Native 0.81 + Expo SDK 54 |
| 导航  | @react-navigation (Stack Navigator) |
| 本地存储 | expo-sqlite (题库数据), AsyncStorage (用户数据) |
| Excel 解析 | SheetJS (xlsx) |
| 文件选择 | expo-document-picker |
| 文件系统 | expo-file-system |

## 项目结构

```
CustomizedQuestionBankApp/
├── App.js                      # 应用入口
├── index.js                    # 注册入口
├── app.json                    # Expo 配置
├── package.json                # 依赖管理
├── eas.json                    # EAS Build 配置
├── assets/                     # 图标和启动画面
├── src/
│   ├── components/             # 通用组件
│   │   ├── QuestionCard.js     # 题目卡片（含选项按钮）
│   │   ├── OptionButton.js     # 选项按钮
│   │   └── ConfirmModal.js     # 确认弹窗
│   ├── contexts/
│   │   └── AppContext.js       # 全局状态管理
│   ├── data/
│   │   ├── excelParser.js      # Excel 解析引擎
│   │   ├── columnMapper.js     # 列映射配置与自动检测
│   │   ├── questionLoader.js   # 题目加载与缓存
│   │   └── questionStorage.js  # 题库本地存储（SQLite）
│   ├── navigation/
│   │   └── AppNavigator.js     # 页面路由导航
│   ├── screens/
│   │   ├── HomeScreen.js       # 首页（题库概览、快捷操作）
│   │   ├── LevelScreen.js      # 等级题目列表
│   │   ├── PracticeScreen.js   # 练习模式
│   │   ├── ExamScreen.js       # 模拟考试
│   │   ├── ResultScreen.js     # 考试结果
│   │   ├── ReviewScreen.js     # 错题/收藏复习
│   │   ├── StatsScreen.js      # 学习统计
│   │   ├── SettingsScreen.js   # 设置页面
│   │   ├── AboutScreen.js      # 关于页面
│   │   ├── ImportScreen.js     # 导入题库（三步向导）
│   │   └── ImportPreviewScreen.js # 导入预览
│   └── utils/
│       ├── storage.js          # AsyncStorage 封装
│       └── fileHelper.js       # 文件选择与读取工具
└── scripts/
    └── convert-xls.js          # XLS → JSON 批处理转换脚本
```

## 快速开始

### 环境要求

- Node.js >= 18
- npm 或 yarn
- Expo CLI

### 安装运行

```bash
# 安装依赖
npm install

# 启动开发服务器
npx expo start

# 在 Android 设备/模拟器上运行
npx expo run:android
```

### 构建 APK

```bash
# 使用 EAS Build（推荐）
eas build -p android --profile preview

# 或本地构建
cd android && ./gradlew assembleRelease
```

## 使用指南

### 导入题库

1. 准备 Excel 文件（`.xlsx` 或 `.xls`），确保包含题目列、答案列、选项列等
2. 在 App 中进入「设置」→「导入题库」
3. 按向导步骤选择文件 → 选择等级 → 确认列映射 → 预览导入
4. 支持的题型：单选题、多选题、判断题、简答题、论述题、计算题

### Excel 列格式说明

默认支持的等级预设：

| 等级  | 类别列 | 题型列 | 答案列 | 题目列 | 选项A-D |
| --- | --- | --- | --- | --- | --- |
| 基础  | —   | 0   | 1   | 2   | 3-6 |
| 高级工 | 0   | 1   | 2   | 3   | 4-7 |
| 技师  | 0   | 1   | 2   | 5   | 6-9 |
| 高级技师 | 0   | 1   | 2   | 3   | 4-7 |

自定义等级支持根据表头文字自动检测列映射。

### 批处理转换脚本

如果有多份 XLS 题库需要批量转为 JSON，可使用 `scripts/convert-xls.js`：

```bash
node scripts/convert-xls.js
```

脚本会将 `电工题库/` 目录下的 XLS 文件转换为 `assets/data/` 下的 JSON 文件。

## 许可证

MIT License
