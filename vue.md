
### vue initUse  
If installedPlugins数组存在直接return  
If 如果有install并且是function执行install   
Else if 是一个function 执行  
Push到installedPlugins数组  

### initMixins
mergeOptions(parent, child)   
Child.extends mergeOptions  
Child.mixins数组循环mergeOptions  
mergeField 合并字段 先循环parent mergeField》再循环 child !hasOwn(parent,key) mergeField  
initLifecycle(vm)  
initEvents(vm)  
initRender(vm)  
callHook(vm, 'beforeCreate')  
initInjections(vm) // resolve injections before data/props  
initState(vm)  
initProvide(vm) // resolve provide after data/props  
callHook(vm, 'created')  
vm.$mount(vm.$options.el)

### initExtend
定义Vue.extend(extendOptions)  
Sub _init(Options)  
Sub.prototype = Object.create(super.prototype)  
Sub.options = mergeOptions  
Sub initProps initComputed extend mixin use  
循环ASSET_TYPES生命周期 sub[type] = super(type)  
Sub superOptions extendOptions   
Sub sealedOptions = extend({},sbu.options)   

### initAssetRegisters 注册生命周期钩子
!definition options[type+’s’][id]  
===’component’ options_base.extend(definition)  
===’derective’ {bind:definition , update:definition }  
options[type+’s’][id] = definition   

### initState

### observer class{dep:Dep,vmCount:number,value:any} 
observer(value,asRootData) => ob = new Observer(value) ob.vmCount++
observer class=>   Array.isArray(value) ? observeArray(value) : walk(value)
observeArray(value) => for循环 observe(items[i])
walk(value) => for循环 【defineReactive(obj, keys[i])】

### watcher class{vm,deep,lazy,dirty,deps,cb}
vm._watchers.push(this) 放入watchers数组
this.value = this.lazy ? undefined : this.get()

### defineReactive
Object.getOwnPropertyDescriptor(obj,key)赋值给对象属性  
【Object.defineProperty(obj, key)】  
Dep class{target:watcher, subs:[watcher]}  
get=> getter.call(obj) > 【dep.depend()】添加dep  
set=> getter.call(obj) >  setter.call(obj, newVal) > !shallow &&【observe(newVal)】  > 【dep.notify()】  

### initProps  
toggleObserving(false)
循环props调用 defineReative(props, key, value)   
proxy(vm, `_props`, key)  
toggleObserving(true)  

### initMethods  
循环methods bind(methods[key], vm)绑定到vm上

### initData  
Object.keys(data) 循环key 【proxy(vm, `_data`, key)  】
observe(data, true /* asRootData */) 注册观察
proxy =>  Object.defineProperty(target, key, sharedPropertyDefinition) 给data添加key 监听set get

### initComputed  
watchers = _computedWatchers = Object.create(null)
循环computed watchers[key] = new Watcher(vm, getter, noop, computedWatcherOptions) 

### initWatch  
for循环watch createWatcher(vm, key, handler) 【vm.$watch 】
$watch  => watcher = new Watcher(vm, expOrFn, cb, options) 如果immediate cb.call(vm, watcher.value)执行cb

### slot
