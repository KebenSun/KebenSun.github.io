---
title: Linux Mint 18 安装搜狗输入法
date: 2018-04-28 15:39:21
categories: 开发环境
tags: 
- Linux 中文输入法
description: 如何在linux mint 18系统上安装搜狗输入法
---
最近在使用Linux Mint 18作为开发环境，正好搜狗输入法也支持linux系统，所以本文记录安装过程
   因为Linux Mint 18 是基于Ubuntu 16，所以本文也适用于Ubuntu 16及以上系统

## 安装Fcitx输入法

   *因为搜狗输入法是基于Fctix输入法，所以需要先安装Fctix*

   * 打开System Settings
   {% asset_img SystemSettings.png System Settings %}

   * 进入Input Method，点击Fctix后面的Add support for Fctix，安装完成后如下图所示
   {% asset_img LanguageSettings.png Language Settings %}

## 安装搜狗输入法
   * 下载：[搜狗输入法 for linux](https://pinyin.sogou.com/linux/?r=pinyin)

   * 打开进入安装界面，点击Install。安装完成后如下图所示：
   {% asset_img Install.png Install %}

   * 安装完成后重启电脑

## 选择搜狗输入法

   * 打开Fctix configuration
   {% asset_img Selection_002.png Selection_002 %}

   * 点击下方添加按钮
   {% asset_img Selection_003.png Selection_003 %}

   * 找到搜狗拼音并添加到输入法，安装完成
   {% asset_img Selection_004.png Selection_004 %}


## 安装完成后可以使用默认的快捷键ctrl+space切换输入法。

## 安装使用时偶尔会出现不显示候选词框，而只显示一个边框的问题。出现这个问题只需要切换一下搜狗拼音的皮肤即可解决。