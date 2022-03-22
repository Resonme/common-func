
### vue initUse  
If installedPlugins数组存在直接return  
If 如果有install并且是function执行install   
Else if 是一个function 执行  
Push到installedPlugins数组  

### initMixins
mergeOptions(parent, child)  
Props inject directives  
Child.extends mergeOptions  
Child.mixins数组循环mergeOptions  
mergeField 合并字段 先循环parent mergeField》循环 child !hasOwn(parent,key) mergeField  

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
### initProps
循环props调用 defineReative(obj, key, value)  
### initMethods
### initData
### initComputed
### initWatch
