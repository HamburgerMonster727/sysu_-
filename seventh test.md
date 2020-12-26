---
title: "字符游戏-贪吃蛇"
date: 2020-12-18
lastmod: 2020-12-18
draft: false
categories: ["中文", "项目"]
author: "梁冠轩"
---

# 任务一：会动的蛇

代码地址：https://gitee.com/liangguanxuan/liangguanxuan/tree/master/eatsnake

用两个数组分别保存蛇的X坐标和Y坐标，蛇移动时只需对坐标进行更改即可实现会动的蛇。

```c
int snakeX[SNAKE_MAX_LENGTH] = {1,1,1,1,1}; //x轴坐标
int snakeY[SNAKE_MAX_LENGTH] = {1,2,3,4,5}; //y轴坐标
```

当蛇头碰到身体或者墙时，游戏结束，当新输入的移动方向与目前蛇的移动方向相反时，不改变蛇的移动方向，继续沿原方向前进。

**完整代码：**

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <windows.h>
#include <conio.h>
#include <locale.h>

#define SNAKE_MAX_LENGTH 20 //蛇的最大长度
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'

int snakeX[SNAKE_MAX_LENGTH] = {1,1,1,1,1}; //x轴坐标
int snakeY[SNAKE_MAX_LENGTH] = {1,2,3,4,5}; //y轴坐标
int snakeLength = 5; //蛇的现长

int snakeMove(char move);
void output();
void gamestart();

char map[12][12]=
   {"************",
    "*XXXXH     *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "************"};

int snakeMove(char move){
    switch(move){
        case 'W'://往上
            //判断移动方向是否与先前移动方向相反，则不作方向调整
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2]+1 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]){
                return -1;
            }
            //先移动身体
            map[snakeX[0]][snakeY[0]] = BLANK_CELL;
            for(int i = 0;i < snakeLength-1;i++){
                snakeX[i] = snakeX[i+1];
                snakeY[i] = snakeY[i+1];
                map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
            }
            //判断头是否撞到墙
            if(map[snakeX[snakeLength-1]-1][snakeY[snakeLength-1]] == WALL_CELL){
                return 0;
            }
            //判断头是否与移动后的身体相撞
            if(map[snakeX[snakeLength-1]-1][snakeY[snakeLength-1]] == SNAKE_BODY){
                return 0;
            }
            else{
                snakeX[snakeLength-1] = snakeX[snakeLength-1]-1;
                map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                return 1;
            }
            break;
        //以下与向上操作同理
        case 'S':
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2]-1 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]){
                return -1;
            }
            map[snakeX[0]][snakeY[0]] = BLANK_CELL;
            for(int i = 0;i < snakeLength-1;i++){
                snakeX[i] = snakeX[i+1];
                snakeY[i] = snakeY[i+1];
                map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
            }
            if(map[snakeX[snakeLength-1]+1][snakeY[snakeLength-1]] == WALL_CELL){
                return 0;
            }
            if(map[snakeX[snakeLength-1]+1][snakeY[snakeLength-1]] == SNAKE_BODY){
                return 0;
            }
            else{
                snakeX[snakeLength-1] = snakeX[snakeLength-1]+1;
                map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                return 1;
            }
            break;
        case 'A':
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2] 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]+1){
                return -1;
            }
            map[snakeX[0]][snakeY[0]] = BLANK_CELL;
            for(int i = 0;i < snakeLength-1;i++){
                snakeX[i] = snakeX[i+1];
                snakeY[i] = snakeY[i+1];
                map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
            }
            if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]-1] == WALL_CELL){
                return 0;
            }
            if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]-1] == SNAKE_BODY){
                return 0;
            }
            else{
                snakeY[snakeLength-1] = snakeY[snakeLength-1]-1;
                map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                return 1;
            }
            break;
        case 'D':
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2] 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]-1){
                return -1;
            }
            map[snakeX[0]][snakeY[0]] = BLANK_CELL;
            for(int i = 0;i < snakeLength-1;i++){
                snakeX[i] = snakeX[i+1];
                snakeY[i] = snakeY[i+1];
                map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
            }
            if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]+1] == WALL_CELL){
                return 0;
            }
            if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]+1] == SNAKE_BODY){
                return 0;
            }
            else{
                snakeY[snakeLength-1] = snakeY[snakeLength-1]+1;
                map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                return 1;
            }
            break;
        //输入不为AWSD时，会保持原有的方向移动
        default:
            //return -1;
            break;
    }
}

void output(){  //输出地图
    printf("======================\n");
    for(int i = 0;i < 12;i++){
        printf("     ");
        for(int j = 0;j < 12;j++){
            printf("%c",map[i][j]);
        }
        printf("\n");
    }
    Sleep(1000);//控制蛇的速度
}

void gamestart(){
    char move = 'D'; //初始为向右出发
    char tmp;
    output();
    while(1){
        scanf("%c",&move);
        int start = snakeMove(move);
        if(start == 1){
            tmp = move;  //tmp保存先前移动方向
            output();
        }
        if(start == -1){  //-1代表这次输入与蛇先前移动方向相反，所以不改变移动方向
            start = snakeMove(tmp);
            output();
        }
        if(start == 0){
            output();
            printf("撞到了墙或者身体，你已死亡\n");
            break;
        }
    }
}

int main(){
    gamestart();
    return 0;
}
```



# 任务二：会吃的蛇

## 初始版本：

在任务一的基础下，增加void creatfood()函数，当蛇头碰到食物时，蛇身体增长一个单位，然后随机创造新的食物，要保证新的食物不与当前蛇重合。

```c
void creatfood(){ //创造食物
    unsigned int times = (unsigned int)time(NULL);
	srand(times);
    foodX = rand()%9+1;
    foodY = rand()%9+1;
    while(1){
        int flag = 1;
        for(int i = 0;i < snakeLength-1;i++){
            if(foodX == snakeX[i] && foodY == snakeY[i]){  //判断新创造的食物是否与蛇有重合
                unsigned int times = (unsigned int)time(NULL);
	            srand(times*(i+1));
                Sleep(500);
                foodX = rand()%9+1;
                foodY = rand()%9+1;
                flag = 0;
            }
        }
        if(flag == 1){
            break;
        }
    }
    map[foodX][foodY] = SNAKE_FOOD;
}
```

**完整代码：**

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <windows.h>
#include <conio.h>
#include <locale.h>

#define SNAKE_MAX_LENGTH 20 //蛇的最大长度
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'

int snakeX[SNAKE_MAX_LENGTH] = {1,1,1,1,1}; //x轴坐标
int snakeY[SNAKE_MAX_LENGTH] = {1,2,3,4,5}; //y轴坐标
int snakeLength = 5; //蛇的现长
int foodX; //食物的x坐标
int foodY; //食物的y坐标
int point = 0;//得分

void creatfood();
int snakeMove(char move);
void output();
void gamestart();

char map[12][12]=
   {"************",
    "*XXXXH     *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "************"};

int snakeMove(char move){
    switch(move){
        case 'W'://往上
            //判断移动方向是否与先前移动方向相反，则不作方向调整
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2]+1 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]){
                return -1;
            }
            //判断头是否吃到食物,若吃到,不用移动身体只需把食物变为头部即可
            if(map[snakeX[snakeLength-1]-1][snakeY[snakeLength-1]] == SNAKE_FOOD){
                if(snakeLength < SNAKE_MAX_LENGTH){
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_BODY;
                    snakeLength += 1;
                    snakeX[snakeLength-1] = snakeX[snakeLength-2]-1;
                    snakeY[snakeLength-1] = snakeY[snakeLength-2];
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    point++;
                    creatfood();
                    return 1;
                }
                point++;
                creatfood();//创造新的食物
            }
            else{
                //先移动身体
                map[snakeX[0]][snakeY[0]] = BLANK_CELL;
                for(int i = 0;i < snakeLength-1;i++){
                    snakeX[i] = snakeX[i+1];
                    snakeY[i] = snakeY[i+1];
                    map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
                }
                //判断头是否撞到墙
                if(map[snakeX[snakeLength-1]-1][snakeY[snakeLength-1]] == WALL_CELL){
                    return 0;
                }
                //判断头是否与移动后的身体相撞
                if(map[snakeX[snakeLength-1]-1][snakeY[snakeLength-1]] == SNAKE_BODY){
                    return 0;
                }
                else{
                    snakeX[snakeLength-1] = snakeX[snakeLength-1]-1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    return 1;
                }
            }
            break;
        //以下与向上操作同理
        case 'S':
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2]-1 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]){
                return -1;
            }
            if(map[snakeX[snakeLength-1]+1][snakeY[snakeLength-1]] == SNAKE_FOOD){
                if(snakeLength < SNAKE_MAX_LENGTH){
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_BODY;
                    snakeLength += 1;
                    snakeX[snakeLength-1] = snakeX[snakeLength-2]+1;
                    snakeY[snakeLength-1] = snakeY[snakeLength-2];
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    point++;
                    creatfood();
                    return 1;
                }
                point++;
                creatfood();
            }
            else{
                map[snakeX[0]][snakeY[0]] = BLANK_CELL;
                for(int i = 0;i < snakeLength-1;i++){
                    snakeX[i] = snakeX[i+1];
                    snakeY[i] = snakeY[i+1];
                    map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
                }
                if(map[snakeX[snakeLength-1]+1][snakeY[snakeLength-1]] == WALL_CELL){
                    return 0;
                }
                if(map[snakeX[snakeLength-1]+1][snakeY[snakeLength-1]] == SNAKE_BODY){
                    return 0;
                }
                else{
                    snakeX[snakeLength-1] = snakeX[snakeLength-1]+1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    return 1;
                }
            }
            break;
        case 'A':
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2] 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]+1){
                return -1;
            }
            if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]-1] == SNAKE_FOOD){
                if(snakeLength < SNAKE_MAX_LENGTH){
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_BODY;
                    snakeLength += 1;
                    snakeX[snakeLength-1] = snakeX[snakeLength-2];
                    snakeY[snakeLength-1] = snakeY[snakeLength-2]-1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    point++;
                    creatfood();
                    return 1;
                }
                point++;
                creatfood();
            }
            else{
                map[snakeX[0]][snakeY[0]] = BLANK_CELL;
                for(int i = 0;i < snakeLength-1;i++){
                    snakeX[i] = snakeX[i+1];
                    snakeY[i] = snakeY[i+1];
                    map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
                }
                if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]-1] == WALL_CELL){
                    return 0;
                }
                if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]-1] == SNAKE_BODY){
                    return 0;
                }
                else{
                    snakeY[snakeLength-1] = snakeY[snakeLength-1]-1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    return 1;
                }
            }
            break;
        case 'D':
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2] 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]-1){
                return -1;
            }
            if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]+1] == SNAKE_FOOD){
                if(snakeLength < SNAKE_MAX_LENGTH){
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_BODY;
                    snakeLength += 1;
                    snakeX[snakeLength-1] = snakeX[snakeLength-2];
                    snakeY[snakeLength-1] = snakeY[snakeLength-2]+1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    point++;
                    creatfood();
                    return 1;
                }
                point++;
                creatfood();
            }
            else{
                map[snakeX[0]][snakeY[0]] = BLANK_CELL;
                for(int i = 0;i < snakeLength-1;i++){
                    snakeX[i] = snakeX[i+1];
                    snakeY[i] = snakeY[i+1];
                    map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
                }
                if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]+1] == WALL_CELL){
                    return 0;
                }
                if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]+1] == SNAKE_BODY){
                    return 0;
                }
                else{
                    snakeY[snakeLength-1] = snakeY[snakeLength-1]+1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    return 1;
                }
            }
            break;
        //输入不为AWSD时，会保持原有的方向移动
        default:
            return -1;
            break;
    }
}

void creatfood(){ //创造食物
    unsigned int times = (unsigned int)time(NULL);
	srand(times);
    foodX = rand()%9+1;
    foodY = rand()%9+1;
    while(1){
        int flag = 1;
        for(int i = 0;i < snakeLength-1;i++){
            if(foodX == snakeX[i] && foodY == snakeY[i]){  //判断新创造的食物是否与蛇有重合
                unsigned int times = (unsigned int)time(NULL);
	            srand(times*(i+1));
                Sleep(500);
                foodX = rand()%9+1;
                foodY = rand()%9+1;
                flag = 0;
            }
        }
        if(flag == 1){
            break;
        }
    }
    map[foodX][foodY] = SNAKE_FOOD;
}

void output(){  //输出地图
    printf("======================\n");
    printf("     point:%d\n",point);
    for(int i = 0;i < 12;i++){
        printf("     ");
        for(int j = 0;j < 12;j++){
            printf("%c",map[i][j]);
        }
        printf("\n");
    }
    Sleep(700);//控制蛇的速度
}

void gamestart(){
    char move = 'D'; //初始为向右出发
    char tmp;
    creatfood();
    output();
    while(1){
        while(_kbhit()){  //判断键盘是否有输入
            move = getchar();
        }
        int start = snakeMove(move);
        if(start == 1){
            tmp = move;  //tmp保存先前移动方向
            output();
        }
        if(start == -1){  //-1代表这次输入与蛇先前移动方向相反，所以不改变移动方向
            start = snakeMove(tmp);
            output();
        }
        if(start == 0){
            output();
            printf("撞到了墙或者身体，你已死亡\n");
            break;
        }
    }
}

int main(){
    gamestart();
    return 0;
}
```

初始版本的代码十分冗长，代码重复性较高，因此我把一些功能单独提取出来，并对gamestart()和snakeMove()进行了结构调整，使代码更加简洁，易读，从278行代码缩减到207行代码。

## 改良版本：

把判断蛇的状态的几个实现单独拿出来作为函数，把蛇的身体移动，蛇头移动，蛇吃食物也单独拿出来作为函数，再通过对snakeMove的优化，初始版本是获取移动方向在snakeMove中选择状态进行移动，改良后是通过坐标移动的方式，在snakeMove(int x,int y)中传入两个参数x和y，在gamestart中选择状态来输入不同的参数到snakeMove中实现蛇的移动。

**改良的功能函数：**

```c
int snakeMove(int x,int y);
int is_wrongway(int x,int y);
int is_eatfood(int x,int y);
int is_hitbody(int x,int y);
int is_hitwall(int x,int y);
void movehead(int x,int y);
void movebody();
```

**初始的gamestart和snakeMove:**

```c
void gamestart(){
    char move = 'D'; //初始为向右出发
    char tmp;
    creatfood();
    output();
    while(1){
        while(_kbhit()){  //判断键盘是否有输入
            move = getchar();
        }
        int start = snakeMove(move);
        if(start == 1){
            tmp = move;  //tmp保存先前移动方向
            output();
        }
        if(start == -1){  //-1代表这次输入与蛇先前移动方向相反，所以不改变移动方向
            start = snakeMove(tmp);
            output();
        }
        if(start == 0){
            output();
            printf("撞到了墙或者身体，你已死亡\n");
            break;
        }
    }
}

int snakeMove(char move){
    switch(move){
        case 'W'://往上
            //判断移动方向是否与先前移动方向相反，则不作方向调整
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2]+1 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]){
                return -1;
            }
            //判断头是否吃到食物,若吃到,不用移动身体只需把食物变为头部即可
            if(map[snakeX[snakeLength-1]-1][snakeY[snakeLength-1]] == SNAKE_FOOD){
                if(snakeLength < SNAKE_MAX_LENGTH){
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_BODY;
                    snakeLength += 1;
                    snakeX[snakeLength-1] = snakeX[snakeLength-2]-1;
                    snakeY[snakeLength-1] = snakeY[snakeLength-2];
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    point++;
                    creatfood();
                    return 1;
                }
                point++;
                creatfood();//创造新的食物
            }
            else{
                //先移动身体
                map[snakeX[0]][snakeY[0]] = BLANK_CELL;
                for(int i = 0;i < snakeLength-1;i++){
                    snakeX[i] = snakeX[i+1];
                    snakeY[i] = snakeY[i+1];
                    map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
                }
                //判断头是否撞到墙
                if(map[snakeX[snakeLength-1]-1][snakeY[snakeLength-1]] == WALL_CELL){
                    return 0;
                }
                //判断头是否与移动后的身体相撞
                if(map[snakeX[snakeLength-1]-1][snakeY[snakeLength-1]] == SNAKE_BODY){
                    return 0;
                }
                else{
                    snakeX[snakeLength-1] = snakeX[snakeLength-1]-1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    return 1;
                }
            }
            break;
        //以下与向上操作同理
        case 'S':
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2]-1 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]){
                return -1;
            }
            if(map[snakeX[snakeLength-1]+1][snakeY[snakeLength-1]] == SNAKE_FOOD){
                if(snakeLength < SNAKE_MAX_LENGTH){
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_BODY;
                    snakeLength += 1;
                    snakeX[snakeLength-1] = snakeX[snakeLength-2]+1;
                    snakeY[snakeLength-1] = snakeY[snakeLength-2];
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    point++;
                    creatfood();
                    return 1;
                }
                point++;
                creatfood();
            }
            else{
                map[snakeX[0]][snakeY[0]] = BLANK_CELL;
                for(int i = 0;i < snakeLength-1;i++){
                    snakeX[i] = snakeX[i+1];
                    snakeY[i] = snakeY[i+1];
                    map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
                }
                if(map[snakeX[snakeLength-1]+1][snakeY[snakeLength-1]] == WALL_CELL){
                    return 0;
                }
                if(map[snakeX[snakeLength-1]+1][snakeY[snakeLength-1]] == SNAKE_BODY){
                    return 0;
                }
                else{
                    snakeX[snakeLength-1] = snakeX[snakeLength-1]+1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    return 1;
                }
            }
            break;
        case 'A':
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2] 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]+1){
                return -1;
            }
            if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]-1] == SNAKE_FOOD){
                if(snakeLength < SNAKE_MAX_LENGTH){
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_BODY;
                    snakeLength += 1;
                    snakeX[snakeLength-1] = snakeX[snakeLength-2];
                    snakeY[snakeLength-1] = snakeY[snakeLength-2]-1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    point++;
                    creatfood();
                    return 1;
                }
                point++;
                creatfood();
            }
            else{
                map[snakeX[0]][snakeY[0]] = BLANK_CELL;
                for(int i = 0;i < snakeLength-1;i++){
                    snakeX[i] = snakeX[i+1];
                    snakeY[i] = snakeY[i+1];
                    map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
                }
                if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]-1] == WALL_CELL){
                    return 0;
                }
                if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]-1] == SNAKE_BODY){
                    return 0;
                }
                else{
                    snakeY[snakeLength-1] = snakeY[snakeLength-1]-1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    return 1;
                }
            }
            break;
        case 'D':
            if(snakeX[snakeLength-1] == snakeX[snakeLength-2] 
            && snakeY[snakeLength-1] == snakeY[snakeLength-2]-1){
                return -1;
            }
            if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]+1] == SNAKE_FOOD){
                if(snakeLength < SNAKE_MAX_LENGTH){
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_BODY;
                    snakeLength += 1;
                    snakeX[snakeLength-1] = snakeX[snakeLength-2];
                    snakeY[snakeLength-1] = snakeY[snakeLength-2]+1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    point++;
                    creatfood();
                    return 1;
                }
                point++;
                creatfood();
            }
            else{
                map[snakeX[0]][snakeY[0]] = BLANK_CELL;
                for(int i = 0;i < snakeLength-1;i++){
                    snakeX[i] = snakeX[i+1];
                    snakeY[i] = snakeY[i+1];
                    map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
                }
                if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]+1] == WALL_CELL){
                    return 0;
                }
                if(map[snakeX[snakeLength-1]][snakeY[snakeLength-1]+1] == SNAKE_BODY){
                    return 0;
                }
                else{
                    snakeY[snakeLength-1] = snakeY[snakeLength-1]+1;
                    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
                    return 1;
                }
            }
            break;
        //输入不为AWSD时，会保持原有的方向移动
        default:
            return -1;
            break;
    }
}
```

**改良后的gamestart和snakeMove：**

```c
void gamestart(){
    char move = 'D'; //初始为向右出发
    creatfood();
    output();
    while(1){
        while(_kbhit()){  //判断键盘是否有输入
            move = getchar();
        }
        int start;
        switch(move){
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
            output();
            printf("撞到了墙或者身体，你已死亡\n");
            break;
        }
    }
}

int snakeMove(int x,int y){
    //判断移动方向是否与先前移动方向相反，则不作方向调整
    if(is_wrongway(x,y)){
        return snakeMove(tmpX,tmpY);
    }
    //判断头是否吃到食物,若吃到,不用移动身体只需把食物变为头部即可
    if(is_eatfood(x,y)){
        tmpX = x,tmpY = y;
        return 1;
    }
    else{
        //先移动身体
        movebody();
        //判断头是否撞到墙
        if(is_hitwall(x,y)){
            return 0;
        }
        //判断头是否与移动后的身体相撞
        if(is_hitbody(x,y)){
            return 0;
        }
        else{
            movehead(x,y);
            tmpX = x,tmpY = y;
            return 1;
        }
    }
}
```

**完整代码：**

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <windows.h>
#include <conio.h>
#include <locale.h>

#define SNAKE_MAX_LENGTH 20 //蛇的最大长度
#define SNAKE_HEAD 'H'
#define SNAKE_BODY 'X'
#define BLANK_CELL ' '
#define SNAKE_FOOD '$'
#define WALL_CELL '*'

int snakeX[SNAKE_MAX_LENGTH] = {1,1,1,1,1}; //x轴坐标
int snakeY[SNAKE_MAX_LENGTH] = {1,2,3,4,5}; //y轴坐标
int snakeLength = 5; //蛇的现长
int foodX; //食物的x坐标
int foodY; //食物的y坐标
int tmpX;  //先前的x坐标
int tmpY;  //先前的y坐标
int point = 0;//得分

void creatfood();
void output();
void gamestart();
int snakeMove(int x,int y);
int is_wrongway(int x,int y);
int is_eatfood(int x,int y);
int is_hitbody(int x,int y);
int is_hitwall(int x,int y);
void movehead(int x,int y);
void movebody();

char map[12][12]=
   {"************",
    "*XXXXH     *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "*          *",
    "************"};

void gamestart(){
    char move = 'D'; //初始为向右出发
    creatfood();
    output();
    while(1){
        while(_kbhit()){  //判断键盘是否有输入
            move = getchar();
        }
        int start;
        switch(move){
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
            output();
            printf("撞到了墙或者身体，你已死亡\n");
            break;
        }
    }
}

int snakeMove(int x,int y){
    //判断移动方向是否与先前移动方向相反，则不作方向调整
    if(is_wrongway(x,y)){
        return snakeMove(tmpX,tmpY);
    }
    //判断头是否吃到食物,若吃到,不用移动身体只需把食物变为头部即可
    if(is_eatfood(x,y)){
        tmpX = x,tmpY = y;
        return 1;
    }
    else{
        //先移动身体
        movebody();
        //判断头是否撞到墙
        if(is_hitwall(x,y)){
            return 0;
        }
        //判断头是否与移动后的身体相撞
        if(is_hitbody(x,y)){
            return 0;
        }
        else{
            movehead(x,y);
            tmpX = x,tmpY = y;
            return 1;
        }
    }
}

int is_wrongway(int x,int y){
    if(snakeX[snakeLength-1]+x == snakeX[snakeLength-2] && snakeY[snakeLength-1]+y == snakeY[snakeLength-2])
        return 1;
    return 0;
}

int is_eatfood(int x,int y){
    if(map[snakeX[snakeLength-1]+x][snakeY[snakeLength-1]+y] == SNAKE_FOOD){
        if(snakeLength < SNAKE_MAX_LENGTH){
            map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_BODY;
            snakeLength += 1;
            snakeX[snakeLength-1] = snakeX[snakeLength-2]+x;
            snakeY[snakeLength-1] = snakeY[snakeLength-2]+y;
            map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
        }
        else{
            movebody();
            movehead(x,y);
        }
        point++;
        creatfood();//创造新的食物
        return 1;
    }
    return 0;
}

int is_hitbody(int x,int y){
    if(map[snakeX[snakeLength-1]+x][snakeY[snakeLength-1]+y] == SNAKE_BODY)
        return 1;
    return 0;
}

int is_hitwall(int x,int y){
    if(map[snakeX[snakeLength-1]+x][snakeY[snakeLength-1]+y] == WALL_CELL)
        return 1;
    return 0;
}

void movebody(){//身体移动只需从尾巴到头部前一个位置，不断把前一个位置的坐标赋值给后一个位置即可
    map[snakeX[0]][snakeY[0]] = BLANK_CELL;
    for(int i = 0;i < snakeLength-1;i++){
        snakeX[i] = snakeX[i+1];
        snakeY[i] = snakeY[i+1];
        map[snakeX[i]][snakeY[i]] = SNAKE_BODY;
    }
}

void movehead(int x,int y){
    snakeX[snakeLength-1] = snakeX[snakeLength-1]+x;
    snakeY[snakeLength-1] = snakeY[snakeLength-1]+y;
    map[snakeX[snakeLength-1]][snakeY[snakeLength-1]] = SNAKE_HEAD;
}

void creatfood(){ //创造食物
    unsigned int times = (unsigned int)time(NULL);
	srand(times);
    foodX = rand()%9+1;
    foodY = rand()%9+1;
    while(1){
        int flag = 1;
        for(int i = 0;i < snakeLength-1;i++){
            if(foodX == snakeX[i] && foodY == snakeY[i]){  //判断新创造的食物是否与蛇有重合
                unsigned int times = (unsigned int)time(NULL);
	            srand(times*(i+1));
                foodX = rand()%9+1;
                foodY = rand()%9+1;
                flag = 0;
            }
        }
        if(flag == 1){
            break;
        }
    }
    map[foodX][foodY] = SNAKE_FOOD;
}

void output(){  //输出地图
    printf("======================\n");
    printf("     point:%d\n",point);
    for(int i = 0;i < 12;i++){
        printf("     ");
        for(int j = 0;j < 12;j++){
            printf("%c",map[i][j]);
        }
        printf("\n");
    }
    Sleep(700);//控制蛇的速度
}

int main(){
    gamestart();
    return 0;
}
```

# 心得体会：

通过这次贪吃蛇游戏的实验，体验到了自顶向下，逐步求精的思想，通过结构化的调整，功能函数的增加，适当且准确的注释，能把冗长的代码变得简洁易读。

