# 启动项目

1. yarn 
2. yarn dev

# 接口说明

## 获取列表

```javascript
axios.get('/list')
```

## 删除

```javascript
axios.delete(`/del/${id}`)
```

## 编辑

```javascript
axios.patch(`/edit/${id}`, {
  name: '姓名',
  place: '籍贯',
})
```
