---
title: "Go Error Wrapping"
date: 2026-02-12
draft: false
description: "Wrap errors in Go with fmt.Errorf and %w for proper error chains."
tags: ["go", "errors"]
categories: ["backend"]
---

Wrap errors with `%w` to preserve the error chain for `errors.Is` and `errors.As`:

```go
func readConfig(path string) (*Config, error) {
    data, err := os.ReadFile(path)
    if err != nil {
        return nil, fmt.Errorf("readConfig %s: %w", path, err)
    }

    var cfg Config
    if err := json.Unmarshal(data, &cfg); err != nil {
        return nil, fmt.Errorf("readConfig unmarshal: %w", err)
    }

    return &cfg, nil
}
```

Check the chain with `errors.Is`:

```go
if errors.Is(err, os.ErrNotExist) {
    // handle missing file
}
```
