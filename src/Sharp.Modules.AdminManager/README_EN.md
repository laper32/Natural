# AdminManager

## Overview

AdminManager is a permission management plugin designed to extract `Sharp.Shared.Objects.IAdmin` from the core, enabling modular and extensible permission management.

For more information, you can also refer to https://www.doubao.com/thread/wc0f1c5cae120c2bb
> If you are a non-Chinese user, please use a translator.

## Design Goals

### 1. Core Decoupling

This plugin aims to extract `Sharp.Shared.Objects.IAdmin` from the core, making permission management an independent pluggable module rather than part of the core. Benefits include:
- Reduced core complexity
- Improved system maintainability
- Easier independent upgrades and replacements of the permission system

### 2. API Changes

Correspondingly, `Sharp.Extensions.CommandManager.ICommandManager.RegisterAdminCommand` will be removed. Please use the new API provided by AdminManager for permission-related command registration and management.

### 3. Permission Configuration Convention

AdminManager provides a complete permission configuration convention system supporting the following features:

#### Configuration Structure

The configuration file uses JSON format and contains three main sections:

**PermissionCollection** - Defines all available specific permissions

```json
"PermissionCollection": {
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

**Roles** - Defines reusable permission groups

```json
"Roles": {
  "GlobalRoot": ["*"],                    // All permissions
  "SystemAdmin": ["System:*:*"],          // Wildcard matching
  "PluginA.Admin": [
    "PluginA:*:*",
    "!PluginA:Weapon:Give"                // Deny specific permission
  ]
}
```

**Admins** - Specific users and their permission configurations

```json
"Admins": [
  {
    "Name": "SuperAdmin",
    "Identity": 111111111,
    "Immunity": 255,
    "Permissions": [
      "@GlobalRoot",                       // Inherit role
      "!System:Plugin:Uninstall"           // Deny specific permission
    ]
  }
]
```

#### Core Features

##### 1. Wildcard Matching
- `*` - Match all permissions
- `System:*:*` - Match all three-segment System permissions
- `*:Chat:*` - Match all chat-related permissions across all plugins

##### 2. Deny Permissions (Highest Priority)
- `!PluginA:Weapon:Give` - Deny specific permission
- `!System:Role:*` - Wildcard deny

##### 3. Role Inheritance
- `@GlobalRoot` - Inherit all permissions from a role
- Supports multiple role inheritance
- Supports nested inheritance (roles inheriting from roles)

##### 4. Case Insensitive
- Permission names and role names are case-insensitive
- `system:*:*` is equivalent to `System:*:*`

##### 5. Segment Matching (Prefix Matching)
- `System:*` - Match all permissions starting with System (e.g., `System:Role:Create`, `System:User:View:Detail`, etc.)
- `System:*:*` - Match all System permissions with three or more segments (e.g., `System:Role:Create`, `System:Role:Create:Advanced`, etc.)
- Wildcard prefix matching: pattern segments ≤ permission segments

## Configuration Examples

For complete configuration examples, please refer to the project test cases, which include the following test scenarios:
- Global administrator permission configuration
- Separation of system administrators and plugin administrators
- Multi-role inheritance and permission override
- Combination of wildcards and deny rules
- Case insensitivity validation
- Nested role inheritance (multi-level depth)

## Migration Guide

If you previously used `Sharp.Extensions.CommandManager.ICommandManager.RegisterAdminCommand`, please:

1. Migrate to the AdminManager plugin
2. Define permissions using the new permission configuration system
3. Register and check permissions through the API provided by AdminManager

## Use Cases

- ✅ Need for fine-grained permission control system
- ✅ Unified permission management in multi-plugin environments
- ✅ Need for role inheritance and permission reuse
- ✅ Desire to decouple the permission system from the core
