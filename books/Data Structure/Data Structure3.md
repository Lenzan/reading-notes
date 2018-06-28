# 栈与队列
## 栈的定义与实现
### 栈的定义
定义：把运算位置限制在表尾端
栈顶：允许运算端
栈顶指示器：用来指示动态变化的栈顶位置
空栈：表中没有任何元素
满栈：无法申请到栈区可用空间
栈的常见运算：进栈或入栈（表尾插入）、出栈或退栈（表尾删除）
上溢：栈已满还要入栈
下溢：栈已空还要出栈
栈的特性：后进先出  
![stack](https://github.com/Lenzan/reading-notes/blob/master/books/Data%20Structure/Texture/stack1.jpg)
#### 栈的抽象数据类型定义
数据元素：可以是任意的数据，但必须属于同一数据对象。
关系：栈中数据元素之间是线性关系。
基本操作：
- InitStack(S)
- ClearStack(S)
- IsEmpty(S)
- IsFull(S)
- Push(S , x)
- Pop(S , x)
- GetTop(S , x)
### 栈的顺序实现
定义：
- 用一组<color=red>连续</color>的存储单元依次存放<color=red>自栈底到栈顶</color>的数据元素.
- 设一个<color=red>位置指针top</color>（栈顶指针）动态指示栈顶元素在顺序栈中的位置。
- top = -1 表示空栈。  

#### 用C语言描述栈的顺序存储结构

        #define TRUE 1  
        #define FALSE 0
        #define Stack_Size 50//任意长度
        typedef struct
        {
            StackElementType elem[Stack_Size];//类型
            int top;//指示器
        }SeqStack;//顺序栈
#### 顺序栈基本操作的实现
- 初始化

        void InitStack(SeqStack *S)
        {
            //构造一个空栈S*
            S->top = -1;
        }

- 判栈空

        int IsEmpty(SeqStack *S){
            //判栈S为空栈时返回值为真，反之为假
            return(S->top == -1 ? TRUE : FALSE);
        }

- 判栈满

        int IsFull(SeqStack *S){
            //判栈S为满时返回值为真，反之为假
            return(S->top == Stack_Size - 1 ? TRUE : FALSE);
        }

- 进栈

        int Push(SeqStack *S , StackElementType x){
            if(S->top == Stack_Size - 1)//判断栈是否已满
            return(FALSE);
            S->top++;
            S->elem[S->top] = x;
            return(TURE);
        }

- 出栈

        int Pop(SeqStack *S , StackElementType *x){
            if(S->top == - 1){
                //判断栈是否已空
            return(FALSE);
            }
            else{
                *x =S->elem[S->top];
                 S->top--;
                return(TURE);
            }
        }

- 读栈顶元素

         int GetTop(SeqStack *S , StackElementType *x){
            if(S->top == - 1){
                //判断栈是否已空
            return(FALSE);
            }
            else{
                *x =S->elem[S->top];
                return(TURE);
            }
        }

#### 两栈共享的数据结构
##### 两栈共享栈空间示意图
利用了栈栈底位置不变 ， 而栈顶位置动态变化的特性为两个栈申请一个共享的一维数组空间S[M]  
将两个栈的栈底分别为一维数组的两端 0 和 M-1  
共享栈空间示意： top[0] 和 top[1] 为两个栈顶指示器  
![stack](https://github.com/Lenzan/reading-notes/blob/master/books/Data%20Structure/Texture/stack.jpg)
##### 两栈共享的数据结构定义

        #define M 100
        typedef struct{
            StackElementType Stack[M];//栈区
            int top[2];//两个栈顶指示器
        }DqStack;

##### 初始化操作算法

        void InitStack(DqStack *S){
            S->top[0] = -1;
            S->top[1] = M;
        }

##### 进栈操作算法
进栈操作步骤：
- 进栈需判栈满 S->top[0] + 1 == S->top[1]
- 修改栈顶指针
- 元素进栈

        int Push(DqStack *S , StackElementType x , int i){
            if(S->top[0] + 1 == S->top[1])//判断栈是否已满
            return(FALSE);
            switch(i){
                case 0:
                    S->top[0]++;
                    S->Stack[S->top[0]] = x;
                    break;
                case 1:
                    S->top[1]--;
                    S->Stack[S->top[1]] = x;
                    break;
                default:
                return(FALSE);
           }
           return(TRUE)
        }

![stack2](https://github.com/Lenzan/reading-notes/blob/master/books/Data%20Structure/Texture/stack3.jpg)
##### 出栈操作算法
出栈操作步骤：
- 出栈需判空
- 元素出栈
- 修改栈顶指针

        int Pop(DqStack *S , StackElementType *x , int i){
            switch(i){
                case 0:
                    if(S->top[0] == -1)return(FALSE);
                    *x = S->Stack[S->top[0]];
                    S->top[0]--;
                    break;
                case 1:
                    if(S->top[1] == M)
                    return(FALSE);
                    *x = S->Stack[S->top[1]];
                    S->top[1]++;
                    break;
                default:
                return(FALSE);
           }
           return(TRUE)
        }


### 栈的链式实现
#### 链栈的定义及示意图
采用带头结点的单链表实现链栈。  
头指针就作为指针  
链栈示意图:  
![链栈](https://github.com/Lenzan/reading-notes/blob/master/books/Data%20Structure/Texture/stack4.jpg)
- top 为栈顶指针，始终指向当前栈顶元素前面的头结点。
- 若 top -> next == NULL , 则代表空栈
#### 用C语言定义的链栈结构

        typedef struct node{
            StackElementType data;
            struct node *next;
        }LinkStackNode;
        typedef LinkStackNode *LinkStack;

#### 链栈的进栈操作

        int Push(LinkStacktop , StackElementType x){
            LinkStackNode* temp;//将数据元素x压人栈 top 中
            temp = (LinkStackNode*)malloc(sizeof(LinkStackNode));
            if(temp == NULL)return(FALSE);//申请空间失败
            temp->data = x;
            temp->next = top->next;
            top->next = temp;//修改当前栈顶指针
            return(TRUE);
        }

#### 链栈的出栈操作

        int Pop(LinkStacktop , StackElementType *x){
            LinkStackNode* temp;//将栈 top 的栈顶元素弹出，放到x所指的存储空间中
            temp = top->next;
            if(temp == NULL)return(FALSE);//栈为空
            top->next = temp->next;
            *x = temp->data;
            free(temp);//释放存储空间
            return(TRUE);
        }

#### 多栈运算
## 栈的应用与递归
### 括号匹配
算法思想：利用设置一个括号栈判别左右括号个数与类型匹配的情况
- 若读入左括号则入栈，等带相匹配的同类右括号
- 若读入右括号且与栈顶的左括号类型匹配，栈顶左括号出栈，否则属于不合法
### 表达式求值
- 无括号算术表达式求值  

        运算符优先级
        #  +-  */  **
        0   1   2   3

-  算术表达式处理规则
    1. 规定运算符的优先级表
    2. 设置两个栈：OVS（运算数栈）、OPTR（运算符栈）
    3. 自左向右扫描，进行如下处理
    - 遇到 运算数则进 OVS 栈
    - 遇到运算符则与 OPTR 栈的栈顶运算符进行优先级比较：
    如果当前运算符 >OPTR 栈顶运算符，则当前运算符进 OPTR 栈
    如果当前运算符<=OPTR 栈顶运算符，则 OPTR 退栈一次，得到栈顶运算符 y, OVS 连续退栈两次，得到运算数 a,b,对 a,b 执行 y 操作，得到结果 T(i) ,将 T(i) 进 VOS 栈。

### 栈与递
递归：在定义自身的同时又出现了对自身的调用。
直接递归函数:在定义的函数体内直接调用自己。
间接递归函数：一个函数经过一系列中间调用语句，通过其他函数简介调用自己，如 P 调用 Q ，Q 在调用 P。
#### 递归特性的问题
##### 递归定义的数学函数

- 二阶Fibonacci数列

        int Fib(int n){
            if(n == 0)
            return 0;
            if(n == 1)
            return 1 ;
            return Fib(n-1) + Fib(n-2);
        }

- 阿克曼函数

        int Ack(int m , int n){
            if(m == 0)
            return n+1;
            else if(m != 0 && n == 0)
            return Ack(m-1 , 1);
            else
            return Ack(m-1 , Ack(m , n-1));
        }

##### 递归数据结构的处理
如：广义表、二叉树、树等结构其本身均具有固有的递归特性，因此可以自然地采用递归法进行处理。
##### 递归求解方法
例子：汉诺塔问题的算法实现

        //将塔座X上编号为1至 n 的 n 个由小到大圆盘按规则搬到塔座 Z 上，Y 可用作辅助塔座
        void hanoi(int n , char y , char z){
            if(n == 1){
                move(x,1,z)//将编号为1的圆盘从 X 移动到 Z
            }
            else{
                hanoi(n-1 , x , z , y);//将 X 上编号为 1 至 n-1 的圆盘移到 Y,Z 作辅助塔
                move(x , n , z);//将编号为 n 的圆盘从 X 移动到 Z
                hanoi(n-1 , y , x , z);//将 y 上编号为1至 n-1 的圆盘移动到 z , x作辅助塔
            }
            
        }

##### 递归问题的优点
- 对递归问题描述简洁
- 结构清晰
- 程序的正确性容易证明

## 队列的定义与实现
## 队列的应用
## 总结与提高