# 约定式提交 1.0.0-beta.2
## 概述
开源维护者在将特性分支合并入 master 时，可编写标准化的提交说明。

提交说明的结构如下所示：

    <类型>[可选的作用域]: <描述>

    [可选的正文]

    [可选的脚注]
1. fix: 类型 为 fix 的提交表示在代码库中修复了一个 bug（`这和语义化版本中的 PATCH 相对应`）。
2. feat: 类型 为 feat 的提交表示在代码库中新增了一个功能（`这和语义化版本中的 MINOR 相对应`）。
3. BREAKING CHANGE: 在可选的正文或脚注的起始位置带有 BREAKING CHANGE: 的提交，表示引入了破坏性变更（这和语义化版本中的 MAJOR 相对应，`不再向下兼容`）。

## 示例
feat: allow provided config object to extend other configs

BREAKING CHANGE: `` `extends` `` key in config file is now used for extending other config files

1. 每个提交都必须使用类型字段前缀，这由一个形如 feat 或 fix 的名词组成，其后接冒号和空格。
2. 当一个提交为应用或类库实现了新特性时，必须使用 feat 类型。
3. 当一个提交为应用修复了 bug 时，必须使用 fix 类型。
4. 可选的作用域字段可以在类型后提供。`作用域是描述代码库中某个部分的词组`，封装在括号中，形如 fix(parser): 等。
5. 描述字段必须紧接在类型或作用域前缀之后。
6. 在简短描述之后，可以编写更长的提交正文，为代码变更提供额外的上下文信息。正文必须起始于描述字段结束的一个空行后。
7. 在正文结束的一个空行后，可以编写脚注（如果正文缺失，可以编写在描述之后）。脚注应当为代码变更包含额外的 `issue 引用信息`
8. 破坏性变更必须在提交的正文或脚注加以展示。一个破坏性变更必须包含大写的文本 BREAKING CHANGE，紧跟冒号和空格。
9. 在 BREAKING CHANGE: 之后必须提供描述，`描述对 API 的变更`
10. 脚注必须只包含 BREAKING CHANGE、外部链接、issue 引用和其它元数据信息。
11. 在提交说明中，可以使用 feat 和 fix 之外的类型。