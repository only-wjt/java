---
title: hashCode()&equals()&==
tags: basic
grammar_cjkRuby: true
---

## 简介
1. 首先equals()和hashcode()这两个方法都是从object类中继承过来的。 
equals()方法在object类中定义如下： 
  public boolean equals(Object obj) { 
return (this == obj); 
} 
