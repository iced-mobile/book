`rust-analyzer` can be configured to build an Android target

- Vscode: in `.vscode/settings.json`
  ```json
  {
    "rust-analyzer.cargo.target": "aarch64-linux-android"
  }
  ```
- Zed: in `.zed/settings.json`
  ```json
  {
    "lsp": {
      "rust-analyzer": {
        "initialization_options": {
          "cargo": {
            "target": "aarch64-linux-android"
          }
        }
      }
    }
  }
  ```
