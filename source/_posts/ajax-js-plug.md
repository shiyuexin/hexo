---
title: 原生js封装ajax插件
date: 2017-04-17
categories: ajax
description: 想必用过jquery的ajax的人都会觉得相当方便，很多项目中并不允许我们引入jquery库，或者当我们主要为了用他的ajax方法时，用原生javascript封装一个类似$.ajax的插件也是不错的选择。
---


### 【ajax】

```
/* 封装ajax函数
 * @param {string}opt.type http连接的方式，包括POST和GET两种方式
 * @param {string}opt.url 发送请求的url
 * @param {boolean}opt.async 是否为异步请求，true为异步的，false为同步的
 * @param {object}opt.data 发送的参数，格式为对象类型
 * @param {function}opt.success ajax发送并接收成功调用的回调函数
 */
 
 
var Ajax = {
    //ajax模块
    init: function(obj) {
        //初始化数据
        var objAdapter = {
                url: '',
                method: 'get',
                data: {},
                success: function() {},
                complete: function() {},
                error: function(s) {
                    alert('status:' + s + 'error!');
                },
                async: true
            }
            //通过使用JS随机字符串解决IE浏览器第二次默认获取缓存的问题
        objAdapter.url = obj.url + '?rand=' + Math.random();
        objAdapter.method = obj.method || objAdapter.method;
        objAdapter.data = Ajax.params(obj.data) || Ajax.params(objAdapter.data);
        objAdapter.async = obj.async || objAdapter.async;
        objAdapter.complete = obj.complete || objAdapter.complete;
        objAdapter.success = obj.success || objAdapter.success;
        objAdapter.error = obj.error || objAdapter.error;
        return objAdapter;
    },
    //创建XMLHttpRequest对象
    createXHR: function() {
        if(window.XMLHttpRequest) { //IE7+、Firefox、Opera、Chrome 和Safari
            return new XMLHttpRequest();
        } else if(window.ActiveXObject) { //IE6 及以下
            var versions = ['MSXML2.XMLHttp', 'Microsoft.XMLHTTP'];
            for(var i = 0, len = versions.length; i < len; i++) {
                try {
                    return new ActiveXObject(version[i]);
                    break;
                } catch(e) {
                    //跳过
                }
            }
        } else {
            throw new Error('浏览器不支持XHR对象！');
        }
    },
    params: function(data) {
        var arr = [];
        for(var i in data) {
            //特殊字符传参产生的问题可以使用encodeURIComponent()进行编码处理
            arr.push(encodeURIComponent(i) + '=' + encodeURIComponent(data[i]));
        }
        return arr.join('&');
    },
    callback: function(obj, xhr) {
        if(xhr.status == 200) { //判断http的交互是否成功，200表示成功
            obj.success(xhr.responseText); //回调传递参数
        } else {
            alert('获取数据错误！错误代号：' + xhr.status + '，错误信息：' + xhr.statusText);
        }
    },
    ajax: function(obj) {
        if(obj.method === 'post') {
            Ajax.post(obj);
        } else {
            Ajax.get(obj);
        }
    },
    //post方法
    post: function(obj) {
        var xhr = Ajax.createXHR(); //创建XHR对象
        var opt = Ajax.init(obj);
        opt.method = 'post';
        if(opt.async === true) { //true表示异步，false表示同步
            //使用异步调用的时候，需要触发readystatechange 事件
            xhr.onreadystatechange = function() {
                if(xhr.readyState == 4) { //判断对象的状态是否交互完成
                    Ajax.callback(opt, xhr); //回调
                }
            };
        }
        //在使用XHR对象时，必须先调用open()方法，
        //它接受三个参数：请求类型(get、post)、请求的URL和表示是否异步。
        xhr.open(opt.method, opt.url, opt.async);
        //post方式需要自己设置http的请求头，来模仿表单提交。
        //放在open方法之后，send方法之前。
        xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        xhr.send(opt.data); //post方式将数据放在send()方法里
        if(opt.async === false) { //同步
            Ajax.callback(obj, xhr); //回调
        }
    },
    //get方法
    get: function(obj) {
        var xhr = Ajax.createXHR(); //创建XHR对象
        var opt = Ajax.init(obj);
        if(opt.async === true) { //true表示异步，false表示同步
            //使用异步调用的时候，需要触发readystatechange 事件
            xhr.onreadystatechange = function() {
                if(xhr.readyState == 4) { //判断对象的状态是否交互完成
                    Ajax.callback(obj, xhr); //回调
                }
            };
        }
        //若是GET请求，则将数据加到url后面
        opt.url += opt.url.indexOf('?') == -1 ? '?' + opt.data : '&' + opt.data;
        //在使用XHR对象时，必须先调用open()方法，
        //它接受三个参数：请求类型(get、post)、请求的URL和表示是否异步。
        xhr.open(opt.method, opt.url, opt.async);
        xhr.send(null); //get方式则填null
        if(opt.async === false) { //同步
            Ajax.callback(obj, xhr); //回调
        }
    }
};

```
