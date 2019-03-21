---
title: Echarts 图表tooltip的自动播放
date: 2018-07-22 15:56:14
description: Echarts 图表tooltip的自动播放
categories:
- Echarts
tags: 
- Echarts 图表tooltip的自动播放
toc: true 文章目录
---

+ Echarts 图表tooltip的自动播放

<!-- more -->
<The rest of contents | 余下全文>


```
// 写一个函数，参数为chart图表的ref参数， 自动播放的间隔时间time
// 注意chart的option配置项series 要写成数据的形式，如果为对象 修改函数里面series的判断
autoPlayToopTip (chartRef, time = 1500) {
        let dataIndex = -1
        let dataLen = 0
        if (typeof chartRef !== 'undefined' && typeof chartRef.options.series !== 'undefined') {
            if (chartRef.options.series.length > 0 && typeof chartRef.options.series[0].data !== 'undefined') {
                dataLen = chartRef.options.series[0].data.length
            }
            setInterval(() => {
                chartRef.dispatchAction({
                    type: 'downplay',
                    playState: true,
                    seriesIndex: 0,
                    dataIndex
                })
                dataIndex = (dataIndex + 1) % dataLen
                chartRef.dispatchAction({
                    type: 'highlight',
                    playState: true,
                    seriesIndex: 0,
                    dataIndex
                })
                // 显示 tooltip
                chartRef.dispatchAction({
                    type: 'showTip',
                    seriesIndex: 0,
                    playState: true,
                    dataIndex
                })
            }, time)
        }
    },
```
