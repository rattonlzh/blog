# vscode处理git冲突

一般本地仓库和远程仓库的在相同位置内容不同时，合并远程分支（git pull）就会出现conflict， 就会生成
``` 
<<<<HEAD
你的内容
========
远程仓库的内容
>>>>>>冲突提交的SHA值
```

![image-20200628203951747](upload/image-20200628203951747.png)

合并分支出现冲突时， 文本冲突的内容会在对应位置同时显示并以不同颜色高亮，你可以手动选择采用哪个更改
