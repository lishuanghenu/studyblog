---
layout: post
title: "HealthKit(1)"
description: "Study HealthKit 1"
tags: [study, HealthKit]
comments: true
share: true
---
HealthKit是apple迈向个人健康管理的重要一步,对于做健康相关的APP的开发者意义极其重大.对于个人健康相关的数据,它提供了一个统一的管理平台,在这个库中,开发者可以很方便的存储,获取和使用这些数据,并且,它更优越的一点是减少了软硬件开发厂商的耦合性,APP开发者和硬件结合的时候不必一个个的定协议,只要大家都遵循Health的API,就会很方便的调用.

之前很大概的看了一些关于healthkit的内容,也写了一些关于healthkit的内容在公司内部的技术分享中,下面就把这些补上.

HealthKit的类大部分以HK开头
首先介绍的是 HKUnit HK中得单位类
使用很简单 获取一个'g'的单位
{% highlight html linenos %}
{% raw %}
HKUnit *g = [HKUnit gramUnit];
{% endraw %}
{% endhighlight %}

或者有一种更简单的方法
{% highlight html linenos %}
{% raw %}
HKUnit *gPerdL = [HKUnit unitFromString:@"g/dL"];
{% endraw %}
{% endhighlight %}
 
 对于单位转换 HKQuantity已经提供好了现有方法
 HKQuantity 提供了一种封装的量值和计量单位

{% highlight html linenos %}
{% raw %}
HKUnit *gramUnit = [HKUnit gramUnit];
HKQuantity *grams = [HKQuantity quantityWithUnit:gramUnit doubleValue:20];
double kg = [grams doubleValueForUnit:[HKUnit unitFromString:@"kg"]];
{% endraw %}
{% endhighlight %}
比如克转换为千克 升转换为毫升 这些时候使用

HKObjectType 是可以记录以下关于人体健康信息的数据 
1 Steps 计步器
2 Calories 卡路里
3 Oxygen Saturation 血氧饱和度
4 Body Temperature 体温
5 Potassium 钾
6 Vitamin B6  VB6
7 BAC ？？？
8 Body Fat Percentage 体脂百分比
9 Perfusion Index 灌注指数 ？？
10 Vitamin D  VD
11 Vitamin A VA
12 Nike Fuel ？？？
13Vitamin C VC
14 Heart Rate 心脏率
15 Distance 距离
16  Body Mass   体重
17 BMI
18 Blood Pressure 血压
19 RR Interval ？？
20 Vitamin B12 VB12
21 Height 身高
22 Respiratory Rate 呼吸频率
23 Blood Glucose 血糖
等等....

要注意的是 这些不是枚举 而是字符串类型的

例如 stepCount
{% highlight html linenos %}
{% raw %}
HK_EXTERN NSString * const HKQuantityTypeIdentifierStepCount NS_AVAILABLE_IOS(8_0);  
{% endraw %}
{% endhighlight %}

介绍完HKObjectType 下面是HKObject

HKObject:A unique identifier of the receiver in the HealthKit database
它是关于健康数据的基类 继承NSObject 

{% highlight html linenos %}
{% raw %}
/*!
 @property      UUID
 @abstract      A unique identifier of the receiver in the HealthKit database.
 */
@property (readonly, strong) NSUUID *UUID;

/*!
 @property      source
 @abstract      Represents the entity that created the receiver.
 @discussion    The source may be the application or device that created the receiver.
 */
@property (readonly, strong) HKSource *source;

/*!
 @property      metadata
 @abstract      Extra information describing properties of the receiver.
 @discussion    Keys must be NSString and values must be either NSString, NSNumber, or NSDate.
                See HKMetadata.h for potential metadata keys and values.
 */
@property (readonly, copy) NSDictionary *metadata;
{% endraw %}
{% endhighlight %}
主要就是这4个属性 不再赘述 重点的是它的几个子类

1:HKSample:An abstract class representing measurements taken over a period of time.
HKSample有三个属性
{% highlight html linenos %}
{% raw %}
@property (readonly, strong) HKSampleType *sampleType;

@property (readonly, strong) NSDate *startDate;
@property (readonly, strong) NSDate *endDate;
{% endraw %}
{% endhighlight %}
它是获取数据的一个很重要的类 它的子类HKQuantitySample HKCategorySample 我将在下一篇中详细介绍




