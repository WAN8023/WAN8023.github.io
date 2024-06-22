## 快捷键

长按 `Ctrl` + `Shift` 会用默认浏览器打开Microsoft365或Office应用。

以管理员运行CMD窗口，执行：

```shell
REG ADD HKCU\Software\Classes\ms-officeapp\Shell\Open\Command /t REG_SZ /d rundll32
```

---

