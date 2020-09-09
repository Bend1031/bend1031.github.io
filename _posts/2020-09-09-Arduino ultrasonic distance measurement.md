---
layout: post
title: Arduino 超声测距
date: 2020-09-09
author: Bend
header-img: img/post-bg-arduino.png
catalog: true
tags:
- Arduino
---
**功能模块：Arduino、SR04超声波传感器**、杜邦线

```c++
//功能：利用SR04超声波传感器进行测距，并用串口显示测出的距离值

// 设定SR04连接的Arduino引脚
const int TrigPin = 4;
const int EchoPin = 2;

float distance;

void setup()
{
  // 初始化串口通信及连接SR04的引脚
  Serial.begin(9600);
  pinMode(TrigPin, OUTPUT);
  // Trig设置为输出状态
  pinMode(EchoPin, INPUT);
  // 要检测引脚上输入的脉冲宽度，需要先设置为输入状态
  Serial.println("Ultrasonic sensor:");
}
void loop()
{
  // 产生一个10us的高脉冲去触发TrigPin
  digitalWrite(TrigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(TrigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(TrigPin, LOW);
  // 检测脉冲宽度，并计算出距离
  distance = pulseIn(EchoPin, HIGH) * 0.017;//0.017为单位换算系数
  Serial.print("Reliable distance: ");
  Serial.print(distance);
  delay(1000);
  }
}
```
