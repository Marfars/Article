# Xfermode完全使用解析

## BitmapFactory.Options(了解其含义)

- inDensity: 原来的大小

- inTargetDensity: 希望按照变成多大的

- inJustDecodeBounds: 是否只读取边界值

## PorterDuffXfermode(目前仅剩这一种了)

- 查看官方文档

- 一般都会配合离屏缓冲

### 如果实验结果和官方文档不一致，就是两个部分的范围取值不对，要取其交集的最左、最右、最上、最下

## 离屏缓冲

### 首先使用canvas的方法切割区域saveLayer

- 第一个参数为Rect，代表切割范围，尽量越小越好，因为离屏缓冲比较消耗性能

- 第二个参数为paint

### 再绘制完Rect的区域后，使用canvas的restoreToCount将切割的区域还原回去

- 只有一个参数是int类型的count，这个参数一般都是saveLayer的返回值