---
description: Definitive guidelines for writing clean, maintainable, and performant customtkinter applications. This rule file enforces best practices for code structure, theming, component architecture, and common pitfalls.
---

# customtkinter 最佳实践

本文档给出使用 `customtkinter` 开发应用的权威最佳实践。遵循这些准则能让你的 UI 代码一致、可维护、高性能，并充分发挥该库的全部能力。

## 1. 代码组织：基于类的应用

始终使用继承自 `customtkinter.CTk` 的类来组织 `customtkinter` 应用。这样能封装 UI 逻辑、增强模块化、简化状态管理。

❌ 差：过程式的"意大利面"代码
```python
import customtkinter as ctk

# Theme setup (often forgotten or inconsistent)
ctk.set_appearance_mode("System")
ctk.set_default_color_theme("blue")

app = ctk.CTk()
app.geometry("400x240")
app.title("My App")

def on_button_click():
    print("Button clicked!")
    # Logic mixed directly here

label = ctk.CTkLabel(app, text="Hello!")
label.pack(pady=10)

button = ctk.CTkButton(app, text="Click Me", command=on_button_click)
button.pack(pady=10)

app.mainloop()
```

✅