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
  let context, args;
  let _lastTime = null
  return function() {
    const _nowTime = +new Date()
    context = this;
    args = arguments;
    if (_nowTime - _lastTime > gapTime || !_lastTime) {
      fn.apply(context, args);
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
export function unique(arr) {
    var res = arr.filter(function(item, index, array) {
        return array.indexOf(item) === index
    })
    return res
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



### GIS 获取两个点之间中间点位
```
export function getLineCenter(arrs) {
  var count_lat = 0
  var count_lon = 0
  for (var i = 0; i < arrs.length; i++) {
    count_lon += arrs[i][0]
    count_lat += arrs[i][1]
  }
  var lon_v = count_lon / arrs.length
  var lat_v = count_lat / arrs.length
  return [Number(lon_v.toFixed(8)), Number(lat_v.toFixed(8))]
}
```
### GIS 获取两点之间的距离
```
export function getDistance(lnglat1, lnglat2) {
	var lng1 = lnglat1[0]
	var lat1 = lnglat1[1]
	var lng2 = lnglat2[0]
	var lat2 = lnglat2[1]
	var radLat1 = lat1 * Math.PI / 180.0;
	var radLat2 = lat2 * Math.PI / 180.0;
	var a = radLat1 - radLat2;
	var b = lng1 * Math.PI / 180.0 - lng2 * Math.PI / 180.0;
	var s = 2 * Math.asin(Math.sqrt(Math.pow(Math.sin(a / 2), 2) +
		Math.cos(radLat1) * Math.cos(radLat2) * Math.pow(Math.sin(b / 2), 2)));
	s = s * 6378.137; // EARTH_RADIUS;
	s = Math.round(s * 10000) / 10000;
	return s;
}
```

### 时间格式化
```
export function parseTime(date, fmt = 'yyyy-MM-dd hh:mm:ss') {
  const o = {
    'M+': date.getMonth() + 1, // 月份
    'd+': date.getDate(), // 日
    'h+': date.getHours(), // 小时
    'm+': date.getMinutes(), // 分
    's+': date.getSeconds(), // 秒
    'q+': Math.floor((date.getMonth() + 3) / 3), // 季度
    'S': date.getMilliseconds() // 毫秒
  }
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
  }
  for (const k in o) {
    if (new RegExp('(' + k + ')').test(fmt)) {
      fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? (o[k]) : (('00' + o[k]).substr(('' + o[k]).length)))
    }
  }
  return fmt
}
```

### 树形结构 => 数组
```
export function treeToArr(datas, childrenKey = 'children') {
  const stack = [...datas]
  const data = []
  let pop
  let copyPop
  let children
  while (stack.length !== 0) {
    pop = stack.pop()
    children = pop[childrenKey]
    copyPop = JSON.parse(JSON.stringify(pop))
    delete copyPop[childrenKey]
    data.push(copyPop)
    if (children) {
      for (let i = children.length - 1; i >= 0; i--) {
        stack.push(children[i])
      }
    }
  }
  return data
}
```

### 数组 => 树形结构
```
export function listToTree(list, rootId = 0) {
  const result = []
  const resMap = {}
  let id
  let pid
  list.map(item => {
    id = item.id
    pid = item.parentId
    if (!resMap[id]) {
      resMap[id] = {
        children: []
      }
    }
    resMap[id] = {
      ...item,
      children: resMap[id]['children']
    }
    const treeItem = resMap[id]
    if (pid === rootId) {
      result.push({
        ...treeItem
      })
    } else {
      if (!resMap[pid]) {
        resMap[pid] = {
          children: []
        }
      }
      resMap[pid].push(treeItem)
    }
  })

  return result
}
```

### 数据类型
```
function typeOf(obj) {
  return Object.prototype.toString.call(obj).slice(8, -1).toLowerCase()
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

### 类 全屏
```
class FullScreen {
  /**
   * @description: 全屏初始化
   * @param {Function} fn 用户浏览器不支持全屏的回调
   */
  constructor(fn) {
    this.prefixName = '' // 浏览器前缀
    this.isFullscreenData = true // 浏览器是否支持全屏
    this.isFullscreen(fn)
    this.init()
  }

  full = null
  fullState = false

  init() {
    const obj = {
      enter: e => {
        // 如果退出全屏 退出的还是全屏状态，将会触发进入全屏的回调，这种情况比较少 注意一下
        this.fullState = true
      },
      quit: e => {
        this.fullState = false
        // 通常不会出现嵌套的情况
      }
    }
    this.screenChange(obj.enter, obj.quit)
  }

  /**
   * @description: 将传进来的元素全屏
   * @param {String} domName 要全屏的dom名称
   */
  fullscreen(domName) {
    if (this.fullState) {
      this.exitFullscreen()
      return
    }
    const docElm = domName
      ? document.querySelector(domName)
      : document.documentElement
    if (docElm.requestFullscreen) {
      docElm.requestFullscreen()
    } else if (docElm.mozRequestFullScreen) {
      // FireFox
      docElm.mozRequestFullScreen()
    } else if (docElm.webkitRequestFullScreen) {
      // Chrome等
      docElm.webkitRequestFullScreen()
    } else if (docElm.msRequestFullscreen) {
      // IE11
      docElm.msRequestFullscreen()
    }
  }

  // 退出全屏
  exitFullscreen() {
    const methodName =
      this.prefixName === ''
        ? 'exitFullscreen'
        : `${this.prefixName}ExitFullscreen`
    document[methodName]()
  }
  /**
   * @description: 监听进入/离开全屏
   * @param {Function} enter 进入全屏的回调
   *  @param {Function} quit 离开全屏的回调
   */
  screenChange(enter, quit) {
    if (!this.isFullscreenData) return
    const methodName = `on${this.prefixName}fullscreenchange`
    document[methodName] = e => {
      if (this.isElementFullScreen()) {
        enter && enter(e) // 进入全屏回调
      } else {
        quit && quit(e) // 离开全屏的回调
      }
    }
  }
  /**
   * @description: 浏览器无法进入全屏时触发,可能是技术原因，也可能是用户拒绝：比如全屏请求不是在事件处理函数中调用,会在这里拦截到错误
   * @param {Function} enterErrorFn 回调
   */
  screenError(enterErrorFn) {
    const methodName = `on${this.prefixName}fullscreenerror`
    document[methodName] = e => {
      enterErrorFn && enterErrorFn(e)
    }
  }
  /**
   * @description: 是否支持全屏+判断浏览器前缀
   * @param {Function} fn 不支持全屏的回调函数 这里设了一个默认值
   */
  isFullscreen(fn) {
    let fullscreenEnabled
    // 判断浏览器前缀
    if (document.fullscreenEnabled) {
      fullscreenEnabled = document.fullscreenEnabled
    } else if (document.webkitFullscreenEnabled) {
      fullscreenEnabled = document.webkitFullscreenEnabled
      this.prefixName = 'webkit'
    } else if (document.mozFullScreenEnabled) {
      fullscreenEnabled = document.mozFullScreenEnabled
      this.prefixName = 'moz'
    } else if (document.msFullscreenEnabled) {
      fullscreenEnabled = document.msFullscreenEnabled
      this.prefixName = 'ms'
    }
    if (!fullscreenEnabled) {
      this.isFullscreenData = false
      fn && fn() // 执行不支持全屏的回调
    }
  }
  /**
   * @description: 检测有没有元素处于全屏状态
   * @return 布尔值
   */
  isElementFullScreen() {
    const fullscreenElement =
      document.fullscreenElement ||
      document.msFullscreenElement ||
      document.mozFullScreenElement ||
      document.webkitFullscreenElement
    if (fullscreenElement === null) {
      return false // 当前没有元素在全屏状态
    } else {
      return true // 有元素在全屏状态
    }
  }
}
```
