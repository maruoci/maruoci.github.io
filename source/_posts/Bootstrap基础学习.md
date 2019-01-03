---
title: Bootstrap基础
date: 2018-12-03 17:38:30
categories:
- 前端
tags:
- Bootstrap
---

>  Bootstrap的学习

## 参考资料

 [Runoob官网](http://www.runoob.com/bootstrap/bootstrap-tutorial.html)
 
<!-- more --> 

## 目的

 整理这篇笔记的目的是为了做出一个主页的页面布局。所以这里我也不再挨个整理各个样式的含义了，主要以完成目标为主。

## 导航栏的使用

 **示例**
 
 ```
 <header>
    <nav class="navbar navbar-default" role="navigation">
        <div class="container-fluid">
            <div class="navbar-header">
                <a class="navbar-brand" href="/">
                    <img class="logo" id="logo" src="{{ url_for('static', filename='images/logo.png')}}" alt="Page">
                </a>
            </div>
            <div>
                <ul class="nav navbar-nav navbar-right">
                    <li class="active"><a href="#">Link1</a></li>
                    <li ><a href="#">Link2</a></li>
                    <li ><a href="#">Link3</a></li>
                </ul>
            </div>
        </div>
    </nav>
 </header>
 ```

 1. class .container .container-fluid的区别。
 2. data-toggle

## 轮播图

 ```
 <div id="myCarousel" class="carousel slide" data-ride="carousel" data-interval="3000">
    <!-- 轮播（Carousel）指标 -->
    <ol class="carousel-indicators">
        <li data-target="#myCarousel" data-slide-to="0" class="active"></li>
        <li data-target="#myCarousel" data-slide-to="1"></li>
        <li data-target="#myCarousel" data-slide-to="2"></li>
    </ol>
    <!-- 轮播（Carousel）项目 -->
    <div class="carousel-inner">
        <div class="item active">
            <img style="width: 100%; height: 760px;" src="{{ url_for('static', filename='images/slide/1.jpg')}}" alt="First slide">
        </div>
        <div class="item">
            <img style="width: 100%; height: 760px;" src="{{ url_for('static', filename='images/slide/2.jpg')}}" alt="second slide">
        </div>
        <div class="item">
            <img style="width: 100%; height: 760px;" src="{{ url_for('static', filename='images/slide/3.jpg')}}" alt="third slide">
        </div>
    </div>
    <!-- 轮播（Carousel）导航<span _ngcontent-c3="" aria-hidden="true" class="glyphicon glyphicon-chevron-right"></span>  -->
    <a class="carousel-control left" href="#myCarousel"
       data-slide="prev">
        <span class="glyphicon glyphicon-chevron-left" aria-hidden="true"></span>
            <span class="sr-only">Previous</span>
    </a>

    <a class="carousel-control right" href="#myCarousel"
       data-slide="next">
         <span class="glyphicon glyphicon-chevron-right" aria-hidden="true"></span>
            <span class="sr-only">Next</span>
    </a>
</div>
 ```
 
 轮播图设置轮播时间，3000代表3秒轮播一次。
 `
 data-ride="carousel" data-interval="3000"
 `
 