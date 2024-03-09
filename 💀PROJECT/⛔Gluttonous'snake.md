# 思路
 - 首先，我们需要创建一个二维数组来表示游戏地图，用不同的数字来区分空白、墙壁、食物和蛇身。 - 其次，我们需要用一个链表来表示蛇的身体，每个节点存储蛇身的坐标和方向。链表的头节点是蛇头，尾节点是蛇尾。
 - 然后，我们需要用一个循环来控制游戏的流程，每次循环中，我们要做以下几件事： - 获取用户输入的方向，并更新蛇头的方向。 
 - 根据蛇头的方向和坐标，判断是否撞墙或咬到自己，如果是，则游戏结束。 
 - 根据蛇头的方向和坐标，判断是否吃到食物，如果是，则在链表头部添加一个新节点，并在地图上随机生成一个新食物。 
 - 如果没有吃到食物，则在链表头部添加一个新节点，并删除尾部节点。这样就实现了蛇的移动效果。 - 更新地图上蛇身的位置，并打印出地图。 

1.  设计游戏界面和游戏规则：游戏界面包括贪吃蛇、食物、游戏得分等，游戏规则包括贪吃蛇移动、吃食物、撞墙或自身死亡等。
    
2.  编写代码实现游戏初始化：创建游戏窗口、初始化贪吃蛇和食物等。
    
3.  编写代码实现贪吃蛇的移动：根据用户的输入控制贪吃蛇的移动方向，并更新贪吃蛇的位置。
    
4.  编写代码实现食物的生成和吃掉：随机生成食物并判断贪吃蛇是否吃到了食物，若吃到则更新得分并增加贪吃蛇的长度。
    
5.  编写代码实现游戏结束的判断：判断贪吃蛇是否撞墙或自身死亡（吃太饱），如果是则游戏结束。
    
6.  编写代码实现游戏的循环：在游戏未结束的情况下不断更新游戏状态并重新渲染游戏界面。
    
7.  调试和优化代码：检查代码中可能存在的错误并进行优化，提高游戏的运行效率和稳定性。


贪吃蛇项目主要用到了c语言的循环，函数，指针，结构体，枚举，联合，文件操作，简单的数据结构等知识
-   设计一个地图和一个蛇的数据结构。地图可以用二维数组表示，蛇可以用链表表示。
-   初始化地图和蛇，并在地图上随机生成食物。
-   使用键盘控制蛇的移动方向，并判断是否吃到食物或撞到墙壁。
-   如果吃到食物，则增加蛇的长度，并重新生成食物。
-   如果撞到墙壁或自己，则游戏结束，并显示得分。
-   使用光标定位和颜色设置来优化界面效果。



# 参考
- 其实这个需要第三方库SDL,只能看个思路,源代码没啥用
```c
#include <SDL2/SDL.h>
#include<stdio.h>
#define SCREEN_WIDTH 640
#define SCREEN_HEIGHT 480
#define SNAKE_SIZE 20

typedef struct node {
    int x;
    int y;
    struct node *next;
} NODE;

typedef struct snake {
    NODE *head;
    NODE *tail;
} SNAKE;

int main(int argc, char *argv[]) {
    // Initialize SDL
    SDL_Init(SDL_INIT_VIDEO);

    // Create window and renderer
    SDL_Window *window = SDL_CreateWindow("Snake Game", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, SCREEN_WIDTH, SCREEN_HEIGHT, SDL_WINDOW_SHOWN);
    SDL_Renderer *renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);

    // Load snake and map textures
    SDL_Surface *snake_surface = SDL_LoadBMP("snake.bmp");
    SDL_Texture *snake_texture = SDL_CreateTextureFromSurface(renderer, snake_surface);
    SDL_FreeSurface(snake_surface);

    SDL_Surface *map_surface = SDL_LoadBMP("map.bmp");
    SDL_Texture *map_texture = SDL_CreateTextureFromSurface(renderer, map_surface);
    SDL_FreeSurface(map_surface);

    // Game variables
    int snake_length = 5;
    SNAKE snake;
    snake.head = malloc(sizeof(NODE));
    snake.head->x = 10;
    snake.head->y = 10;
    snake.head->next = NULL;
    snake.tail = snake.head;
    int i;

    // Game loop
    SDL_Event event;
    int quit = 0;
    while (!quit) {
        // Handle events
        while (SDL_PollEvent(&event)) {
            switch (event.type) {
                case SDL_QUIT:
                    quit = 1;
                    break;
                case SDL_KEYDOWN:
                    switch (event.key.keysym.sym) {
                        case SDLK_UP:
                            // Move snake up
                            break;
                        case SDLK_DOWN:
                            // Move snake down
                            break;
                        case SDLK_LEFT:
                            // Move snake left
                            break;
                        case SDLK_RIGHT:
                            // Move snake right
                            break;
                    }
                    break;
            }
        }

        // Clear screen
        SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
        SDL_RenderClear(renderer);

        // Draw map
        SDL_RenderCopy(renderer, map_texture, NULL, NULL);

        // Draw snake
        NODE *current = snake.head;
        for (i = 0; i < snake_length; i++) {
            SDL_Rect rect = {current->x, current->y, SNAKE_SIZE, SNAKE_SIZE};
            SDL_RenderCopy(renderer, snake_texture, NULL, &rect);
            current = current->next;
        }

        // Update screen
        SDL_RenderPresent(renderer);
    }

    // Free resources
    SDL_DestroyTexture(snake_texture);
    SDL_DestroyTexture(map_texture);
    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();

    return 0;
}


```


# 源代码(双进程实现)

```python
import turtle  # 导入海龟图形库
import copy  # 导入复制库
from random import randrange  # 导入一个随机库
from random import randint
import threading #导入线程库

##########################  LOG  ############################
# 看到一个双人坦克的小游戏
# 意识到lambda的好用,以及那个turtle.ontimer的方法参数要求
# 抛弃繁琐的c语言链表，我用了python万能的库资源
# 双进程库threading,两个循环肯定需要走完一个才能进行下一个,如何同时进行两个循环
# 随机整数库和随即范围库
# draw函数自定义颜色
# 判定静态食物,我就直接用坐标重合判断吃掉,动态食物的话发现有个库方法叫distance,很好用(借鉴大佬)
# 加入积分榜
# 本来想用多个turtle对象,后面还是用列表直接存储坐标信息然后画一个方块比较方便 
# 操作变换位置时的time,ontimer可以设置动态食物的更换位置时间
# 三进程会出现崩溃,动态食物做不了呜呜呜
# 判定碰撞时候，尾部的是特殊的，蛇会吃掉另一条蛇的尾部
# 判定碰撞，排除尾部，头部的话姑且算是同归于尽
# 判定碰撞，排除自己撞死自己，自己撞死自己属于是单人游戏增加难度用的，双人游戏的话就不用了吧嘿嘿，可以绕圈圈
# 关于边界的问题还是搞不太明白
#############################################################
# 变色丸子设定:可以变成黑色,此时无敌效果
# 奖励丸子设定:可以直接全屏绿石子
square_size = 20  # 正方体边长


HEIGHT=650
WIDTH=1360
# snake_P1=turtle.Turtle()#玩家1
# snake_P2=turtle.Turtle()#玩家2
# moving_food=turtle.Turtle() #定义一个随即移动食物

flag_winner=-1 #标记赢家
count_food=0 #标记食物
snake_body1 = [[-20, 0], [0, 0]]  # 存储P1坐标
snake_body2 = [[0,20], [0, 40]]  # 存储P2坐标
food = [20,20] # 食物坐标默认在中间
moving_food =[square_size, square_size] #存储动态食物的坐标
finalfood =[] # 最终食物默认无
aim1 = [square_size,0 ]  # 默认往右移动一个
aim2 = [-square_size,0]  # 默认往左移动一个 #后期通过direct修改朝向
aim3=[0,0] #无默认,通过函数实现一个随机方向

color1="blue" #默认蓝色
color2="red" #默认红色 #经典红蓝对决

move_speed_snake = 90  # 蛇移动速度
move_speed_food = 500 # 动态食物移动速度

def draw_square(x, y, size, color):  # x,y是起点,size是边长 #逆时针

    turtle.penup()
    turtle.goto(x, y)
    turtle.pendown()
    turtle.color(color)
    turtle.begin_fill()

    for i in range(4):
        turtle.forward(size)  # 前进
        turtle.left(90)  # 逆时针旋转
    turtle.end_fill()

def body_len(sanke_body):
    return len(sanke_body) # 返回身体长度
    
def is_border(snake_body):  # 检查与其它(海龟/食物)的距离, 如果吃到了, 返回布尔值True
    return abs(snake_body[-1][0])>WIDTH/2 or abs(snake_body[-1][1])>HEIGHT/2 #头部的x和y



def direct(x, y,aim):  # 改变前进方向 # (0,-)就是往下移动 (-,0)就是往左移动 (0,+)就是往上移动 (+,0)就是往下移动
    aim[0] = x
    aim[1] = y

def is_crash(snake_body_P1,snake_body_P2):
    except_tail=copy.deepcopy(snake_body_P2)
    except_tail.pop(0) # 排除尾部 #这样那个移动函数里吃尾部的判断才能生效
    if snake_body_P1[-1] in except_tail: #P1的头部在P2的身体上
        return True
    else:
        return False
    
def draw_body(snake_body,color):
    for body in snake_body:
        draw_square(body[0], body[1], square_size, color)

def summon_food():
    food[0] = randrange(-35, 35)*square_size
    food[1] = randrange(-15, 15)*square_size  # 必须是square_size的倍数才有可能吃到食物
# def rand_direct(aim):
#     aim[0] = randrange(-1, 1)*square_size
#     aim[1] = randrange(-1, 1)*square_size  
#     turtle.ontimer(lambda: rand_direct(aim),move_speed_food)
#     turtle.ontimer(lambda: rand_direct(aim),move_speed_food)
# def rand_move(moving_food,aim):
#     # randmove_turtle是要随机移动的turtle对象, moveRange是随机移动的范围 整型, 最小2表示范围在全屏, 值越大范围越小
#     # moving_food=[(randint(-WIDTH//moveRange, WIDTH//moveRange),randint(-HEIGHT//moveRange, HEIGHT//moveRange))]
#     # global moving_food
#     # direct(randrange(-3,3)*square_size,randrange(-3,3)*square_size,aim3) #我好聪明
#     # moving_food=[moving_food[0]+aim3[0],moving_food[1]+aim3[1]]
#     rand_direct(aim)
#     moving_food=[moving_food[0]+aim[0],moving_food[1]+aim[1]]
#     draw_square(moving_food[0],moving_food[1],square_size,"black")
#     turtle.ontimer(lambda: rand_move(moving_food, aim),move_speed_snake)  
  

def move(snake_body_P1,aim,color,snake_body_P2):  # 消除尾部方块,在头部添加方块,再刷新一次动画,完成移动
    # 深度拷贝最后一个方块为蛇头部,也就是将一个包含xy坐标的列表拷贝给head
    global finalfood
    head = copy.deepcopy(snake_body_P1[-1])
    head = [head[0]+aim[0], head[1]+aim[1]]  # 此时修改头部的位置 注意这只能一格一格来,所以是一次增长5
    if is_border(snake_body_P1) and len(snake_body_P1)!=1:
        snake_body_P1.pop(-1) #蛇身碱到一个
        # snake_body=[[0,0]] #蛇身变成一个
    if head == finalfood:
        turtle.write("YOU WIN",align="center",font=("宋体",30,"normal"))
        return
    if head == food:  # 当方块重合时(此处是列表和列表直接比较)直接生成新的食物
        summon_food()  
    else:
        snake_body_P1.pop(0)  # 将第一个方块剪切掉,其实就是尾部
    if is_crash(snake_body_P1,snake_body_P2):
        finalfood=[0,0]
        draw_square(finalfood[0],finalfood[1],square_size,"black")
        return 
    if len(snake_body_P1)==0: # 蛇无长度（被吃掉了）
        return 
    if head == copy.deepcopy(snake_body_P2[0]): # 当P1吃到P2的尾部
        snake_body_P1.append(snake_body_P2.pop(0))
  
    snake_body_P1.append(head)  # 添加修改之后的头部
    turtle.clear()  # 刷新一下画布
    turtle.update()  # 更新
    draw_square(food[0], food[1], square_size, "green")  # 画出食物,边长也得是和蛇的body边长一样
    draw_body(snake_body_P1,color)  # 重新绘出蛇的身体
    # turtle.write("snake",align="center",font=("宋体",10,"normal"))
    turtle.ontimer(lambda: move(snake_body_P1, aim,color,snake_body_P2),move_speed_snake)  # 不断重复执行move这个函数 #我真服了这个ontimer
    # turtle.ontimer(move(snake_body, aim),move_speed) 是不能这么写的,我真服了


####################  作图限制&绑定按键  ####################
# turtle.bgpic("r'./giphy.gif")
# turtle.setworldcoordinates(-1450, -725, 1450, 725)
turtle.title("Gluttonous snake (blue vs red)")
turtle.speed(0)  # 设置绘画速度
turtle.setup(1450, 725, 0, 0)  # 设定边界,后面两个参数是在屏幕的位置,前面是宽度和长度
turtle.tracer(False)  # 去除这个绘画的过程效果
turtle.hideturtle()  # 去除箭头指示
turtle.listen()  # 确保海龟窗口处于桌面的焦点中，这样就可以监听按键事件
turtle.onkey(lambda: direct(0, square_size,aim1), "Up")
turtle.onkey(lambda: direct(0, -square_size,aim1), "Down")
turtle.onkey(lambda: direct(-square_size, 0,aim1), "Left")
turtle.onkey(lambda: direct(square_size, 0,aim1), "Right")
turtle.onkey(lambda: direct(0, square_size,aim2), "w")
turtle.onkey(lambda: direct(0, -square_size,aim2), "s")
turtle.onkey(lambda: direct(-square_size, 0,aim2), "a")
turtle.onkey(lambda: direct(square_size, 0,aim2), "d")

##################### 设置进程 #########################
mythread_P1=threading.Thread(target=move, args=(snake_body1,aim1,color1,snake_body2), daemon=True)
mythread_P2=threading.Thread(target=move, args=(snake_body2,aim2,color2,snake_body1), daemon=True)
mythread_P1.start() #双进程
mythread_P2.start()


turtle.write("gameover",align="center",font=("宋体",20,"normal"))
# while True
#     turtle.write("gameover",align="center",font=("宋体",10,"normal"))
# threading.Thread(target=rand_move, args=(moving_food,aim3),daemon=True).start()
turtle.done()

```