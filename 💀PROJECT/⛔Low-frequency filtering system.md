- 低频单词过滤
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <time.h>
#define MAX_SIZE 1000 // 总单词数五百多个

typedef struct LinkNode
{                  // 定义链表节点类型
    char data[30]; // 每个节点存放一个字符串
    int count;
    struct LinkNode *next; // 指针指向下一个节点
} LinkNode, *LinkList;

typedef struct BSTNode
{
    char data[30];
    int count;
    struct BSTNode *left;
    struct BSTNode *right;
} BSTNode, *BST;

typedef struct
{
    BST arr[MAX_SIZE];
    int top;
} Stack;

//***********************二叉树非递归遍历所需要的栈结构函数*************
void initStack(Stack *stack)
{
    stack->top = -1;
}

int isEmpty(Stack *stack)
{
    return stack->top == -1;
}

int isFull(Stack *stack)
{
    return stack->top == MAX_SIZE - 1;
}

void push(Stack *stack, BST node)
{
    if (isFull(stack))
    {
        printf("Stack is full. Cannot push element.\n");
        return;
    }
    stack->top++;
    stack->arr[stack->top] = node;
}

BST pop(Stack *stack)
{
    if (isEmpty(stack))
    {
        printf("Stack is empty. Cannot pop element.\n");
        return NULL;
    }
    BST node = stack->arr[stack->top];
    stack->top--;
    return node;
}

BST top(Stack *stack)
{
    if (isEmpty(stack))
    {
        printf("Stack is empty.\n");
        return NULL;
    }
    return stack->arr[stack->top];
}

//*************************************************************
enum Option
{
    EXIT,   // 0
    ALL,    // 1
    TIME,   // 2
    COUNT,  // 3
    DELETE, // 4
    OUTPUT, // 5
    ASL,    // 6
};

//*************************LIST基本操作************************
void print_list(LinkList head)
{
    LinkNode *node;
    for (node = head; node != NULL; node = node->next)
    {
        printf("%s--------------------------------(%d)\n", node->data, node->count);
    }
}
void destroy_list(LinkList head)
{
    while (head != NULL)
    {
        LinkNode *node = head;
        head = head->next;
        free(node);
    }
}
int LIST_count_node(LinkList head)
{
    int sum = 0;
    LinkNode *p = head;
    while (p)
    {
        sum++;
        p = p->next;
    }
    return sum;
}
//************************LIST核心功能实现****************************

// LinkList merge_lists(LinkList list1, LinkList list2)//后面发现根本没用上
// {
//     if (list1 == NULL)
//         return list2;
//     if (list2 == NULL)
//         return list1;

//     LinkNode *head;
//     LinkNode *current;

//     if (list1->count <= list2->count)
//     {
//         head = list1;
//         list1 = list1->next;
//     }
//     else
//     {
//         head = list2;
//         list2 = list2->next;
//     }

//     current = head;

//     while (list1 && list2)
//     {
//         if (list1->count <= list2->count)
//         {
//             current->next = list1;
//             list1 = list1->next;
//         }
//         else
//         {
//             current->next = list2;
//             list2 = list2->next;
//         }
//         current = current->next;
//     }

//     if (list1)
//         current->next = list1;
//     else if (list2)
//         current->next = list2;

//     return head;
// }

void LIST_bubblesort(LinkList head)
{
    int swapped;
    LinkNode *p;
    LinkNode *tail = NULL;

    if (head == NULL)
        return;

    do
    {
        swapped = 0;
        p = head;

        while (p->next != tail)
        {
            if (p->count > p->next->count)
            {
                // 交换节点的数据
                char temp[30];
                int countTemp;

                strcpy(temp, p->data);
                strcpy(p->data, p->next->data);
                strcpy(p->next->data, temp);

                countTemp = p->count;
                p->count = p->next->count;
                p->next->count = countTemp;

                swapped = 1;
            }

            p = p->next;
        }

        tail = p;

    } while (swapped);
}

void LIST_add_node(LinkList *head, const char *data)
// 必须使用指针的指针才能完成节点的加入
{
    if (*head == NULL)
    {
        // 创建新节点
        LinkNode *newNode = (LinkNode *)malloc(sizeof(LinkNode));
        strcpy(newNode->data, data);
        newNode->count = 1;
        newNode->next = NULL;

        // 将新节点设为头节点
        *head = newNode;
        return;
    }
    // 遍历链表查找相同的节点
    LinkNode *current = *head;
    while (current != NULL)
    {
        if (strcmp(current->data, data) == 0)
        {
            // 找到相同的节点，增加 count 值
            current->count++;
            return;
        }
        current = current->next;
    }

    // 没有找到相同的节点，创建新节点并插入到头部
    LinkNode *newNode = (LinkNode *)malloc(sizeof(LinkNode));
    strcpy(newNode->data, data);
    newNode->count = 1;
    newNode->next = *head;
    *head = newNode;
}

void LIST_output_node(LinkList head)
{
    LinkNode *p = head;
    while (p)
    {
        if (p->count >= 5)
        {
            printf("%s(%d)\n", p->data, p->count);
        }
        p = p->next;
    }
}
void LIST_delete_node(LinkList *head)
{
    LinkNode *dummynode = (LinkNode *)malloc(sizeof(LinkNode)); // 创建哑节点
    dummynode->next = *head;
    LinkNode *p = dummynode;
    LinkNode *q = *head;
    while (q)
    {
        if (q->count < 5)
        {
            printf("%s(%d)\n", q->data, q->count);
            p->next = q->next;
            free(q); // 释放要删除的节点内存
        }
        else
        {
            p = p->next;
        }
        q = p->next;
    }
    *head = dummynode->next;
    free(dummynode); // 释放哑节点内存
}

void LIST_test_example(LinkList *head) // 测试用的
{
    LIST_add_node(head, "apple");
    LIST_add_node(head, "apple");
    LIST_add_node(head, "banana");
    LIST_add_node(head, "banana");
    LIST_add_node(head, "banana");
    LIST_add_node(head, "banana");
    LIST_add_node(head, "apple");
    LIST_add_node(head, "grape");
}

//*************************LIST文件操作****************************

// 单链表文件输入操作
void File_input_bylist(LinkList *head)
{
    FILE *in = fopen("inFile.txt", "r+"); // 打开输入文件
    char arr[20], ch;
    while (!feof(in))
    { // 直到碰见文件结束符结束循环
        int i = 0;
        memset(arr, 0, sizeof(arr)); // 对数组a进行初始化
        while ((ch = fgetc(in)) != EOF && !(ch == ',' || ch == '.' || ch == '!' || ch == '?' || ch == '(' || ch == ')' || ch == ';' || ch == '"' || ch == ' '))
        {
            arr[i++] = ch;
        }
        if (arr[0])
            LIST_add_node(head, arr);
    }
    fclose(in);
}
// 单链表文件输出操作
void File_output_bylist(LinkList head)
{
    LinkList node = head->next;
    FILE *out = fopen("outFile.txt", "w+"); // 建立输出文件
    fprintf(out, "该文章中出现频率不小于5的单词:\n");
    while (node)
    {
        fprintf(out, "%s(%d)\t", node->data, node->count); // 将删除后的链表存入进去
        node = node->next;
    }
    fclose(out);
    printf("写入文件outFile.txt成功\n");
}

//******************************LIST菜单功能************************

// 单链表功能3：文件统计个数
void count_list(LinkList head)
{
    printf("Original List:\n\n"); // 初始统计
    LIST_bubblesort(head);
    LIST_count_node(head);
    print_list(head);
}
// 单链表功能4:删除低频词并显示
void delete_list(LinkList *head)
{

    printf("\nThe list will delete the low-frequency words>>>>>>>>>>>\n\n");
    LIST_bubblesort(*head);
    LIST_delete_node(head);    // 删除并显示
    File_output_bylist(*head); // 将删除之后的链表写入文件
}
// 单链表功能5：输出剩余单词以及其频率
void output_list(LinkList *head)
{
    printf("\n\nThe list will show the high-frequency words>>>>>>>>>>\n\n");
    LIST_bubblesort(*head);
    LIST_output_node(*head); // 跳跃并显示(跳过了低频单词)
}
// 单链表功能6：计算单链表ASL值 // ∑Pi*Ci (i....n)
void ASL_list(int sum)
{
    printf("\nTotal number of words:\t%d\tASL = %.2lf\n", sum, (double)((3 * sum + 1) / 4.0));
    // ASL = 查找成功的ASL + 查找失败的ASL
    /*遍历链表，统计链表的长度（即节点数量）和每个节点的位置索引。可以使用一个计数器变量和一个指针来实现这一步骤。对于每个节点，计算从头节点到该节点的距离。可以使用位置索引或者逐步移动指针来计算距离。将所有节点的距离加起来，得到总的搜索步骤数。将总的搜索步骤数除以节点数量，得到平均搜索步骤数，即ASL。*/
}

// 单链表功能1：连续执行操作
void all_list(LinkList *head)
{
    int sum = LIST_count_node(*head); // 因为担心删除操作之后这个起初的单词总数就改变了,影响后续ASL的计算
    count_list(*head);
    delete_list(head);
    output_list(head);
    ASL_list(sum);
}
// 单链表功能2：显示执行时间
void time_list(LinkList *head)
{
    double start, finish;
    start = (double)clock();                         // 获取当前时间
    all_list(head);                                  // 执行一下全部流程
    system("cls");                                   // 清屏
    finish = (double)clock();                        // 获取结束时间
    printf("执行时间：%.2f ms\n", (finish - start)); // 时间单位：毫秒
}
//****************************TREE基本操作************************
// 二叉树打印(中序)
void print_tree(BST root)
{
    if (root)
    {
        print_tree(root->left);
        printf("%-10s(%d)\n", root->data, root->count);
        print_tree(root->right);
    }
}
// 二叉树的回收
void destroy_tree(BST root)
{
    if (root)
    {
        destroy_tree(root->left);
        destroy_tree(root->right);
        free(root);
    }
}
// 二叉树节点计算
int TREE_count_node(BST root)
{
    if (!root)
    {
        return 0;
    }
    return (1 + TREE_count_node(root->left) + TREE_count_node(root->right)); // 先递归左子树，再递归右子树
}

//****************************TREE核心操作************************

// 二叉排序树的插入结点操作
void TREE_insert_node(BST *root, const char *a)
{
    if (!(*root))
    {
        *root = (BST)malloc(sizeof(BSTNode));
        strcpy((*root)->data, a);
        (*root)->left = NULL;
        (*root)->right = NULL;
        (*root)->count = 1;
    }
    else
    {
        if (strcmp(a, (*root)->data) < 0)
        { // 左子树
            TREE_insert_node(&(*root)->left, a);
        }
        else if (strcmp(a, (*root)->data) == 0)
        { // 相等count+1
            (*root)->count++;
        }
        else
        { // 右子树
            TREE_insert_node(&(*root)->right, a);
        }
    }
}

// 构造低频树
void create_lowtree(BST root, BST *newtree)
{
    if (root == NULL)
    {
        return;
    }

    if (root->count < 5)
    {
        int count = root->count; // 我是天才
        while (count--)
        {
            TREE_insert_node(newtree, root->data);
        }
        // 如果没有这个while的话,对于新树而言,旧树的data只出现了一次,旧树的count没有办法通过insertnode函数来传递给新树,但是我们可以通过旧树得知插入次数,这样insertnode可以不断插入来把旧树的count传递给新树
    }

    create_lowtree(root->left, newtree);  // 递归搜索左子树
    create_lowtree(root->right, newtree); // 递归搜索右子树
}

// 构造高频树
void create_hightree(BST root, BST *newtree)
{

    if (root == NULL)
    {
        return;
    }

    if (root->count >= 5) //
    {
        int count = root->count;
        while (count--)
        {
            TREE_insert_node(newtree, root->data);
        }
    }

    create_hightree(root->left, newtree);  // 递归构造左子树,先读newtree,再取newtree其left的地址
    create_hightree(root->right, newtree); // 递归构造右子树
}

int TREE_calc_search_leagth(BST root, int depth)
{
    if (root == NULL)
    {
        return 0;
    }

    int leftASL = TREE_calc_search_leagth(root->left, depth + 1);
    int rightASL = TREE_calc_search_leagth(root->right, depth + 1);
    int totalSL = depth + leftASL + rightASL;

    return totalSL;
}

void TREE_test_example(BST *root) // 测试功能
{
    TREE_insert_node(root, "apple");
    TREE_insert_node(root, "apple");
    TREE_insert_node(root, "grape ");
    TREE_insert_node(root, "banana");
    TREE_insert_node(root, "grape ");
    TREE_insert_node(root, "banana");
    TREE_insert_node(root, "banana");
    TREE_insert_node(root, "apple");
    TREE_insert_node(root, "apple");
    TREE_insert_node(root, "pineapple");
    TREE_insert_node(root, "pineapple");
    TREE_insert_node(root, "pineapple");
    TREE_insert_node(root, "pineapple");
    TREE_insert_node(root, "pineapple");
    TREE_insert_node(root, "1");
    TREE_insert_node(root, "2");
    TREE_insert_node(root, "3");
}

//***********************TREE文件功能************************
// 二叉排序树文件输入操作
void File_input_bytree(BST *root)
{
    FILE *in;
    in = fopen("inFile.txt", "r"); // 打开输入文件
    char arr[20], ch;
    int i;
    while (!feof(in))
    { // 直到碰见文件结束符结束循环
        i = 0;
        memset(arr, 0, sizeof(arr));
        while ((ch = fgetc(in)) != EOF && !(ch == ',' || ch == '.' || ch == '!' || ch == '?' || ch == ' ' || ch == '(' || ch == ')' || ch == ';' || ch == '"'))
        {
            arr[i++] = ch;
        }
        if (arr[0])
            TREE_insert_node(root, arr);
    }
    fclose(in);
}
// 二叉排序树文件输出操作
void File_output_bytree(BST newtree)
{
    FILE *out;
    Stack stack;
    stack.top = -1;
    out = fopen("outFile.txt", "a+"); // 建立输出文件
    fprintf(out, "该文章中出现频率不小于5的单词:\n");

    while (newtree != NULL || stack.top != -1)
    {
        while (newtree != NULL)
        {
            stack.top++;
            stack.arr[stack.top] = newtree;
            newtree = newtree->left;
        }

        if (stack.top > -1)
        {
            newtree = stack.arr[stack.top];
            stack.top--;
            fprintf(out, "%s (%d)\t", newtree->data, newtree->count);
            newtree = newtree->right;
        }
    }
    fclose(out);
}

//************************TREE菜单功能************************

void count_tree(BST root)
{
    printf("Original List:\n\n"); // 初始统计
    print_tree(root);
}

void delete_tree(BST root)
{
    printf("\nThe list will delete the low-frequency words>>>>>>>>>>>\n\n");
    BSTNode *newtree = NULL;
    create_lowtree(root, &newtree);
    print_tree(newtree);
}
void output_tree(BST root)
{
    printf("\n\nThe list will show the high-frequency words>>>>>>>>>>\n\n");
    BSTNode *newtree = NULL;
    create_hightree(root, &newtree);
    File_output_bytree(newtree); // 将出现次数不小于5的单词全部写入文件
    print_tree(newtree);
}
void ASL_tree(BST root)
{
    int numerator = TREE_calc_search_leagth(root, 0);
    int denominator = TREE_count_node(root);
    printf("\nTotal number of words:\t%d\tASL = %.3lf\n", denominator, (double)((numerator) / denominator));
}
void all_tree(BST root)
{
    count_tree(root);
    delete_tree(root);
    output_tree(root);
    ASL_tree(root); // 由于二叉树是通过构造新树来实现删除操作的,也就是废弃掉旧树,而非链表在原表上直接进行删除操作.
}
void time_tree(BST root)
{
    double start, finish;
    start = (double)clock();                         // 获取当前时间
    all_tree(root);                                  // 执行一下全部流程
    system("cls");                                   // 清屏
    finish = (double)clock();                        // 获取结束时间
    printf("执行时间：%.2f ms\n", (finish - start)); // 时间单位：毫秒
}

//**************************菜单*******************************

void sub_menu()
{
    printf("? 1. 一键执行(Continuously execute until completion)\n");                                // all
    printf("? 2. 执行时间(Display execution time)\n");                                               // time
    printf("? 3. 单步_统计初始(Step by step: Identify and count words)\n");                          // count
    printf("? 4. 单步_删除低频(Single step execution: DELETE and DISPLAY low frequency words)\n");   // delete
    printf("? 5. 单步_展示高频(Single step execution: output other words and their frequencies)\n"); // output
    printf("? 6. 单步_平均查找(Single step execution: Calculate and output ASL)\n");                 // ASL
    printf("? 0. 返回菜单(Return to the main menu)\n");                                              // exit
    printf("输入序号以选择(input the number to select:)");
}

void main_menu() // 主菜单
{
    printf("\n************************************************\n");
    printf("********   1.SingleLinkedList           ********\n");
    printf("********   2.BinarySearchTree           ********\n");
    printf("********   0.QUIT                       ********\n");
    printf("************************************************\n\n");
}

int List_menu()
{
    system("pause");
    system("cls");
    printf("★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★\n");
    printf("★         SingleLinkedList             ★ \n");
    printf("★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★\n\n");

    LinkList head = NULL;

    sub_menu(); // 显示功能菜单
    int num;
    scanf("%d", &num);
    while (!(num == 1 || num == 2 || num == 3 || num == 4 || num == 5 || num == 6 || num == 0))
    {
        printf("Input error, please re-enter\n");
        scanf("%d", &num);
    }
    system("cls");

    File_input_bylist(&head);
    // 根据文章构造链表,接下来就可以通过序号选择来进行操作了
    // 每次递归回来都会重新构造链表
    switch (num)
    {
    case ALL:
        all_list(&head);
        destroy_list(head);
        List_menu();
        break;
    case TIME:
        time_list(&head);
        destroy_list(head);
        List_menu();
        break;
    case COUNT:
        count_list(head);
        destroy_list(head);
        List_menu();
        break;
    case DELETE:
        delete_list(&head);
        destroy_list(head);
        List_menu();
        break;
    case OUTPUT:
        output_list(&head);
        destroy_list(head);
        List_menu();
        break;
    case ASL:
        ASL_list(LIST_count_node(head));
        destroy_list(head);
        List_menu();
        break;
    case EXIT:
        return 0;
    }
    return 1;
}

int Tree_menu()
{
    system("pause");
    system("cls");
    printf("★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★\n");
    printf("★         BinarySearchTree         ★ \n");
    printf("★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★★\n\n");

    BST root = NULL;

    sub_menu(); // 显示功能菜单
    int num;
    scanf("%d", &num);
    while (!(num == 1 || num == 2 || num == 3 || num == 4 || num == 5 || num == 6 || num == 0))
    {
        printf("Input error, please re-enter\n");
        scanf("%d", &num);
    }
    system("cls");

    File_input_bytree(&root);
    // 根据文章构造链表,接下来就可以通过序号选择来进行操作了
    // 每次递归回来都会重新构造二叉树
    switch (num)
    {
    case ALL:
        all_tree(root);
        destroy_tree(root);
        Tree_menu(); // 返回菜单是个递归操作
        break;
    case TIME:
        time_tree(root);
        destroy_tree(root);
        Tree_menu();
        break;
    case COUNT:
        count_tree(root);
        destroy_tree(root);
        Tree_menu();
        break;
    case DELETE:
        delete_tree(root);
        destroy_tree(root);
        Tree_menu();
        break;
    case OUTPUT:
        output_tree(root);
        destroy_tree(root);
        Tree_menu();
        break;
    case ASL:
        ASL_tree(root);
        destroy_tree(root);
        Tree_menu();
        break;
    case EXIT:
        return 1;
    }
    return 0;
}

void menu() // 菜单选择操作
{
    int option;
    main_menu();
    scanf("%d", &option);
    while (!(option == 1 || option == 2 || option == 0))
    {
        printf("Input error, please re-enter\n");
        scanf("%d", &option);
    }
    system("cls");

    switch (option)
    {
    case 1:
        List_menu();
        menu();
        break;
    case 2:
        Tree_menu();
        menu();
        break;
    }
}
//**************************主函数实现*********************
int main()
{
    menu();
    return 0;
}

```
