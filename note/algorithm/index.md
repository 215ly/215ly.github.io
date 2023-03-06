# 线性结构

## 一、线性表
1. 具有相同数据类型的有限**序列**（这里理解为顺序）
2. 有限集合，不能无限
3. 除头结点外，每个节点只能有一个直接父节点
4. 除尾节点外，每个节点只能有一个直接子节点
### 顺序存储的线性表（顺序表）
> **如何分配连续地址的存储单元？**
1. **静态实现**：直接采用数组，申请后不能扩展存储空间
    1. 创建顺序表结构
        ```cpp
        #include <iostream>
        #include <stdio.h>
        #include <string.h>
        using namespace std;
        #define MAXSIZE 100 //数组最大值
        typedef int ElemType; //元素
        typedef struct { //创建顺序表结构体
            ElemType data[MAXSIZE];
            unsigned int length;
        }SeqList,*PseqList; 
        ```
    2. 初始化顺序表
        ```cpp
        void init(PseqList LL){
            if(LL== nullptr) return;
            LL->length = 0;//长度为0
            memset(LL->data, 0, sizeof(ElemType) * MAXSIZE);//元素为0
        }
        ```
    3. 在指定位置插入元素（指定下标后，将下标所有元素往后移动）
        ```cpp
        int add(PseqList LL,unsigned int ii,ElemType *ee){
            if (LL== nullptr||ee== nullptr) {
                cout << "请检查顺序表或元素是否存在" << endl;
                return -1;
            }
            if (LL->length>=MAXSIZE) {
                cout << "顺序表已满，插入失败！" << endl;
                return -1;
            }
            if (ii > LL->length + 1 || ii < 0) {
                cout << "插入位置不合法！" << endl;
                return -1;
            }
            int kk;
            //后序遍历到插入的ii位置为止
            for (kk=LL->length;kk>=ii;kk--)
            {
                memcpy(&LL->data[kk],&LL->data[kk-1],sizeof(ElemType));  //后移一位
            }
            memcpy(&LL->data[ii-1],ee,sizeof(ElemType));  //将待插入的值复制到ii的位置下标
            LL->length++;       // 表的长度加1。
            return 1;
        }
        ```
        4. 表头插入,表尾插入
        ```cpp
        ElemType ee;
        ee=55;
        //直接调用add(),插入位置为表头索引0
        add(&LL,0,&ee);//在顺序表LL中，将55插入到索引0的位置

        //直接调用add(),插入位置为表长度+1
        add(&LL,LL->length+1,&ee);//在顺序表LL中，将55插入到表尾的位置
        ```
        4. 删除元素（指定下标后，将下标所有元素往前移动）
        ```cpp
        int del(PseqList LL,unsigned int ii){
            //...省略判空条件
            // 把ii之后的元素前移。
            int kk;
            for (kk=ii;kk<=LL->length;kk++)
            {
                memcpy(&LL->data[kk-1],&LL->data[kk],sizeof(ElemType)); //元素往前移
            }
            LL->length--;       // 表的长度减1。
            return 1;
        }
        ```


2. **动态实现**：动态分配内存，可任意扩展存储空间

> 顺序表随机访问很快，可通过下标直接获取元素`O(1)`，插入和删除较慢，因为需要移动元素，内存拷贝次数比较多

