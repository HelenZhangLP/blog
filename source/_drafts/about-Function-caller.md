---
title: about Function.caller
tags:
- JavaScript
---

## Function.caller
## <font color="#f33">Deprecated</font>
Be aware that this feature may cease to work at any time.
function.caller 返回调用指定函数的函数

```javascript
let blackList = {
  "deleteTaskById": 1,
  "addUserInfo": 1
}
let mId,pId
/**
 * Function.caller.name
 * 获取调用 wxRequest 函数的函数名称
 * 
 * 场景
 * 记录接口调用的方法名称到 options.callerName中，在requestP中会根据黑名单决定请求是否可以发请求
 * 
 */
function wxRequest(options = {}) {
  options.callerName = wxRequest.caller.name;
}

// 黑名单中的请求处理： 写操作的接口 并且 当前不是本人
function requestP(mId, pId) {
  if (blackList[callerName] && mId && mId !== pId) {
    return Promise.reject({
      code: "DAM",
      message: toastTx.cantModify
    });
  }
}
```
