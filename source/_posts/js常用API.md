---
title: js常用API
date: 2019-05-27 18:50:49
tags:
- js
top:
categories:
- js
copright:
---
<!-- 已经写好的文章在对应的md文件头部添加top：{number}即可 数值越大优先级越高 -->
### cookie的获取、设置、删除
```javascript
  cookies: {
    //获取
    get(name) {
      let arr, reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
      if (arr = document.cookie.match(reg))
        return (arr[2]);
      else
        return null;
    },
    /**
     * 设置cookie
     * @param c_name 名称
     * @param value  值
     * @param time   时间，s30=30秒，h12=12小时，d7=7天
     * @param path   路径，默认 根 /
     */
    set(c_name, value, time, path = '/') {
      let exp = new Date();
      let strSec = getSec(time);
      exp.setTime(exp.getTime() + strSec * 1);
      document.cookie = c_name + "=" + escape(value) + (!time ? "" : ";expires=" + exp.toGMTString()) + ';path=' + path;

      function getSec(str) {
        let str1 = str.substring(1, str.length) * 1;
        let str2 = str.substring(0, 1);
        if (str2 === "s") {
          return str1 * 1000;
        } else if (str2 === "h") {
          return str1 * 60 * 60 * 1000;
        } else if (str2 === "d") {
          return str1 * 24 * 60 * 60 * 1000;
        }
      }
    },
    //删除
    del(name) {
      let exp = new Date();
      exp.setTime(exp.getTime() - 1);
      let cval = this.get(name);
      if (cval != null)
        document.cookie = name + "=" + cval + ";expires=" + exp.toGMTString();
    }
  },

```

### 清除空格
```javascript
  clearBank(txt) {
    if (typeof  txt === 'string') {
      return txt.replace(/\s/g, '')
    } else {
      console.log('请输入正确的类型')
    }
  },
```

### 解码html
```javascript
htmlDecode(html) {
    return html.replace(/(\&|\&)gt;/g, ">")
        .replace(/(\&|\&)lt;/g, "<")
        .replace(/(\&|\&)quot;/g, "\"");
},
```

### 转译特殊字符
```javascript
escape(str) {
  if (this.isEmpty(str)) {
    return str;
  }
  str = str.replace(/&lt;/g, "<");
  str = str.replace(/&gt;/g, ">");
  str = str.replace(/&acute;/g, "'");
  str = str.replace(/&#45;&#45;/g, "--");
  str = str.replace(/&lt;/g, "<");
  str = str.replace(/&lt;/g, "<");
  str = str.replace(/&quot;/g, '\"');
  str = str.replace(/&acute;/g, "\'");
  return str;
},
```

### 判断登录设备是PC还是手机
```javascript
isPc() {
    let userAgentInfo = navigator.userAgent;
    let Agents = ["Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod"];  //判断用户代理头信息
    let flag = true;
    for (let v = 0; v < Agents.length; v++) {
      if (userAgentInfo.indexOf(Agents[v]) !== -1) {
        flag = false;
        break;
      }
    }
    return flag;   //true为pc端，false为非pc端
},
```

### 清除localstorage时 指定不清除的项 []
```javascript
clearStorage(storeKeyArr) {
    let store = JSON.parse(JSON.stringify(localStorage));
    for(let key in store){
        if(!storeKeyArr.includes(key)){
        localStorage.removeItem(key)
        }
    }
}
```

### 对JSON根据某一对象的属性进行排序
json：JSON数组  attr：排序依据属性  type：默认从高到低
```javascript
orderby(json, attr, type) {
  return json.sort(function (a, b) {
    let value1 = a[attr];
    let value2 = b[attr];
    return type === "down" ? value1 - value2 : value2 - value1;
  })
},
```

### 时间格式化
fmt：格式  obj：Date对象 （yyyy-MM-dd hh:mm:ss.S）
```javascript
dateFormat(fmt, obj) { // author: meizz
  let o = {
    "M+": obj.getMonth() + 1, // 月份
    "d+": obj.getDate(), // 日
    "h+": obj.getHours(), // 小时
    "m+": obj.getMinutes(), // 分
    "s+": obj.getSeconds(), // 秒
    "q+": Math.floor((obj.getMonth() + 3) / 3), // 季度
    "S": obj.getMilliseconds(),
    // 毫秒
  };
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (obj.getFullYear() + "").substr(4 - RegExp.$1.length));
  }
  for (let k in o) {
    if (new RegExp("(" + k + ")").test(fmt)) {
      fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
    }
  }
  return fmt;
},
```
### 获取指定日期的前几天或后几天
```javascript
getThatDate(date, num) {
    let trans_day = "";
    let cur_date = new Date(date);
    let cur_year = new Date(date).getFullYear();
    let cur_month = cur_date.getMonth() + 1;
    let real_date = cur_date.getDate();
    cur_month = cur_month > 9 ? cur_month : ("0" + cur_month);
    real_date = real_date > 9 ? real_date : ("0" + real_date);
    let eT = cur_year + "-" + cur_month + "-" + real_date;
    trans_day = this.transDate(eT, num);
    return trans_day;
},
transDate(dateParameter, num) {
    let translateDate = "",
        dateString = "",
        monthString = "",
        dayString = "";
    translateDate = dateParameter.replace("-", "/").replace("-", "/");
    let newDate = new Date(translateDate);
    newDate = newDate.valueOf();
    newDate = newDate + num * 24 * 60 * 60 * 1000;
    newDate = new Date(newDate);
    if ((newDate.getMonth() + 1).toString().length === 1) {
        monthString = 0 + "" + (newDate.getMonth() + 1).toString();
    } else {
        monthString = (newDate.getMonth() + 1).toString();
    }
    if (newDate.getDate().toString().length === 1) {
        dayString = 0 + "" + newDate.getDate().toString();
    } else {
        dayString = newDate.getDate().toString();
    }
    dateString = newDate.getFullYear() + "-" + monthString + "-" + dayString;
    return dateString;
},
```

### 根据传入时间计算当前日期所在周的开始日期和结束日期
```javascript
getTimeSlot(date) {
  let sWeek;  //开始的星期
  let eWeek;  //结束的星期
  if (date.getDay() === 0) {
    sWeek = date.getDay() + 6;
    eWeek = date.getDay();
  } else {
    sWeek = date.getDay() - 1;
    eWeek = 7 - date.getDay();
  }
  //获取开始日期
  let sTime = date.getTime() - sWeek * 24 * 60 * 60 * 1000;  //获取开始星期的时间戳
  let startDate = new Date(sTime);  //转换时间戳
  let sDate = startDate.getFullYear() + "年" + (startDate.getMonth() + 1) + "月" + startDate.getDate() + "日";
  //获取结束日期
  let eTime = date.getTime() + eWeek * 24 * 60 * 60 * 1000;
  let endDate = new Date(eTime); //时间戳转换
  let eDate = endDate.getFullYear() + "年" + (endDate.getMonth() + 1) + "月" + endDate.getDate() + "日";
  return {
    startDate: startDate,
    endDate: endDate,
    sDate: sDate,
    eDate: eDate
  }
},
```

### 根据传入时间计算当前时间是该月份的第几周,以周日作为节点，d时间对象,必须是系统原生时间对象;
```javascript
calcWeek(d) {
  let now = d.getTime(); //当前时间戳
  let res = {now: d.getTime()}; //组装返回数据
  let oneDayLong = 24 * 60 * 60 * 1000; //一天时间长度
  //根据当前时间获取周日
  let monthHead = new Date(d.getFullYear(), d.getMonth(), 1).getTime(); //月初时间戳
  let monthEnd = new Date(d.getFullYear(), d.getMonth() + 1, 1).getTime() - 24 * 3600 * 1000; //月末时间戳
  let wd;//根据传入时间计算传入日期对应的截止时间戳
  if (d.getDay() === 0) {
    wd = now
  } else {
    wd = now + (7 - d.getDay()) * oneDayLong
  }

  if (wd > monthEnd) {
    //截止大于月底就是下月的第一周
    res.week = 1;
    res.month = new Date(d).getMonth() + 2;
  } else if (new Date(wd).getDate() <= 7) {
    //截止小于7号就是本月第一周
    res.week = 1;
    res.month = new Date(d).getMonth() + 1;
  } else if (new Date(monthHead).getDay() === 1) {
    //本月一号是周一的情况下,截止除以7就是第几周
    res.week = new Date(wd).getDate() / 7;
    res.month = new Date(d).getMonth() + 1;
  } else {
    //截止 日期 减去第一周天数 除以7再加一就是第几周
    let weekDayDate = new Date(wd).getDate(); //截止日期
    let firstWeekDays = new Date(monthHead).getDay(); //当月第一周长度
    if (new Date(monthHead).getDay() === 0) {
      firstWeekDays = 1
    } else {
      firstWeekDays = 8 - new Date(monthHead).getDay()
    }
    res.week = (weekDayDate - firstWeekDays) / 7 + 1;
    res.month = new Date(d).getMonth() + 1;
  }
  return res
},
```

### 上传文件
```javascript
uploader(obj) {
  //变量定义区
  let config=obj;
  // const qiniuUrl = 'https://upload.qiniup.com'; //上传接口
  let exitMsg = {}; //对外消息通知
  let proStatus = true;//前进状态
  //accept属性列表
  let acceptList = {
    "3gpp": "audio/3gpp, video/3gpp",
    ac3: "audio/ac3",
    asf: "allpication/vnd.ms-asf",
    au: "audio/basic",
    css: "text/css",
    csv: "text/csv",
    doc: "application/msword",
    dot: "application/msword",
    dtd: "application/xml-dtd",
    dwg: "image/vnd.dwg",
    dxf: "image/vnd.dxf",
    gif: "image/gif",
    htm: "text/html",
    html: "text/html",
    jp2: "image/jp2",
    jpe: "image/jpeg",
    jpeg: "image/jpeg",
    jpg: "image/jpeg",
    js: "text/javascript, application/javascript",
    json: "application/json",
    mp2: "audio/mpeg, video/mpeg",
    mp3: "audio/mpeg",
    mp4: "audio/mp4, video/mp4",
    mpeg: "video/mpeg",
    mpg: "video/mpeg",
    mpp: "application/vnd.ms-project",
    ogg: "application/ogg, audio/ogg",
    pdf: "application/pdf",
    png: "image/png",
    pot: "application/vnd.ms-powerpoint",
    pps: "application/vnd.ms-powerpoint",
    ppt: "application/vnd.ms-powerpoint",
    rtf: "application/rtf, text/rtf",
    svf: "image/vnd.svf",
    tif: "image/tiff",
    tiff: "image/tiff",
    txt: "text/plain",
    wdb: "application/vnd.ms-works",
    wps: "application/vnd.ms-works",
    xhtml: "application/xhtml+xml",
    xlc: "application/vnd.ms-excel",
    xlm: "application/vnd.ms-excel",
    xls: "application/vnd.ms-excel",
    xlt: "application/vnd.ms-excel",
    xlw: "application/vnd.ms-excel",
    xml: "text/xml, application/xml",
    zip: "aplication/zip",
    xlsx: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet",
    xltx: "application/vnd.openxmlformats-officedocument.spreadsheetml.template",
    potx: "application/vnd.openxmlformats-officedocument.presentationml.template",
    ppsx: "application/vnd.openxmlformats-officedocument.presentationml.slideshow",
    pptx: "application/vnd.openxmlformats-officedocument.presentationml.presentation",
    sldx: "application/vnd.openxmlformats-officedocument.presentationml.slide",
    docx: "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
    dotx: "application/vnd.openxmlformats-officedocument.wordprocessingml.template",
    xlsm: "application/vnd.ms-excel.addin.macroEnabled.12",
    xlsb: "application/vnd.ms-excel.sheet.binary.macroEnabled.12",
  };
  //创建input元素
  let upIpt = document.createElement('input');
  let body = document.querySelector('body');
  upIpt.type = 'file';

  //将可接受的文件类型添加给input
  if (obj.hasOwnProperty('accept') && obj.accept.length > 0) {
    let accetpArr = [];
    let flag = true;
    obj.accept.forEach(function (e) {
      if (acceptList.hasOwnProperty(e)) {
        accetpArr.push(acceptList[e])
      } else {
        errorFn('没有找到文件类型:' + e);
        flag = false
      }
    });
    if (flag) {
      upIpt.accept = accetpArr.join(',')
    } else {
      return false
    }
  }
  upIpt.style = 'display:none';
  body.appendChild(upIpt);

  //监听上传元素值改变
  upIpt.onchange = function () {

    let val = upIpt.value;
    if (val) {

      //上传之前回调
      if (obj.hasOwnProperty('beforeUpload')) {
        obj.beforeUpload();
      }

      //验证文件
      checkFile(upIpt, obj.size);

      //获取token然后开始上传
      if (proStatus) {
        getToken(uploadFile)
      }

    }
  };

  //验证文件
  function checkFile(obj, size) {
    if (size && !/^[1-9]\d*$/.test(size)) {
      errorFn('文件大小必须是正整数');
      return false
    }
    let file;
    let isIE = /msie/i.test(navigator.userAgent) && !window.opera;
    if (isIE && !obj.files) {
      let filePath = obj.value;
      let fileSystem = new ActiveXObject("Scripting.FileSystemObject");
      file = fileSystem.GetFile(filePath);

    } else {
      file = obj.files[0]
    }
  let  fileSize = Math.round(file.size / 1024 * 100) / 100; //单位为KB
    if (size && fileSize >= size) {
      errorFn('上传文件不能超过:' + size + "kb");
      return false
    }
    if(config.hasOwnProperty('checkSlot')){
        if(!config.checkSlot(file)){
          errorFn('自定义条件验证失败');
        }
    }
  }

  // 获取token
  let getToken = function (fn) {
    let xhr = new XMLHttpRequest();
    // 获取七牛token
    // xhr.open("POST", 'https://toolapi.mayte.cn/qt2', true);
    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded;charset=UTF-8");
    xhr.setRequestHeader("Accept", "application/json, text/javascript, */*; q=0.01");
    xhr.setRequestHeader("X-Requested-With", "XMLHttpRequest");
    xhr.send();
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (xhr.status === 200) {
          fn && fn(xhr.responseText)
        } else {
          errorFn('token获取出错,错误代码:' + xhr.status)
        }
      }
    };
  };

  //上传文件
  function uploadFile(token) {
    let file, directory;
    let isIE = /msie/i.test(navigator.userAgent) && !window.opera;
    if (isIE && !upIpt.files) {
      let filePath = upIpt.value;
      let fileSystem = new ActiveXObject("Scripting.FileSystemObject");
      file = fileSystem.GetFile(filePath);
    } else {
      file = upIpt.files[0]
    }
    let form = new FormData();
    form.append('file', file);
    form.append('token', token);
    form.append('x:folder', directory);
    let xhr = new XMLHttpRequest();
    xhr.open('POST', qiniuUrl, true);
    xhr.setRequestHeader('Accept', 'application/json, text/javascript, */*; q=0.01');
    xhr.upload.onprogress = function (event) {
      if (obj.hasOwnProperty('progress')) {
        obj.progress(event)
      }
    };
    xhr.onreadystatechange = function () {
      if (xhr.readyState === 4) {
        if (obj.hasOwnProperty('whatever')) {
          obj.whatever()
        }
        if (xhr.status === 200) {
          if (obj.hasOwnProperty('success')) {
            obj.success(xhr.responseText)
          }
          upIpt.remove();//移除上传标签
        } else {
          errorFn('上传是出错,错误代码:' + xhr.status + " " + xhr.responseText)
        }
      }
    };
    xhr.send(form) //发射
  }

  // 获取文件类型
  function getFileSuffix(item) {
    if (!item) return '';
    let els = item.split('.');
    return (els[els.length - 1].split('-'))[0]
  }

  //虚拟点击
  upIpt.click();

  //抛出错误信息
  function errorFn(d) {
    exitMsg.status = 0;
    exitMsg.msg = d;
    proStatus = false; //不再继续
    upIpt.remove(); //删除上传标签
    if (obj.hasOwnProperty('fail')) {
      obj.fail(exitMsg)
    }
    if (obj.hasOwnProperty('whatever')) {
      obj.whatever()
    }
  }
},
```

### 身份证验证
```javascript
decideIdcard(value) {
  if (value.length === 15) {
    let reg = /^[1-9][0-9]{5}[2-9][0-9](0[1-9]|1[0-2])(0[1-9]|[1-2][0-9]|3[0-1])[0-9]{3}$/;
    return reg.test(value);
  } else if (value.length === 18) {
    let xiaoYan = ["1", "0", "X", "9", "8", "7", "6", "5", "4", "3", "2"];
    let xiao = ["1", "0", "x", "9", "8", "7", "6", "5", "4", "3", "2"];
    let yuShu = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    let xiShu = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2];
    let cardArray = value.split("");

    let sum = 0;
    for (let i = 0; i < cardArray.length - 1; i++) {
      sum += parseInt(cardArray[i]) * xiShu[i];
    }
    let yu = sum % 11;
    for (let i = 0; i < yuShu.length; i++) {
      if (yu === yuShu[i]) {
        if (value.substring(value.length - 1).toUpperCase() === xiaoYan[i]) {
          return true;
        }
      }
    }
  }
  return false;
},
```

### 判断是否为空
```javascript
isEmpty(val) {
  if (typeof val === 'number') {
    return false
  }
  return (!val || val === undefined || typeof val === 'undefined' || val === null || val === 'undefined')
}
```