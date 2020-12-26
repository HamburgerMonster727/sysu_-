---
title: "第五次项目：用伪代码描述算法"
date: 2020-10-28
lastmod: 2020-10-28
draft: false
categories: ["中文", "项目"]
author: "梁冠轩"
---

# 第五次项目：用伪代码描述算法

## 介绍“自顶向下，逐步求精”的编程方法

### 自顶向下

　　自顶向下，Top-Down，指的是对大问题的分解，即将我们遇到的复杂的，所谓“大”的问题，分解为小问题，从而方便我们找出问题的关键、重点、突破口所在，进而用更加清晰的、定性定量的方法去描述、解决问题。

### 逐步求精

　　逐步求精指的是我们对问题的一种映射与转化，即将现实世界中的问题具体化地抽象转化为对应领域、世界的问题；将复杂问题经过抽象化处理，精简、分解为相对简单的小问题，从而实现将整个大问题细化精确到某些简单小问题的解决中。

## 以你观察的洗衣机为案例，用伪代码描述洗衣机洗衣的程序。

假设洗衣机可执行的基本操作如下：

water_in_switch(open_close) // open 打开上水开关，close关闭

water_out_switch(open_close) // open 打开排水开关，close关闭

get_water_volume() //返回洗衣机内部水的高度

motor_run(direction) // 电机转动。left左转，right右转，stop停

time_counter() // 返回当前时间计数，以秒为单位

halt(returncode) //停机，success 成功 failure 失败

### “正常洗衣”程序的步骤：

```
选择 洗衣模式 输入 水位、时间
注水至预设水位
浸泡预设时间
漂洗预设时间
    每个周期  电机转动左三秒、右三秒
排水至水位为0
脱水
  电机快速转动 每周期左100秒右100秒 5个周期
关闭电源
```

### 进一步的伪代码：

```
READ(water_line,soak_time,rinse_time)

WHILE getwatervolume()<water_line
  waterinswitch(open)
ENDWHILE

waterinswitch(close)

SET now=timecounter()
WHILE timecounter()<=now+soak_time
ENDWHILE

SET now=timecounter()
WHLILE timecounter()<=now+rinse_time
  SET now1=timecounter()
  motorrun(left)
  IF timecounter()==now1+3
    motorrun(right)
  ENDIF
  IF timecounter()==now1+6
    motorrun(stop)
  ENDIF
ENDWHILE

WHILE getwatervolume()>0
  wateroutswitch(open)
ENDWHILE

FOR i=1 to 5
  now1=timecounter();
  motorrun(left)
  IF timecounter()==now1+100
    motorrun(right)
  ENDIF
  IF timecounter()==now1+200
    motorrun(stop)
  ENDIF
ENDFOR
wateroutswitch(close)

halt(success)
```