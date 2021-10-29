# common-func
## 深拷贝
```
export function deepCopy(obj) {
  let result
  if (typeof obj === 'object') {
    result = obj.constructor === Array ? [] : {}
    for (var i in obj) {
      result[i] = typeof obj[i] === 'object' ? deepCopy(obj[i]) : obj[i]
    }
  } else {
    result = obj
  }
  return result
}
```
