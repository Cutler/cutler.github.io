title: 优化篇　第三章 内存优化 —— 系统内存
date: 2016-7-15 10:40:53
categories: Android开发 - 不屈白银
---
# 第三节 系统内存优化 #
　　本节将会介绍系统内存优化（进程内存、系统流畅度等）的相关知识。

## 系统流畅度 ##
　　我们常说的流畅度低就是指APP卡顿，也就是丢帧。通常影响流畅度有如下几个因素（但不限于）：

	1、APP的主线程被阻塞，导致主线程没有足够的时间去绘制界面，或者直接导致ANR。
	2、APP的界面控件多且复杂，导致主线程虽然不忙但是依然无法在16ms内绘制完一帧。
	3、GC频繁，因为GC触发时虚拟机会先暂停所有线程后再执行GC操作，所以当内存泄露、内存抖动等问题发生时，也会导致丢帧发生。
	4、操作系统内存不足，当APP向系统申请更多内存时（比如加载大图片），若系统内存不足则系统会执行去杀后台进程等操作回收内存，这就会导致APP阻塞，进而丢帧。
	5、CPU饥渴，不论是系统APP还是普通APP，如果其内部开启大量线程执行耗时任务，则就会导致丢帧。道理很简单，同一时间干的事情越多，越容易让主线程得不到CPU去绘制界面。其中SystemServer进程里的各类服务忙碌的话，还会导致AMS等无法为普通APP服务，进而导致普通APP阻塞、丢帧。

<br>　　本节就来介绍如何分析、定位出上面说的各种情况，非系统战斗人员请撤离，或者先去阅读[《第四章 Linux》](http://cutler.github.io/android-BY-C01/)。


<br>　　暂缓，我先去写[《第四章 Linux》](http://cutler.github.io/android-BY-C01/)，然后再回来。 by 2016-07-15。






















<br><br>