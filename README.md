[**LinkedList**](#LinkedList)

[**DoubleLinkedList**](#DoubleLinkedList)

[**Set**](#Set)

[**Dictionary**](#Dictionary)

# LinkedList
### LinkedList.c
```c
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>

#define countStart 0 //define

struct ListItem 
{
    struct ListItem *next; //pointer of the next node
    int value; //value of the pointer
};

unsigned int ListLength(const struct ListItem *head) // scrolls through the entire list and return the lenght of the list
{
    int i = countStart;
    while (head)
    {
        i++;
        head = head->next;
    }
    return i;
}

struct ListItem *ListGetTail(  struct ListItem **head) //return the last node of the list
{
    if (!head || !(*head))
    {
        printf("the list is empty or not initializzated.\n");
        return NULL;
    }
    struct ListItem *CurrentItem = *head; //copy of the list
    while (CurrentItem->next) //scrolls the list until it find the last element with no next node 
    {
        CurrentItem = CurrentItem->next;
    }
    return CurrentItem;
}

int ListAppend(struct ListItem **head, const int item) //allow to append a element at the end of the list
{
    if (!head)
    {
        printf("the list is not initializzated.\n");
        return -1;
    }
    struct ListItem *tail = ListGetTail(head); //last element of the list
    if (!tail)//if there is no tail it means that this element is the first of the list
    {
        (*head) = malloc(sizeof(struct ListItem)); //alloc space for the node
        if (!(*head))
        {
            printf("allocation error.\n");
            return -1;
        }
        (*head)->value = item; 
        (*head)->next = NULL;
    }
    else
    { 
        struct ListItem *NewItem = malloc(sizeof(struct ListItem));//alloc space for the node
        if (!NewItem)
        {
            printf("allocation error.\n");
            return -1;
        }
        NewItem->value = item;
        NewItem->next = NULL;
        tail->next = NewItem;
    }
    printf("\nValue %d appended\n", item);
    return 0;
}

int ListRemoveIndex(struct ListItem **head,const int index) //remove a value from the list given a index of the list
{
    if (!head || index < 1)
    {
        printf("cant remove index below zero.\n");
        return -1;
    }

    struct ListItem *current = *head; //pointer for the current node
    struct ListItem *previous = NULL; //pointer for the preveiouse node of current node
    int i = countStart;

    while (current && i < index) //search the node with the specified index
    {
        i++;
        previous = current;
        current = current->next;
    }

    if (!current)
    {
        printf("index too high.\n");
        return 1;
    }

    if (previous) 
    {
        previous->next = current->next;
    }
    else //if the null it means that the current node is the first node
    {
        *head = current->next;
    }
    printf("\nNode %d removed", index);
    free(current);
    return 0;
}

int ListRemoveItem(struct ListItem **head, const int item) //remove a value from the list given a value of the list
{
    if (!head)
    {
        printf("list not initializated correctly.\n");
        return -1;
    }

    struct ListItem *current = *head;
    struct ListItem *previous = NULL;

    while (current && item != current->value)
    {
        previous = current;
        current = current->next;
    }

    if (!current)
    {
        printf("element not find.\n");
        return 0;
    }

    if (previous)
    {
        previous->next = current->next;
    }
    else
    {
        *head = current->next;
    }

    printf("\nNode with value %d removed", item);
    free(current);
    return 0;
}

int ListReverse(struct ListItem **head)//allow to reverse the list
{
    if (!head || !(*head))
    {
        printf("list empty or not initializated.\n");
        return -1;
    }

    if (ListLength(*head) <= 1)//if the list as only one element you dont need to reverse the list )
    {
        return 0;
    }

    struct ListItem *ReversedList = NULL; // empty list for the reverse
    struct ListItem *current = *head; // copy of the list

    while(current)  
    {
        struct ListItem *next = current->next;
        current->next = ReversedList; 
        ReversedList = current;
        current = next;
    }
    *head = ReversedList;
    return 0;
}

void PrintList(const struct ListItem *head)//allow to print the list
{
    printf("\n");
    int i=countStart;
    while (head)
    {   
        i++;
        printf("-index:%d ",i);
        printf("|%p ", head);
        printf("value: %d| ", head->value);
        head = head->next;
    }
    printf("\n");
}

int main()
{
    struct ListItem *head = NULL;

    ListAppend(&head, 100);
    ListAppend(&head, 200);
    ListAppend(&head, 300);
    ListAppend(&head, 400);

    printf("List:");
    PrintList(head);

    ListRemoveIndex(&head, 2);
    printf("\n\nlist after removal of index 2: ");
    PrintList(head);

    ListRemoveItem(&head, 100);
    printf("\n\nlist after removal of value 100: ");
    PrintList(head);

    ListReverse(&head);
    printf("\nlist reversed: ");
    PrintList(head);

    return 0;
}

//clang.exe -o .\c\linkedList.exe  .\c\linkedList.c
```
##

# DoubleLinkedList
### DoubleLinkedList.c
```c
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>
struct ListItem
{
    struct ListItem *next; //pointer of the next node
    struct ListItem *prev; //pointer of the previous node
    int value;  //value of the pointer
};

struct ListItem *ListGetTail( struct ListItem **head)//return the last node of the list
{
    if (!head || !(*head))
    {
        printf("the list is empty or not initializzated.\n");
        return NULL;
    }
    struct ListItem *CurrentItem = *head; //copy of the list
    while (CurrentItem->next) //scrolls the list until it find the last element with no next node 
    {
        CurrentItem = CurrentItem->next;
    }
    return CurrentItem;
}

int ListAppend(struct  ListItem **head,const int item) //allow to append a element at the end of the list
{
    if (!head)
    {
        printf("the list is empty or not initializzated.\n");
        return -1;
    }
    struct ListItem *tail = ListGetTail(head);//last element of the list
    if (!tail)//if there is no tail it means that this element is the first of the list
    {
        (*head) = malloc(sizeof(struct ListItem));//alloc space for the node
        (*head)->value = item;
        (*head)->next = NULL;
        (*head)->prev=NULL;
    }
    else
    {
        struct ListItem *NewItem = malloc(sizeof(struct ListItem));//alloc space for the node
        NewItem->value = item;
        NewItem->next = NULL;
        NewItem->prev=tail;
        tail->next = NewItem;
    }
    printf("Value %d appended\n", item);
    return 0;
};

int ListAppendAfter(struct ListItem **head,const int item, const int index) //allow to append a element after another node given a index
{
    if (!head)
    {
        printf("the list is empty or not initializzated.\n");
        return -1;
    }

    int i = 1;
    struct ListItem *current = *head;

    while (current && i < index)//search the note by the index
    {
        i++;
        current = current->next;
    }

    if (!current)//control if the node is valid
    {
        printf("invalid index\n");
        return -1;
    }

    
    struct ListItem *NewItem = malloc(sizeof(struct ListItem)); //alloc space for the node
    NewItem->value = item;
    NewItem->prev = current;
    NewItem->next = current->next;

    
    if (current->next)
    {
        current->next->prev = NewItem;
    }

    current->next = NewItem;
    printf("\nValue %d appended after index %d\n", item,index);
    return 0;
}

int ListAppendBefore(struct  ListItem **head,const int item,const int index) //allow to append a element before another node given a index
{
    if (!head)
    {
        printf("the list is empty or not initializzated\n");
        return -1;
    }

    int i = 1;
    struct ListItem *current = *head;

    //serach  node by index
    while (current && i < index)
    {
        i++;
        current = current->next;
    }
    
    //control if the node is valid
    if (!current)
    {
        printf("invalid index\n");
        return -1;
    }

    // Create new node
    struct ListItem *NewItem = malloc(sizeof(struct ListItem));
    NewItem->value = item;
    NewItem->next = current;
    
    if (current->prev)
    {  
        NewItem->prev = current->prev;
        current->prev->next = NewItem;
        current->prev = NewItem;
    }
    else
    {
        NewItem->prev=NULL;
        *head = NewItem;
    }
    printf("\nValue %d appended before index %d\n", item,index);
    return 0;
};
        

int ListRemoveIndex(struct ListItem** head,const int index)
{
    if (!head || index < 1)
    {
        printf("cant remove index below zero\n");
        return -1;
    }

    struct ListItem* current = *head;

    int i = 1;
    while (current && i < index) //search the node with the specified index
    {
        current = current->next;
        i++;
    }

    if (!current)
    {
        printf("element not found\n");
        return 1;
    }

    if (current->prev)
    {
        current->prev->next = current->next;
    }
    else
    {
        *head = current->next;
    }

    if (current->next)
    {
        current->next->prev = current->prev;
    }

    printf("\nNode %d removed", index);
    free(current);
    return 0;
}

int ListRemoveItem(struct ListItem** head, const int item )
{
    if (!head)
    {
        printf("list not initializated correctly\n");
        return -1;
    }

    struct ListItem* current = *head;
    while (current && current->value != item) //search the node with the specified value
    {
        current = current->next;
    }

    if (!current)
    {
        printf("invalid node\n");
        return 1;
    }

    if (current->prev)
    {
        current->prev->next = current->next;
    }
    else
    {
        *head = current->next;
    }

    if (current->next)
    {
        current->next->prev = current->prev;
    }

    printf("\nNode with value %d removed", item);
    free(current);
    return 0;
}

void PrintList(const struct ListItem *head)//allow to print the list
{
    printf("\n");
    int i=1;
    while (head)
    {   printf("-index:%d ",i);
        printf("|%p ", head);
        printf("value: %d| ", head->value);
        head = head->next;
        i++;
    }
    printf("\n");
}

int main()
{
    struct ListItem *head = NULL;
   
    ListAppend(&head, 100);
    ListAppend(&head, 200);
    ListAppend(&head, 300);
    ListAppend(&head, 400);

    printf("\nList:");
    PrintList(head);

    ListRemoveIndex(&head, 2);
    printf("\n\nlist after removal of index 2: ");
    PrintList(head);

    ListRemoveItem(&head, 100);
    printf("\n\nlist after removal of value 100: ");
    PrintList(head);

    ListAppendAfter(&head, 450, 2);
    printf("\n\nlist after hanging after index 2: ");
    PrintList(head);

    ListAppendBefore(&head, 350, 2);
    printf("\n\nlist before hanging after index 2:");
    PrintList(head);

    return 0;
}
//clang.exe -o .\c\doubleLinked.exe  .\c\doubleLinked.c
```
##

# Set  
### Set.c
```c
#include "linkedList.h"
#include <stddef.h>
#include <string.h>
#include <stdio.h>
#include <stdlib.h>


#define HASHMAP_SIZE 5 //number of nodes
#define HASHMAP_SIZE_LIST 2 // size of nodes



size_t Djb33xHash(const char *key, const size_t keylen)//method for hashing
{
    size_t hash = 5381;
    for (size_t i = 0; i < keylen; i++)
    {
    hash = ((hash << 5) + hash) ^ key[i];
    }
    return hash;
}

struct SetTable *SetTableNew(const size_t HashmapSize) // create a new table
{
    struct SetTable *table = malloc(sizeof(struct SetTable));// allocate space for the table
    if (!table)
    {
        return NULL;
    }

    table->HashmapSize = HashmapSize;
    table->nodes = calloc(table->HashmapSize, sizeof(struct SetNode *));//resize the table for the nodes
    if (!table->nodes)
    {
        free(table);
        return NULL;
    }

    return table;
}

struct SetNode *SetInsert(struct SetTable *table, const char *key, const size_t KeyLen)
{
    size_t hash = Djb33xHash(key, KeyLen);
    size_t index = hash % table->HashmapSize;
    struct SetNode *head = table->nodes[index];

    
    struct SetNode *current = head;
    while (current)
    {
        if (strcmp(current->key, key) == 0)// check if the key is already in the set
        {
            printf("Key: %s already exists, duplicate keys not allowed\n", key);
            return NULL;
        }
        current = current->next;
    }

    size_t NodeCount = 0;
    current = head;
    while (current)
    {
        NodeCount++;
        current = current->next;
    }

    // control of the number of nodes in a node[index] is superior of HASHMAP SIZE LIST
    if (NodeCount >= HASHMAP_SIZE_LIST)
    {
        printf("Collision impossible to append this key\n");
        return NULL;
    }
    struct SetNode *NewItem = ListAppend(table,key,KeyLen,index);
    return NewItem;
    
}

int SetSearch(struct SetTable *table, const char *key, const size_t KeyLen)
{
    size_t hash = Djb33xHash(key, KeyLen);
    size_t index = hash % table->HashmapSize;
    struct SetNode *head = table->nodes[index];
    if (!head)
    {
        printf("node not found");
        return -1;
    }
    struct SetNode *current = head;
    struct SetNode *previous = NULL;
    while (head)
    {  
        previous = current;
        current = current->next; 
        
        if (strcmp(head->key, key) == 0)
        {
            printf("Key: %s found in node: %p\n", key,head);
        }
        head = head->next;
    }
    return 0;
}

struct SetNode *SetRemove(struct SetTable *table, const char *key, const size_t KeyLen)//remove a node given the key
{
    size_t hash = Djb33xHash(key, KeyLen);
    size_t index = hash % table->HashmapSize;
    struct SetNode *head = table->nodes[index];
    while(head)
    {
         if (strcmp(head->key, key) == 0)
         {
            ListRemoveItem(table,key,index);
         }
         head = head->next;
    }
    return NULL;
}

void PrintSet(const struct SetTable *table)
{
    for (int i = 0; i < table->HashmapSize; i++)
    {
        struct SetNode *current = table->nodes[i];
        PrintList(current,i); // function of linkedlistcopy
        printf("NULL\n");
    }
}

int main()
{
    struct SetTable *myset = SetTableNew(HASHMAP_SIZE);
    if (!myset)
    {
        fprintf(stderr, "Failed to create SetTable\n");
        return 1;
    }

    SetInsert(myset, "Hello0", strlen("Hello0"));
    SetInsert(myset, "Hello1", strlen("Hello1"));
    SetInsert(myset, "Hello2", strlen("Hello2"));
    SetInsert(myset, "Hello3", strlen("Hello3"));
    SetInsert(myset, "Hello4", strlen("Hello4"));
    SetInsert(myset, "Hello5", strlen("Hello5"));
    printf("\n \nNodes:\n");
    PrintSet(myset);
    printf("\n \nSearch Hello3:\n");
    SetSearch(myset, "Hello3", strlen("Hello3"));
    printf("\n \nRemove Hello3:\n");
    SetRemove(myset, "Hello3", strlen("Hello3"));
    printf("\n");
    PrintSet(myset);
    printf("\n \nSearch Hello3:\n");
    SetSearch(myset, "Hello3", strlen("Hello3"));
    printf("\n \nInsert Hello 4:\n");
    SetInsert(myset, "Hello4", strlen("Hello4"));
    printf("\n");
    PrintSet(myset);

    return 0;
}

//clang -o .\c\setExercies.exe .\c\setExercies.c .\c\linkedListCopy.c -I .\c\linkedList.h
```
##
 
### linkedList.h
```h
#ifndef LINKEDLIST_H
#define LINKEDLIST_H

struct SetNode
{
    const char *key; //string of the node
    size_t KeyLen; //string length
    struct SetNode *next;//pointer of the next node
};

struct SetTable // contain all the nodes
{
    struct SetNode **nodes; //pointer of nodes
    size_t HashmapSize;
};

//function declaration
struct SetNode* ListAppend(struct SetTable *table, const char* item,size_t KeyLen,size_t index);
int ListRemoveItem(struct SetTable *table, const char* item,size_t index);
void PrintList(struct SetNode *head, int i);
#endif

```
##

### LinkedListCopy.c
```c
#include "linkedList.h"
#include <stddef.h>
#include <stdio.h>
#include <stdlib.h>

unsigned int ListLength(struct SetNode *head)
{
    int i = 0;
    while (head)
    {
        i++;
        head = head->next;
    }
    return i;
}

struct SetNode* ListAppend(struct SetTable *table, const char* item, size_t KeyLen,size_t index)
{
    
    struct SetNode *head = table->nodes[index];
    struct SetNode *new_item = malloc(sizeof(struct SetNode));
    if (!new_item)
    {
        return NULL;
    }

    new_item->key = item;
    new_item->KeyLen = KeyLen;
    new_item->next = NULL;

    if (!head)
    {
        table->nodes[index] = new_item;
        printf("New key: %s appended\n", item);
        return new_item;
    }

    //append the new key
    struct SetNode *tail = head;
    while (tail->next)
    {
        tail = tail->next;
    }

    tail->next = new_item;
    printf("New key: %s appended\n", item);
    return new_item;
}

int ListRemoveItem(struct SetTable *table, const char* item, size_t index)
{
    
    struct SetNode *head = table->nodes[index];
    struct SetNode *previous = NULL;
    if (previous)//if the node is the first we need to reimpost the previous node
    {
       previous->next = head->next;
    }
    else
    {
       table->nodes[index] = head->next;
    }

    free(head);
    printf("key: %s removed\n",item);
             
    return 0;
}


void PrintList(struct SetNode *head,int i)
{
    printf("Node %d: ", i);
        while (head)
        {
            printf("(%s) -> ", head->key);
            head = head->next;
        }
}

```
##


# Dictionary 
### Dictionary.c
```c
#include <stddef.h>
#include <string.h>
#include <stdio.h>
#include <stdlib.h>

#define maxCollision 2

struct DicNode {
    const char *key;  // string of the node
    size_t KeyLen;    // string length
    void *value;       // generic value associated with the key
};

struct DicTable {
    struct DicNode **nodes;  // pointer of nodes
    size_t HashmapSize;
    size_t NumElements;      // number of elements
};

int collision = 0;

size_t Djb33xHash(const char *key, const size_t keylen) //method for hashing
{
    size_t hash = 5381;
    for (size_t i = 0; i < keylen; i++) {
        hash = ((hash << 5) + hash) ^ key[i];
    }
    return hash;
}

struct DicTable *SetTableNew(const size_t InitialSize) // create a new table
{
    struct DicTable *table = malloc(sizeof(struct DicTable));
    if (!table) {
        return NULL;
    }
    table->HashmapSize = InitialSize;
    table->nodes = calloc(table->HashmapSize, sizeof(struct DicNode *));
    if (!table->nodes) {
        free(table);
        return NULL;
    }
    table->NumElements = 0;
    return table;
}

int resize_and_rehash(struct DicTable *table) //allow the resize and the rehashing of the table
{
    size_t NewSize = table->HashmapSize*2;
    struct DicNode **new_nodes = calloc(NewSize, sizeof(struct DicNode *));
    if (!new_nodes) 
    {
        printf("impossible resize the map\n");
        return -1;
    }

    for (size_t i = 0; i < table->HashmapSize; i++) 
    {
        struct DicNode *current = table->nodes[i];
        while (current) 
        {
            size_t new_index = Djb33xHash(current->key, current->KeyLen) % NewSize;
            while (new_nodes[new_index] != NULL)
            {
                new_index = (new_index + 1) % NewSize;
            }
            new_nodes[new_index] = current;
            current = NULL;
        }
    }

    free(table->nodes);
    table->nodes = new_nodes;
    table->HashmapSize = NewSize;
    if (collision<2)
    {
        printf("\nRehashing, table too small\n\n");
    }
    else
    {
        printf("\nRehashing, too many collisione\n\n");
    }
    
    
    collision=0;
    return 0;
}

int DicSearch(struct DicTable *table, const char *key, const size_t KeyLen) //allow to search a key
{
    size_t hash = Djb33xHash(key, KeyLen);
    size_t index = hash % table->HashmapSize;

    while (table->nodes[index] != NULL) 
    {
        if (table->nodes[index]->KeyLen == KeyLen && strncmp(table->nodes[index]->key, key, KeyLen) == 0) 
        {
            return -1;
        }
        index = (index + 1) % table->HashmapSize;
    }
    return 0;
}

struct DicNode *DicInsert(struct DicTable *table, const char *key, const size_t KeyLen, const void *value) {
    
    if (DicSearch(table, key, KeyLen)) {
        printf("Key: %s already in the table. Change key.\n",key);
        return NULL;
    }

    size_t hash = Djb33xHash(key, KeyLen);
    size_t index = hash % table->HashmapSize;

    if (table->nodes[index] != NULL) 
    {
            collision++;
            printf("Key: %s caused a collision\n",key);
            if (collision>=maxCollision) 
            {
                resize_and_rehash(table);
            }
            
            return NULL;
    }

    table->nodes[index] = malloc(sizeof(struct DicNode));
    if (!table->nodes[index]) {
        return NULL;
    }

    table->nodes[index]->key = _strdup(key);
    if (!table->nodes[index]->key) {
        free(table->nodes[index]);
        return NULL;
    }

    table->nodes[index]->KeyLen = KeyLen;
    table->nodes[index]->value = malloc(sizeof(void*));
    if (!table->nodes[index]->value) {
        free((void*)table->nodes[index]->key);
        free(table->nodes[index]);
        return NULL;
    }

    memcpy(table->nodes[index]->value, &value, sizeof(void*));
    table->NumElements++;
    if (collision>=maxCollision||(double)table->NumElements / table->HashmapSize > 0.7) 
    {
        resize_and_rehash(table);
    }
    printf("Key: %s inserted\n",key);
    return table->nodes[index];
}

int DicRemove(struct DicTable *table, const char *key, const size_t KeyLen) //allow to remove a key from the table
{
    size_t hash = Djb33xHash(key, KeyLen);
    size_t index = hash % table->HashmapSize;

    while (table->nodes[index] != NULL) 
    {
        if (table->nodes[index]->KeyLen == KeyLen && strncmp(table->nodes[index]->key, key, KeyLen) == 0) 
        {
            free((void *)table->nodes[index]->key);
            free(table->nodes[index]);
            table->nodes[index] = NULL;
            printf("Key: %s removed\n",key);
            table->NumElements--;

            return 0;
        }
        index = (index + 1) % table->HashmapSize;
    }
    printf("value not found, cannot be removed\n");
    return 0;
}


void PrintDic(struct DicTable *table) //allow to print the dictionary
{
    for (size_t i = 0; i < table->HashmapSize; i++) {
        struct DicNode *current = table->nodes[i];
        printf("Node %zu: ", i+1);

        if (current != NULL) {
            printf("Key: %s, Value: %p\n", current->key, current->value);
        } else {
            printf("NULL\n");
        }
    }
}

int main() 
{
    int HASHMAP_SIZE = 3;
    struct DicTable *myset = SetTableNew(HASHMAP_SIZE);

    if (!myset) 
    {
        printf("error in the creation of the table\n");
        return 1;
    }
    const int value1 = 100;
    const double value2 = 3.14;
    const char* value3 = "Hello";

    DicInsert(myset, "Hello1", strlen("Hello1"), &value1);
    DicInsert(myset, "Hello2", strlen("Hello2"), &value2);
    printf("\n");
    PrintDic(myset);
    printf("\n");
    DicInsert(myset, "Hello3", strlen("Hello3"), &value3);
    DicInsert(myset, "Hello4", strlen("Hello4"), &value1);
    DicInsert(myset, "Hello5", strlen("Hello5"), &value2);
    DicInsert(myset, "Hello6", strlen("Hello6"), &value3);
    DicInsert(myset, "Hello7", strlen("Hello7"), &value1);
    DicInsert(myset, "Hello8", strlen("Hello8"), &value2);
    DicInsert(myset, "Hello9", strlen("Hello9"), &value3);
    PrintDic(myset);
    
    return 0;
}

//clang.exe -o .\c\dictionariesChallenge.exe .\c\dictionariesChallenge.c
```
##
