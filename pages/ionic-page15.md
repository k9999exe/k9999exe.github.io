---
layout: content
---

# ionic之$ionicHistory
<br/>
定义：当用户通过导航栏切换视图页面的时候，ionicHistory起到跟踪视图的作用，类似的浏览器的行为方式，一个ionic应用程序能够保持以前的视图，当前视图，和前视图（如果有一个）。然而，一个典型的Web浏览器只跟踪一个历史堆栈在一个线性的方式。不同于传统的浏览器环境中，应用程序和应用程序并行的独立的历史，如标签。如果一个用户在一个标签上浏览几页，然后切换到一个新的标签和回退，返回按钮与以前的标签，但到以前的页面访问在该标签。因为ionicHistory有利于并行历史架构

## var historyData=$ionicHistory.viewHistory();
　　返回该应用程序的视图历史数据，如所有的视图和历史记录，以及它们如何在导航堆栈中一起有序和链接的方式

## var currentViewData=$ionicHistory.currentView()
　　返回当前视图数据

## var currentHistoryId = $ionicHistory.currentHistoryId()
　　返回历史堆栈的标识，它是当前视图的父容器

## var currentTitle = $ionicHistory.currentTitlt([val])
　　返回当前视图的标题 或者是设置当前视图的标题

## var backView = $ionicHistory.backView()
　　返回当前视图的后一个视图
　　$ionicHistory.backView().stateName
　　返回当前视图的后一个视图名

## var backViewTitle = $ionicHistory.backViewTitle()
　　返回当前视图的后一个视图的标题

## var forwardView = $ionicHistory.forwardView()
　　返回当前视图前一个视图数据 （如果有）

　　$ionicHistory.forwardView().stateName

　　返回当前视图的前一个视图名

## var currentStateName = $ionicHistory.currentStateName()
　　返回当前视图的状态名称

## $ionicHistory.goBack([backCount])
　　导航到应用程序的返回视图（加入视图存在）backCount填写负数

## $ionicHistory.removeBackView()
　　移除当前视图的前一个视图，包括缓存元素和范围（如果它们存在的话）。

## $ionicHistory.clearHistory()
　　清除应用程序的整个历史，除了当前视图。

## var promise = $ionicHistory.clearCache(stateIds)
　　清除缓存,传入参数，stateIds是一个数组，清除缓存的列表