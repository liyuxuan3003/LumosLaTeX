# 编写规范（文档类）

本文档定义`.cls`文档类的文档编写规范，通用规范见 [PlanCommon.md](PlanCommon.md)。

## 文档结构

文档由一级标题`# 类名`开头，然后依次是以下小节。

```
## 文档选项
## 引入方式
## 自定义命令
## 自定义环境
```

### 文档选项

用两列表格（`选项`、`说明`）列出`\DeclareStringOption`和`\DeclareBoolOption`等定义的键值选项，注明默认值和可选值。

### 引入方式

给出Git子模块引入命令、`\input@path`配置和`\documentclass`声明。不写完整的最小示例（MWE属于Template文档）。

若存在配套的Template和推荐的Minimus，以纯文本URL形式注明（如"参见NotebookNeonTemplate：https://..."）。

### 自定义命令

与宏包相同，见 [PlanPackage.md](PlanPackage.md) 中对应章节。

### 自定义环境

与宏包相同，见 [PlanPackage.md](PlanPackage.md) 中对应章节。
