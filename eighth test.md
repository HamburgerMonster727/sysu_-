---
title: "字符游戏-智能蛇"
date: 2020-12-26
lastmod: 2020-12-26
draft: false
categories: ["中文", "项目"]
author: "梁冠轩"
---

# 智能蛇

代码地址：https://gitee.com/liangguanxuan/liangguanxuan/tree/master/eatsnake

要实现智能蛇，要在上一次的贪吃蛇的基础上增加自动寻路吃食物和自动避障（身体和墙）的功能。

用3个全局变量来辅助自动行走：

```
char move[30]; //保存生成路径的move
int step; //用于遍历move
int stepnum; //记录当前路径有多少步
```

用一个变量eatfood来判断是否吃到了食物，若吃到了食物，就要生成新的自动路径。

自动生成路径的函数：

```c
int smartMove(){ //生成路径
    int distanceX = foodX - snakeX[snakeLength-1];
    int distanceY = foodY - snakeY[snakeLength-1];
    int i = 0;
    for(int j = 1;j <= abs(distanceX);j++){
        if(distanceX > 0)
            move[i] = 'S';
        else
            move[i] = 'W';
        i++;
    }
    for(int j = 1;j <= abs(distanceY);j++){
        if(distanceY > 0)
            move[i] = 'D';
        else
            move[i] = 'A';
        i++;
    }
    return i-1;
}

```

只实现了自动路径是不够的，因为这个路径的计算是不考虑撞到身体的情况，当蛇的身体过长时蛇头会撞到身体，所以我们要在蛇的移动中判断到撞到身体时要自动调整方向，然后再生成一条新的自动路径，由于加入了障碍墙，所以判断到撞到墙时也需要自动调整方向生成新的路径。

新的snakemove:

```c
int is_over(){
    int x = 1,y = 0;
    int w = is_hitbody(x,y)+is_hitwall(x,y);
    int s = is_hitbody(-x,-y)+is_hitwall(-x,-y);
    int a = is_hitbody(y,x)+is_hitwall(y,x);
    int d = is_hitbody(-y,-x)+is_hitwall(-y,-x);
    if(w > 0 && s > 0 && a > 0 && d > 0)
        return 1;
    return 0;
}
int snakeMove(int x,int y){
    //判断是否已被四周围死
    if(is_over()){
        return 0;
    }
    //判断移动方向是否与先前移动方向相反，则不作方向调整
    if(is_wrongway(x,y)){
        return snakeMove(tmpX,tmpY);
    }
    //判断头是否吃到食物,若吃到,不用移动身体只需把食物变为头部即可
    if(is_eatfood(x,y)){
        tmpX = x,tmpY = y;
        eatfood = 1;
        return 1;
    }
    else{
        //判断头是否撞到墙
        if(is_hitwall(x,y)){ //撞到了墙，要调整方向继续前进
            int flag1 = is_hitwall(y,x);
            int flag2 = is_hitwall(-y,-x);
            int flag3 = is_hitbody(y,x);
            int flag4 = is_hitbody(-y,-x);
            int flag5 = is_hitbody(-x,-y);
            if(flag1 == 1 && flag2 == 1){//左右都为墙
                return 0;
            }
            if(flag1 == 1 && flag2 != 1){//左边为墙
                if(flag4 != 1)
                    snakeMove(-y,-x);
                else //左墙右身体，无法调整方向
                    return 0;
                output();
            }
            if(flag2 == 1 && flag1 != 1){//右边为墙
                if(flag3 != 1)
                    snakeMove(y,x);
                else //右墙左身体，无法调整方向
                    return 0;
                output();
            }
            if(flag1 != 1 && flag2 != 1){ //左右无墙，主要判断身体
                if(flag3 == 1 && flag4 == 1){//左右都为身体
                    if(flag5 != 1){
                        snakeMove(-x,-y);
                        output();
                    }
                    else
                        return 0;
                }
                if(flag3 == 1 && flag4 != 1){//左边为身体
                    snakeMove(-y,-x);
                    output();
                }
                if(flag4 == 1 && flag3 != 1){//右边为身体
                    snakeMove(y,x);
                    output();
                }
                if(flag3 != 1 && flag4 != 1){//左右都无身体
                    snakeMove(y,x);//随便选一个方向前进
                    output();
                }
            }
            stepnum = smartMove(move);
            step = 0;
            return 1;
        }
        //判断头是否与移动后的身体相撞
        if(is_hitbody(x,y)){//撞到了身体，要调整方向
            int flag1 = is_hitbody(y,x);
            int flag2 = is_hitbody(-y,-x);
            int flag3 = is_hitbody(-x,-y);
            if(flag1 == 1 && flag2 == 1){//左右都为身体
                if(flag3 != 1){//后不为身体
                    snakeMove(-x,-y);
                    output;
                }
                else
                    return 0;
            }
            if(flag1 == 1 && flag2 != 1){//左为身体
                snakeMove(-y,-x);
                output();
            }
            if(flag2 == 1 && flag1 != 1){//右为身体
                snakeMove(y,x);
                output();
            }
            if(flag1 != 1 && flag2 != 1){//左右都无身体
                snakeMove(y,x); //随便选一个方向前进
                output();
            }
            stepnum = smartMove(move);
            step = 0;
            return 1;
        }
        else{
            movebody();
            movehead(x,y);
            tmpX = x,tmpY = y;
            return 1;
        }
    }
}
```

为了蛇会自动根据生成的路径行走，对gamestart也需要进行更改。

```c
void gamestart(){
    creatfood();
    stepnum = smartMove();//生成初始路径
    output();
    while(1){
        if(eatfood == 1){ //吃到了食物，生成新的路径
            stepnum = smartMove(); 
            eatfood = 0;
        }
        int start;
        int over = 0;
        for(step = 0;step <= stepnum;step++){ //遍历move
            switch(move[step]){
                case 'W':
                    start = snakeMove(-1,0);             
                    break;
                case 'S':
                    start = snakeMove(1,0);              
                    break;
                case 'A':
                    start = snakeMove(0,-1);            
                    break;
                case 'D':
                    start = snakeMove(0,1);
                    break;
                default:  //输入错误保持先前的方向移动
                    start = snakeMove(tmpX,tmpY);
                    break;
            }
            if(start == 1){
                output();
            }
            if(start == 0){
                over = 1;
                output();
                printf("撞到了墙或者身体，你已死亡\n");
                break;
            }
        }
        if(over == 1)
            break;
    }
}
```

把sleep调为1，可以快速得到智能蛇的最终结果，我的最高记录是27分，可以尝试更高的分数。

![图片说明](/img/hugo/snake.png)