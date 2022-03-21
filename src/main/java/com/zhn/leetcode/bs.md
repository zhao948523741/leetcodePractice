# 二分查找
### 1.通用框架
```java
public int binarySearch(int [] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    while(...) {
        int mid = left + (right - left) / 2;
        if(nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = ...;
        } else if (nums[mid] > target) {
            right = ...;
        }
    }
    return ...;
}
```

### 2.搜索一个数，存在返回索引，否则返回-1；
```java
public int binarySearch(int [] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    while(left <= right) { // 1
        int mid = left + (right - left) / 2;
        if(nums[mid] == target) {
            return mid;
        } else if(nums[mid] < target) {
            left = mid + 1; // 2
        } else if(nums[mid] > target) {
            right = mid - 1; // 3
        }
    }
    return -1;
}
```
这里1处 <= 而不是 < ，因为初始化的right = nums.length - 1，
而不是 right = nums.length。搜索区间是 [left, right] 闭区间，
right = nums.length 的搜索区间是 [left, right) 是左开右闭的，  
<= 循环的结束条件是left = right + 1。也就是[right + 1, right]，
这个区间没有元素遗漏。而 < 的循环结束条件是 left = right，也就是[right, right]，
right元素没有进入循环，遗漏了right元素。也可以改成left < right, 
但是要在最后多一步判断。
```java
while(left < right) {
        ...
}
return nums[left] == target ? left : -1;
```
2和3处，left = mid + 1，right = mid - 1。因为mid元素已经判断过，
缩小搜索区间，且搜索区间是闭区间，无需再判断mid元素。   
这样写存在问题，无法得到target的左右侧边界，例如
```nums = [1,2,2,2,3]```
这个数组。

### 3.寻找左侧边界的二分搜素
```java
public int leftBound(int [] nums, int target) {
    if(nums.length == 0) {
        return -1;
    }
    int left = 0;
    int right = nums.length; // 注意
    while(left < right) {
        int mid = left + (right - left) / 2;
        if(nums[mid] == target) {
            right = mid;
        } else if(nums[mid] > target) {
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        }
    }
    return left;
}
```


### 寻找右侧边界的二分搜索
```java
public int rightBound(int [] nums, int target) {
    if(nums.length == 0) {
        return -1;
    }
    int left = 0;
    int right = nums.length; // 注意
    while(left < right) {
        int mid = left + (right - left) / 2;
        if(nums[mid] == target) {
            left = mid + 1; // 注意
        } else if(nums[mid] > target) {
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        }
    }  
    return left - 1; // 注意！！！
}
```
这里返回 left - 1，循环结束条件是left = right，并且当nums[mid] == target 时，更新left = mid + 1；
所以最终结束后，nums[left]=nums[right]=nums[mid+1],因为nums[mid] = target时要left+1，因此
最终nums[mid+1]不一定等于target，nums[mid]也就是nums[left-1]才有可能时target。   
但寻找左侧边界时，返回left。原因是，当nums[mid] = target时，更新right = mid，如果找到左侧nums[mid] == target，
最终循环结束 left = right = mid。因此返回left，而寻找右侧边界时，如果找到右侧的边界nums[mid] == target，
则left = right = mid+1。所以返回left-1。

>注意数组越界情况，
> 如果while()循环中left < right，起始条件right = nums.length，
> 循环结束时，left=right，可能存在left = right = nums.length 。
> 
>如果while()循环中写 left <= right 的话，起始条件 right = nums.length - 1，
> 需要注意，循环结束时，left = right + 1，可能存在left = nums.length的情况，
> 因此需要注意数组越界情况，

## 4.二分还有一种通用的理解和模板
b站视频 https://www.bilibili.com/video/BV1d54y1q7k7?t=767.8
把数组看作红蓝两部分，起始时，left在-1，right在nums.length。
```java
int left = -1;
int right = nums.length;
while(left + 1 != right) {
    int mid = (left + right) / 2;
    if(isBlue(mid)) {
        l = m;
    } else {
        r = m;
    }
}
return l or r; // 最终l指向蓝色区域最右边，r指向红色区域最左边，l和r不相等。
```
这个二分模板写法，需要以下步骤：    
首先需要建模红蓝区域，然后需要确定isBlue()函数，然后确定返回时l还是r，最后是后续处理。

