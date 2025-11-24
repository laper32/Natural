# AdminManager

## 简介

AdminManager 是一个权限管理插件，旨在将 `Sharp.Shared.Objects.IAdmin` 从内核中转移出来，实现权限管理的模块化和可扩展性。

## 设计目标

### 1. 内核解耦

本插件旨在将 `Sharp.Shared.Objects.IAdmin` 从内核中转移出来，使权限管理成为一个独立的可插拔模块，而不是内核的一部分。这样做的好处包括：
- 降低内核复杂度
- 提高系统的可维护性
- 便于权限系统的独立升级和替换

### 2. API 变更

相对应的，`Sharp.Extensions.CommandManager.ICommandManager.RegisterAdminCommand` 将会被移除。请使用 AdminManager 提供的新 API 进行权限相关的指令注册和管理。

### 3. 权限配置约定

AdminManager 提供了一套完整的权限配置约定系统，支持以下特性：

#### 配置结构

配置文件采用 JSON 格式，包含三个主要部分：

**PermisisonCollection（权限集合）** - 定义所有可用的具体权限

```json
"PermisisonCollection": {
  "System": [
    "System:Role:Create",
    "System:Role:Delete",
    "System:User:View"
  ],
  "PluginA": [
    "PluginA:Weapon:Get",
    "PluginA:Item:Fetch"
  ]
}
```

**Roles（角色字典）** - 定义可复用的权限组

```json
"Roles": {
  "GlobalRoot": ["*"],                    // 所有权限
  "SystemAdmin": ["System:*:*"],          // 通配符匹配
  "PluginA.Admin": [
    "PluginA:*:*",
    "!PluginA:Weapon:Give"                // 拒绝特定权限
  ]
}
```

**Admins（管理员列表）** - 具体用户及其权限配置

```json
"Admins": [
  {
    "Name": "SuperAdmin",
    "Identity": 111111111,
    "Immunity": 255,
    "Permissions": [
      "@GlobalRoot",                       // 继承角色
      "!System:Plugin:Uninstall"           // 拒绝特定权限
    ]
  }
]
```

#### 核心特性

##### 1. 通配符匹配
- `*` - 匹配所有权限
- `System:*:*` - 匹配所有三段 System 权限
- `*:Chat:*` - 匹配所有插件的聊天相关权限

##### 2. 拒绝权限（优先级最高）
- `!PluginA:Weapon:Give` - 拒绝特定权限
- `!System:Role:*` - 通配符拒绝

##### 3. 角色继承
- `@GlobalRoot` - 继承角色的所有权限
- 支持多角色继承
- 支持嵌套继承（角色继承角色）

##### 4. 大小写不敏感
- 权限名和角色名均不区分大小写
- `system:*:*` 等同于 `System:*:*`

##### 5. 段数匹配
- `System:*:*` - 仅匹配三段权限
- `System:*:*:*` - 仅匹配四段权限
- 通配符严格按段数匹配

## 配置示例

完整的配置示例请参见项目测试用例，包含以下测试场景：
- 全局管理员权限配置
- 系统管理员与插件管理员分离
- 多角色继承与权限覆盖
- 通配符与拒绝规则组合使用
- 大小写不敏感验证
- 嵌套角色继承（多层深度）

## 迁移指南

如果你之前使用 `Sharp.Extensions.CommandManager.ICommandManager.RegisterAdminCommand`，请：

1. 迁移到 AdminManager 插件
2. 使用新的权限配置系统定义权限
3. 通过 AdminManager 提供的 API 注册和检查权限

## 使用场景

- ✅ 需要精细的权限控制系统
- ✅ 多插件环境下的统一权限管理
- ✅ 需要角色继承和权限复用
- ✅ 希望将权限系统从内核中解耦