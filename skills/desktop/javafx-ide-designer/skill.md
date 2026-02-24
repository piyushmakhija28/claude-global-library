# JavaFX IDE Designer Skill

**Version**: 1.0.0  
**Last Updated**: Feb 24, 2026  
**Category**: Desktop UI Design  
**Status**: Active

---

## Core Components

### 1. Menu Bar + Tool Bar
- File, Edit, View, Run, Tools, Help menus
- Quick action buttons (Build, Run, Clean)
- Keyboard shortcuts integration

### 2. File Explorer (TreeView)
- Lazy-loading on expand
- Skip hidden files (.git, target, node_modules)
- Double-click to open files
- Show file modification indicators

### 3. Code Editor (TabPane + RichTextFX)
- Multi-tab support
- Syntax highlighting for Java/XML/JSON
- File modification tracking (• indicator)
- Line numbers, code folding

### 4. Terminal/Console
- TextArea for output (read-only)
- TextField for commands
- Async execution (don't freeze UI)
- Special commands: help, clear, pwd

### 5. Chat Panel
- User/Assistant message bubbles
- Different styling by sender
- Auto-scroll to latest message
- Message persistence
- Model selector dropdown

### 6. Settings Dialog
- Tabbed preferences (General, API, Chat)
- Theme selection
- Font size adjustment (Spinner)
- Reset to defaults

---

## Layout Pattern

```
┌──────── MenuBar + ToolBar ────────┐
├────────────────────────────────────┤
│  │                    │            │
│ F│   Editor           │ Chat       │
│ i│   (TabPane)        ├───────────┤
│ l│                    │ Terminal   │
│ e│                    │            │
└────────────────────────────────────┘
│         StatusBar (Session, Model) │
└────────────────────────────────────┘
```

---

## Best Practices

✅ Use FXML for layouts  
✅ Never block UI thread (use Platform.runLater)  
✅ Apply CSS for theming (dark theme recommended)  
✅ Lazy-load large datasets  
✅ Persist user settings  
✅ Show appropriate error messages  
✅ Implement keyboard shortcuts  

---

## Quick Implementation Steps

1. Create BorderPane root layout
2. Add MenuBar + ToolBar to top
3. Add FileExplorer (TreeView) to left
4. Add EditorTabPane to center
5. Add ChatPanel + Terminal to right (SplitPane)
6. Add StatusBar to bottom
7. Apply dark theme CSS
8. Implement event handlers
9. Create services for business logic
10. Test all components

---

## Services Pattern

- **UIService**: Component management
- **FileService**: File I/O operations
- **ApiService**: External API calls (Claude, etc)
- **SettingsService**: Preferences storage
- **ThemeService**: Theme management

---

For detailed patterns and code examples, see QUICK-START.md
