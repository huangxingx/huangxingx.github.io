---
title: gorm 中使用 json
date: 2019-06-11 16:55:56
tags:
- golang
- gorm
categories:
- golang
---

### Json 类的实现

```golang
package models

import (
	"bytes"
	"database/sql/driver"
	"errors"
)

type JSON []byte

func (j JSON) Value() (driver.Value, error) {
	if j.IsNull() {
		return nil, nil
	}
	return string(j), nil
}
func (j *JSON) Scan(value interface{}) error {
	if value == nil {
		*j = nil
		return nil
	}
	s, ok := value.([]byte)
	if !ok {
		errors.New("Invalid Scan Source")
	}
	*j = append((*j)[0:0], s...)
	return nil
}
func (m JSON) MarshalJSON() ([]byte, error) {
	if m == nil {
		return []byte("null"), nil
	}
	return m, nil
}
func (m *JSON) UnmarshalJSON(data []byte) error {
	if m == nil {
		return errors.New("null point exception")
	}
	*m = append((*m)[0:0], data...)
	return nil
}
func (j JSON) IsNull() bool {
	return len(j) == 0 || string(j) == "null"
}
func (j JSON) Equals(j1 JSON) bool {
	return bytes.Equal([]byte(j), []byte(j1))
}

```

### 在自定义 Model 中使用 Json Type

```golang
package models
type Model struct {
    ID        int   `gorm:"primary_key" json:"id"`
    CreatedAt int64 `json:"createdAt"`
    UpdatedAt int64 `json:"updatedAt"`
    Object    JSON  `sql:"type:json" json:"object,omitempty"`
}
```

