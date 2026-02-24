# JavaFX IDE Designer - Quick Start

## Invoke This Skill

```
/javafx-ide-designer
```

Or reference in code with:
```
Skill: javafx-ide-designer
```

---

## Quick Implementation Checklist

### Phase 1: Layout
- [ ] BorderPane as root
- [ ] MenuBar (File, Edit, View, Run, Tools)
- [ ] ToolBar with quick buttons
- [ ] FileExplorer (TreeView)
- [ ] EditorTabPane with RichTextFX
- [ ] ChatPanel (VBox)
- [ ] Terminal (TextArea + TextField)
- [ ] StatusBar (HBox)

### Phase 2: Styling
- [ ] Apply dark-theme.css
- [ ] Color scheme: #1e1e1e background, #d4d4d4 text
- [ ] Highlight color: #0d47a1 (blue)
- [ ] Selected item: #0e639c

### Phase 3: Services
- [ ] FileService (open, save, list)
- [ ] TerminalService (command execution)
- [ ] ChatService (message handling)
- [ ] SettingsService (persistence)

### Phase 4: Features
- [ ] File modification tracking
- [ ] Keyboard shortcuts
- [ ] Settings dialog
- [ ] Error handling

---

## Common Code Snippets

### Add Menu Item
```java
MenuItem newItem = new MenuItem("New");
newItem.setOnAction(e -> onNewFile());
fileMenu.getItems().add(newItem);
```

### Lazy-Load Tree
```java
treeItem.expandedProperty().addListener((obs, old, val) -> {
    if (val && treeItem.getChildren().isEmpty()) {
        loadChildren(treeItem);
    }
});
```

### Add Chat Message
```java
addChatMessage("Claude", "Hello!", false);
addChatMessage("You", "Hi there", true);
```

### Async Operation
```java
new Thread(() -> {
    String result = doExpensiveWork();
    Platform.runLater(() -> updateUI(result));
}).start();
```

### Settings Persistence
```java
SettingsService settings = new SettingsService();
settings.setSetting("fontSize", 14);
settings.saveSettings();
```

---

## File Structure for IDE Project

```
project/
├── src/main/java/
│   ├── controller/
│   │   └── MainController.java
│   ├── service/
│   │   ├── FileService.java
│   │   ├── TerminalService.java
│   │   ├── ChatService.java
│   │   └── SettingsService.java
│   ├── model/
│   │   ├── Project.java
│   │   └── Workspace.java
│   └── ui/
│       ├── CodeEditor.java
│       ├── FileExplorer.java
│       └── ChatPanel.java
├── src/main/resources/
│   ├── fxml/main.fxml
│   └── css/dark-theme.css
└── pom.xml
```

---

## Maven Dependencies for IDE

```xml
<!-- JavaFX -->
<dependency>
    <groupId>org.openjfx</groupId>
    <artifactId>javafx-controls</artifactId>
    <version>21.0.2</version>
</dependency>

<!-- RichTextFX for code editor -->
<dependency>
    <groupId>org.fxmisc.richtext</groupId>
    <artifactId>richtextfx</artifactId>
    <version>0.11.2</version>
</dependency>

<!-- ControlsFX for extra controls -->
<dependency>
    <groupId>org.controlsfx</groupId>
    <artifactId>controlsfx</artifactId>
    <version>11.2.0</version>
</dependency>

<!-- Jackson for config files -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.16.1</version>
</dependency>
```

---

## Next Steps

1. Start with FXML layout
2. Create MainController
3. Implement services
4. Add CSS theming
5. Test each component
6. Optimize performance
7. Add keyboard shortcuts
8. Deploy and release

---

**For detailed patterns and examples, see skill.md**
