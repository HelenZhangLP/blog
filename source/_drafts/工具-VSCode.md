---
title: 工具-VSCode
tags:
- 工具
- Tools
---

## VSCode 配置
<span class="custom-box custom-box-939">VSCode snippets 配置 python 头</span>
1.  右下角`设置` -> `用户代码片段`
2.  弹出的输入框中输入 `python.json`
3.  `python.json` 中添加如下配置：
```python
"HEADER": {
    "prefix": "header",
    "body": [
        "#!/usr/bin/env python",
        "#Author: 'Helen.Zhang'",
        "#Time: $CURRENT_YEAR/$CURRENT_MONTH/$CURRENT_DATE $CURRENT_HOUR:$CURRENT_MINUTE:$CURRENT_SECOND"
    ]
}
```
4. `*.py` 中输入 `header`，执行结果如下：
```
#!/usr/bin/env python
#Author: 'Helen.Zhang'
#Time: 2023/03/03 16:50:11
```
