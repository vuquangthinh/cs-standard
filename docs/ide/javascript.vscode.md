# Auto fix eslint, support auto format and auto save

{
  "eslint.packageManager": "yarn",
  "editor.tabSize": 4,
  "editor.detectIndentation": false,
  "editor.rulers": [120],
  "files.insertFinalNewline": true,
  "files.autoSave": "onFocusChange",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
