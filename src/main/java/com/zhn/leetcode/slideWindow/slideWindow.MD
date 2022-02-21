## 滑动窗口模板
### 最长
初始化left，right，currResult，bestResult  
```
while(右指针没有到结尾){  
    窗口扩大，加入right对应的元素，更新当前currResult;
    while(currResult不满足要求){
        窗口缩小，删除left元素，left右移；
    }
    更新最优结果bestResult；
    right++
}
return bestResult；
```

### 最短
初始化left，right，currResult，bestResult
```
while(右指针没有到结尾){  
    窗口扩大，加入right对应的元素，更新当前currResult;
    while(currResult满足要求){
        更新最优结果bestResult；
        窗口缩小，删除left元素，left右移；
    }
    right++
}
return bestResult；
```
