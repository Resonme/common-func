# common-func
### 深拷贝
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

### 防抖
```
export function debounce(fn, wait) {
  let timer = null
  return function() {
    var args = arguments
    if (timer) {
      clearTimeout(timer)
      timer = null
    }
    timer = setTimeout(function() {
      fn.apply(this, args)
    }, wait)
  }
}
```

### 节流
```
export function throttle(fn, gapTime) {
  let _lastTime = null
  return function() {
    const _nowTime = +new Date()
    if (_nowTime - _lastTime > gapTime || !_lastTime) {
      fn()
      _lastTime = _nowTime
    }
  }
}
```

### 获取地址栏参数
```
export function getUrlParam(param) {
  const reg = new RegExp('(^|&)' + param + '=([^&]*)(&|$)')
  const r = window.location.search.substr(1).match(reg) || window.location.hash.substring((window.location.hash.search(/\?/)) + 1).match(reg)
  if (r != null) {
    return decodeURIComponent(r[2])
  }
}
```

### 数组去重
```
export function arrayBreakRepeat(arr) {
  const result = []
  const obj = {}
  for (const i of arr) {
    if (!obj[i]) {
      result.push(i)
      obj[i] = 1
    }
  }
  return result
}
```

### 数组排序 
```
export function insertSort(arr, field) {
  const len = arr.length
  let preIndex
  let current
  for (var i = 1; i < len; i++) {
    preIndex = i - 1
    current = arr[i]
    while (preIndex >= 0 && arr[preIndex][field] > current[field]) {
      arr[preIndex + 1] = arr[preIndex]
      preIndex--
    }
    arr[preIndex + 1] = current
  }
  return arr
}
```

### 类 历史记录缓存
```
class LyuSave {
  constructor(list = [], CAP = 2) {
    this.list = list
    this.CAP = CAP
  }

  set(key, value) {
    let index = -1
    this.list.some((item, i) => {
      if (item.key === key) {
        index = i
        return true
      } else {
        return false
      }
    })
    if (index !== -1) {
      this.list.splice(index, 1)
    }
    this.list.push({ key, value })
    if (this.list.length > this.CAP) {
      this.list.shift()
    }
    console.log('存储后数组', this.list)
  }

  get(key) {
    let index = -1
    this.list.some((item, i) => {
      if (item.key === key) {
        index = i
        return true
      } else {
        return false
      }
    })
    if (index === -1) {
      return -1
    }
    const listItem = this.list.splice(index, 1)
    this.list.push(listItem[0])
    console.log('获取后数组', this.list)
    return listItem[0].value
  }
}
```

### 类 自定义发布订阅事件
```
export default class EventHandle {
  handles = {}
  // 订阅事件
  on(eventType, handle) {
    // eslint-disable-next-line no-prototype-builtins
    if (!this.handles.hasOwnProperty(eventType)) {
      this.handles[eventType] = []
    }
    if (typeof handle === 'function') {
      this.handles[eventType].push(handle)
    } else {
      throw new Error('缺少回调函数')
    }
    return this
  }

  // 发布事件
  emit(eventType, ...args) {
    // eslint-disable-next-line no-prototype-builtins
    if (this.handles.hasOwnProperty(eventType)) {
      this.handles[eventType].forEach((item, key, arr) => {
        item.apply(null, args)
      })
    } else {
      throw new Error(`"${eventType}"事件未注册`)
    }
    return this
  }

  // 删除事件
  off(eventType, handle) {
    // eslint-disable-next-line no-prototype-builtins
    if (!this.handles.hasOwnProperty(eventType)) {
      throw new Error(`"${eventType}"事件未注册`)
    } else if (typeof handle !== 'function') {
      throw new Error('缺少回调函数')
    } else {
      this.handles[eventType].forEach((item, key, arr) => {
        if (item === handle) {
          arr.splice(key, 1)
        }
      })
    }
    return this // 实现链式操作
  }

  offAll() {
    this.handles = {}
  }
}
```
