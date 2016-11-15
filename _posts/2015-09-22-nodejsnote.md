---
layout: post
title:  "node.js 笔记（全局变量）"
date:   2015-09-22 23:54:31
categories: 代码之术
---

## 全局对象  
	global，如console,process

## 全局变量  
ECMAScript 定义  

- 最外层定义的变量
- 全局对象的属性
- 隐式定义的变量

node.js不可能定义外层变量，一般都是全局对象的属性
<!--more-->
## process 
global对象的属性，用于描述Node.js进程状态的对象  
成员方法：
- process.argv
	命令行参数数组，第一个元素是node，第二个元素是脚本文件名，第三个元素开始是运行参数
- process.stdout
	标准输出流 
- process.stdin 
	标准输入流
	为事件循环设置一项任务，- process.nextTick(callback)
node.js会在下次事件循环调响应时调用callback

## console
提供控制台标准输出
- console.log
- console.error
- console.trace

## util 
### util.inherite
实现对象间原型继承  
	
    util.inherits(Sub,Base);
### util.inspect 
将任意对象转换成字符串 

## events
事件发射器  

	EventEmitter.on(event,lisener)
    //注册时间监听器
    EventEmitter.emit(event,[],[])
    //发射event事件，传入若干参数
    EventEmitter.once(event,listener)
    //为指定事件注册一个单次监听器

error事件 ：遇到异常时发射error事件，没有响应的事件即为异常

	var events = require('events');
    var emitter = new events.EventEmitter();
	emitter.emit('error');
    
## 文件系统fs

	fs.readFile(filename,[encoding],[callback])
    var fs=require('fs');
    fs.readFile('content.txt',function(err,data){
    	if(err){
    		console.log("error");
    	}else{
    		console.log("data");
    	}
    });

console.log("error");
    	}else{
    		console.log("data");
    	}
    });


