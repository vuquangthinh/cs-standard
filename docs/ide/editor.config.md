## Config common for all editor (vim, vscode, atom, webstorm)

### Install
- Choice your favorite editor.
- Install plugin: editor config.
- create file .editorconfig in root project.
- Copy all config below.



========== Common config ==========
```
# top-most EditorConfig file
root = true

# Unix-style newlines with a newline ending every file
[*]
end_of_line = lf

# Matches multiple files with brace expansion notation
# Set default charset
[*.js, *.ts, *.tsx, *.ts]
charset = utf-8
indent_style = space
indent_size = 2

# Matches the exact files either package.json or .travis.yml
[{package.json,.travis.yml}]
indent_style = space
indent_size = 2
```
