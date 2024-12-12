# Step-by-step implementation of linked lists (requires some C language foundation)

(Since I really didn't understand the specific implementation steps of linked lists in class, I wrote this blog to record the learning process, and interested novices can also follow along.)

## 1.Know the structure of a linked list & create a simple static linked list and output data

Q: What is a linked list?

A: A linked list is composed of a series of nodes, each node contains two fields, one is a data field, which is used to store data, and the other is a pointer field, which saves the address of the next node, and the linked list is non-contiguous in memory. ==To put it simply, a linked list is a struct variable that is connected to a struct variable by a pointer.== 
![](https://img2024.cnblogs.com/blog/3532859/202412/3532859-20241211110720859-918659546.png)
[Note]：A large rectangle in this diagram is represented as an element in the linked list, i.e., a node; A large rectangle consists of two small rectangles, representing the data for a node (highlighted in yellow in the figure) and a pointer pointing to the next node (highlighted in red in the figure).

Q: Why should I use a linked list?
A: The insertion and deletion of a linked list at a specified position does not require a large number of elements to be moved like an array, and only the pointer at the corresponding position needs to be modified. For example, if I were to insert an element b into the fifth position of the array a[10], then from a[5] to a[9], all the elements would have to be moved backward by one bit, which is very cumbersome. Linked lists, on the other hand, only need to modify the pointer variables of the original fourth and fifth digits.

But linked lists also have drawbacks (compared to arrays)
	1. Because the address is random, it can only be searched by traversal, and the search efficiency is lower.
	2. Compared with arrays, linked lists have more pointer domain space overhead.

According to the classification of memory storage methods, linked lists can be divided into static linked lists and dynamic linked lists. Static linked lists are fixed in length, which may cause a certain amount of memory waste; Dynamically linked lists, on the other hand, dynamically allocate memory through pointers, which can reduce the waste of some memory.

According to the form, linked lists can be divided into one-way linked lists, bidirectional linked lists, circular linked lists, one-way circular lists, and bidirectional circular linked lists.

![](C:\Users\11151\Desktop\链表是单向还是双向.png)

![循环或非循环](C:\Users\11151\Desktop\循环或非循环.png)

(The legend is very clear, so I won't explain too much)

In addition, linked lists are divided into two types: with and without a lead. The "head" here refers to the head node, and the following is an explanation of the head node.
(1) In the data structure, a node is set up before the start node of the singly linked list called the head node, the data field of the head node can not store any information, or it can store additional information such as the length of the linked list, and the pointer field of the head node stores the pointer to the first node (that is, the storage location of the first node).
(2) Function: Facilitate the operation of the linked list, reduce the amount of code, for example, to know that inserting in the linked list, deleting an element is to change the position of the pointer field of the node on this node, so how to carry out these operations on the first node, there is, but troublesome, it is not as convenient as introducing a head node. In short, the introduction of header nodes can reduce the amount of code by taking into account some special cases.

Since linked lists are concatenated by pointer variables, getting the first node of a linked list is equivalent to getting the entire linked list. But because the linked list is not continuous, finding a node is a little cumbersome.

Next, let's try to knock out the static linked list step by step. First, in order to implement that a node in a linked list contains two domains, we define a struct. 

```C
struct ListNode //define a struct ListNode
{
    int data;              //define a data member of type int
    struct ListNode* next;//define a pointer member of type struct ListNode*
};
```

[Note]: Explain why the type of next here is ListNode *, because the meaning of next is to point to the next node, and the variable type of the next node is the struct we define ListNode, and next is a pointer, so it should be ListNode *.

Next, define a function to initialize each node (you can also do it directly in the main function)

```C
void test()
{
    struct ListNode node1 = { 1, NULL };//define a node1 of type struct ListNode
    struct ListNode node2 = { 2, NULL };//define a node2 of type struct ListNode
    struct ListNode node3 = { 3, NULL };//define a node3 of type struct ListNode
    struct ListNode node4 = { 4, NULL };//define a node4 of type struct ListNode
    node1.next = &node2;// point node1's next pointer to node2
    node2.next = &node3;// point node2's next pointer to node3
    node3.next = &node4;// point node3's next pointer to node4
    //be careful, the last node's next pointer should be NULL, not &node1

    struct ListNode* pHead = &node1;
}
```

Q: Now that the most important structure of a static linked list has been created, how should I output the data stored in each node of the linked list?

A: This is achieved by traversing the entire linked list.

Next, we traverse the linked list output data. How do you iterate through this linked list? Because nodes are connected by pointers, we can only do this by pointers if we want to access the nodes.
So let's define a helper pointer variable, such as pCurrent, that points to the next node.

```C
pCurrent = pCurrent->next;
```

Since it can point to the next node, the helper pointer variable type should be the pointer of the node, i.e., the ListNode* type.
[Note]：==->== is a whole, which is used to point to structs, classes in C++, and other pointers with subdata. In other words, if we define a struct in C and state a pointer to the struct, then we need to use "->" to retrieve the data in the struct with a pointer.

Q: Traversing nature needs to be implemented with loops, so what are the conditions for loops?
A: We noticed that when creating a linked list, each node is connected by a next pointer, and the next of the last node points to NULL, so when pCurrent is updated to the last, it should be NULL, and the loop ends.

```C
 while (pCurrent != NULL)// while pCurrent is not NULL (i.e. the end of the linked list has not been reached)
    {   printf("%d ", pCurrent->data);// print the data of the current node
        // move pCurrent to the next node in the linked list
        pCurrent = pCurrent->next;// move pCurrent to the next node in the linked list
    }
```

At this point, a few key parts have been completed, and we will sort out some and add some of the code. Here's the full code for that section

```C
#include <stdio.h>
#include <stdlib.h>
struct ListNode //define a struct ListNode
{
    int data;              //define a data member of type int
    struct ListNode* next;//define a pointer member of type struct ListNode*
};

void test()
{
    struct ListNode node1 = { 1, NULL };//define a node1 of type struct ListNode
    struct ListNode node2 = { 2, NULL };//define a node2 of type struct ListNode
    struct ListNode node3 = { 3, NULL };//define a node3 of type struct ListNode
    struct ListNode node4 = { 4, NULL };//define a node4 of type struct ListNode
    node1.next = &node2;// point node1's next pointer to node2
    node2.next = &node3;// point node2's next pointer to node3
    node3.next = &node4;// point node3's next pointer to node4
    //be careful, the last node's next pointer should be NULL, not &node1

    struct ListNode* pHead = &node1;//define a pointer pHead of type struct ListNode* and point it to node1

    //how to traverse a linked list

    // first define a pointer pCurrent to point to the head of the linked list
    struct ListNode* pCurrent = &pHead;// point pCurrent to the head of the linked list

    // then use a while loop to traverse the linked list
    while (pCurrent != NULL)// while pCurrent is not NULL (i.e. the end of the linked list has not been reached)
        printf("%d ", pCurrent->data);// print the data of the current node
        // move pCurrent to the next node in the linked list
        pCurrent = pCurrent->next;// move pCurrent to the next node in the linked list
    }
}
int main()
{
    test();// call the test function
    return 0; // return 0 to indicate successful execution of the program
}
```

