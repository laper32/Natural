# CommandManager

## Overview

CommandManager is a plugin that provides centralized command registration management. By using this plugin, you can clearly track the source of all registered commands, facilitating further management and processing.

## Key Features

### 1. Centralized Command Registration Management

This plugin provides centralized command registration management. In other words, if commands are registered elsewhere, you can identify who registered them and take further action accordingly. This transparency makes command management more controllable and traceable.

### 2. Ideal Choice for Multi-Plugin Scenarios

This plugin is very useful in multi-plugin scenarios. If you don't need multi-plugin support and prefer a monolithic approach, the Extension package is still suitable for you. CommandManager is particularly suited for scenarios requiring coordination and command management across multiple independent plugins.

### 3. Avoid Mixing with Extension

**Important Notice:** Mixing Extension with this plugin is not recommended, as it may lead to unexpected situations. Please choose one approach based on your project architecture:
- Multi-plugin architecture: Use Sharp.Modules.CommandManager
- Monolithic architecture: Use Sharp.Extensions.CommandManager

## Use Cases

- ✅ Multi-plugin projects requiring unified management of commands registered by various plugins
- ✅ Projects needing to track command sources and registrants
- ✅ Scenarios requiring centralized command management and control
- ❌ Simple monolithic applications (Extension recommended)
