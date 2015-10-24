title: ECharts - 数据图表的使用
date: 2015.10.16 18:14:23
categories:
  - 笔记随笔
tags:
 - ECharts
 - js
---
![ECharts](/blog/images/article_img/3.png)

# 关于ECharts（[ECharts](http://echarts.baidu.com/doc/about.html)）
ECharts 是百度提供的一款开源、功能强大的数据可视化产品。
主要提供折线图、柱状图、散点图、K线图、饼图、雷达图、地图、和弦图、力导向布局图、仪表盘以及漏斗图。

# 特性
- 拖拽重计算
- 数据视图
- 多图联动
- 值域漫游
- 炫光特效
……

# 准备
下载 ECharts 静态包[【echarts-2.2.7】](http://echarts.baidu.com/build/echarts-2.2.7.zip)，也可以直接使用链接进行加载。建议下载静态包，避免官方 **更新新特性** 时造成图表无法正常呈现的问题。

<!-- more -->

# 使用
ECharts的使用很简单，以官方提供的为例分为下面几步：
1. 新建一个 `test.html` 并放置一个 `div` 来承载图表：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div id="main" style="height: 400px;"></div>
</body>
</html>
```

2. 引入 `echarts.js` ：

```
<script src="http://echarts.baidu.com/build/dist/echarts.js"></script>
```

3. 添加模块加载器配置 echarts 和所需图表的路径（相对路径为从当前页面链接到echarts.js），引入图表文件见[引入 ECharts2](http://echarts.baidu.com/doc/doc.html#引入ECharts2)：

```
<script type="text/javascript">
    // 路径配置
    require.config({
        paths: {
          echarts: 'http://echarts.baidu.com/build/dist'
        }
     });
</script>
```

4. 动态加载echarts和所需图表，回调函数中可以初始化图表并驱动图表的生成，option见[API & Doc](http://echarts.baidu.com/doc/doc.html#Option)：

```
require(
    [
        'echarts',
        'echarts/chart/bar' // 使用柱状图就加载bar模块，按需加载
    ],
    function (ec) {
        // 基于准备好的dom，初始化echarts图表
        var myChart = ec.init(document.getElementById('main'));

        var option = {
            tooltip: {
                show: true
            },
            legend: {
                data:['销量']
            },
            xAxis : [
                {
                    type : 'category',
                    data : ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
                }
            ],
            yAxis : [
                {
                    type : 'value'
                }
            ],
            series : [
                {
                    "name":"销量",
                    "type":"bar",
                    "data":[5, 20, 40, 10, 10, 20]
                }
            ]
        };

        // 为echarts对象加载数据
        myChart.setOption(option);
    }
);
```

5. 最后的样子：
![](http://upload-images.jianshu.io/upload_images/741039-41774d66f5de150e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 重点- option 部分相关配置说明
用户可以在 option 配置里自定义图标的样式。
- title 标题：
![](http://upload-images.jianshu.io/upload_images/741039-4b20a04ddb817324.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
title : {
    text: '某地区蒸发量和降水量',  //文本
    subtext: '纯属虚构'
}
```
- toolbox 便捷的工具：
![](http://upload-images.jianshu.io/upload_images/741039-b144b67c25fb7e82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
toolbox: {
    show : true,  //是否显示工具栏
    feature : {  //特性
        mark : {show: true},  //辅助线
        dataView : {show: true, readOnly: false},  //数据视图
        magicType : {show: true, type: ['line', 'bar']},  //切换视图（折线/柱状）
        restore : {show: true},  //重新加载视图
        saveAsImage : {show: true}  //保存该视图为图片
    }
}
```
- series 数据列表：
```
series : [
{
    name:'蒸发量',  //名称
    type:'bar',  //视图类型
    data:[2.0, 4.9, 7.0, 23.2, 25.6, 76.7, 135.6, 162.2, 32.6, 20.0, 6.4, 3.3],  //数据
    markPoint : {  //标记
        data : [
            {type : 'max', name: '最大值'},  //最大值，name为显示文本
            {type : 'min', name: '最小值'}  //最小值，name为显示文本
        ]
    },
    markLine : {  //标线
        data : [
            {type : 'average', name: '平均值'}  //平均值，name为显示文本
        ]
    }
}
```
- xAxis ： X 轴
- yAxis ： Y 轴
- legend ：
![](http://upload-images.jianshu.io/upload_images/741039-7b625b354b1d39bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
legend: {
    orient : 'vertical',  //方向“垂直”，默认从左向右横向排列
    x : 'left',  //位于 X 轴左侧，默认顶部居中
    data:['直接访问','邮件营销','联盟广告','视频广告','搜索引擎']  //显示文本
}
```

更多配置可在[【实例】](http://echarts.baidu.com/doc/example.html)中点开测试。

# 参考
- [入门教程](http://echarts.baidu.com/doc/start.html)
- [实例](http://echarts.baidu.com/doc/example.html)
- [Fork on github](https://github.com/ecomfe/echarts)