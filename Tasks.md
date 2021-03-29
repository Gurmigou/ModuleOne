### Написати функцію для копіювання двосторонньої черги, реалізованої списком з двома звязками.
```
#include <iostream>
using namespace std;
struct Node
{
    int data;
    Node *previous;
    Node *next;
};
struct Deque
{
    Node* front;
    Node* rear;
};
Deque* CopyDeque(Deque* ToBeCopied){
    Deque* CopiedDeque = new Deque();
    int CopiedData;
    Node* NewPrevious= nullptr;
    while (ToBeCopied->front!= nullptr){
        Node* NewNode= new Node();
        CopiedData=ToBeCopied->front->data;
        NewNode->data=CopiedData;
        NewNode->previous=NewPrevious;
        NewPrevious=NewNode;
        if (NewNode->previous != nullptr) {
            NewNode->previous->next=NewNode;
        }
        ToBeCopied->front=ToBeCopied->front->next;
    }
    NewPrevious->next = nullptr;
    CopiedDeque->rear=NewPrevious;
    Node* NewFront = CopiedDeque->rear;
    while(NewFront->previous!=nullptr){
        NewFront=NewFront->previous;
    }
    CopiedDeque->front = NewFront;
    return CopiedDeque;
}
int main() {
    Node c { 12, nullptr, nullptr };
    Node b{ 25, nullptr, &c };
    Node a{ 155, nullptr, &b };

    c.previous = &b;
    b.previous = &a;

    Deque deque{&a, &c};

    Deque* copy = CopyDeque(&deque);
    Node* curr = copy->front;

    while (curr != nullptr) {
        cout << curr->data << " ";
        curr = curr->next;
    }

    return 0;
}
```
### Написати функцію для побудови послідовно-звязного індексного зберігання розрідженої матриці В [10;40]
```
#include <iostream>
using namespace std;
const int length = 10, height =40 ;
struct ListItem {
    int i, j;
    int data;
    ListItem *next;

    ListItem() {};

    ListItem(int, int, int);
};
    void insert(ListItem **, int, ListItem *);
    void print(ListItem **, int);
int main()
{
    ListItem** roots = new ListItem*[height];
    for (int i = 0; i < height; ++i) {
        roots[i] = new ListItem[length];
        roots[i] = nullptr;
    }
    int arr[height][length];
    for (int i = 0; i < height; ++i) {
        for (int j = 0; j < length; ++j)
            cin >> arr[i][j];
    }
    for (int i = 0; i < height; ++i) {
        for (int j = 0; j < length; ++j) {
            if (arr[i][j] != 0) {
                ListItem* newNode = new ListItem(i, j, arr[i][j]);
                insert(roots, length, newNode);
            }
        }
    }
    cout << endl << endl;
    print(roots, length);
    delete[] roots;


void insert(ListItem** roots, int len, ListItem* newNode) {
    insert(&roots[newNode->i], newNode);
}

void print(ListItem** roots, int len) {
    for (int i = 0; i < len; ++i) {
        print(roots[i]);
        cout << endl;
    }
}
}
```
### Написати функцію, яка у списку, що зберігається у звязаному циклічному представленні, з кожної групи сусідніх однакових елементів залишає лише один
```
struct Node {
   int data;
   Node* next;
};

Node* DeleteSimilarItems(Node* head){
    Node* current = head;
    while (current->next!=head) {
        if (current->next->data == current->data) {
            Node *next = current->next->next;
            delete current->next;
            current->next = next;
        } else {
            current = current->next;
        }
    }
    if(current!=head&&current->data==head->data){
        Node* NextHead=head->next;
        delete head;
        current->next=NextHead;
        head=NextHead;
    }
    return head;
}
```
### В елементах списку, що зберігається у зв'язному циклічному представленні, розміщенні цілі числа. Написати функцію, що вилучає зі списку всі від'ємні значення.
```
#include <iostream>
using namespace std;

struct Node {
    int data;
    struct Node* next;
};

void AddNode(struct Node** head, int data)
{
    struct Node* first= (struct Node*)malloc(sizeof(struct Node));

    struct Node* temp = *head;
    first->data = data;
    first->next = *head;

    if (*head != nullptr) {

        while (temp->next != *head) temp = temp->next;
        temp->next = first;

    }
    else {
        first->next = first;
    }

    *head = first;
}

void deleteNode(Node* head_ref, Node* deleted)
{
    struct Node* temp = head_ref;

    if (head_ref == deleted)
        head_ref = deleted->next;

    while (temp->next != deleted) {
        temp = temp->next;
    }

    temp->next = deleted->next;

    free(deleted);

}

void DeleteNegativeNumbers(Node* head)
{
    struct Node* node = head;
    struct Node* next;

    do {

        if (node->data<0)
            deleteNode(head, node);

        next = node->next;
        node = next;

    } while (node != head);
}

void printList(struct Node* head)
{
    struct Node* temp = head;
    if (head != nullptr) {
        do {
            cout<< temp->data<<" ";
            temp = temp->next;
        } while (temp != head);
    }
}

int main()
{
    struct Node* head = nullptr;

    AddNode(&head, 6);
    AddNode(&head, -5);
    AddNode(&head, 12);
    AddNode(&head, -3);
    AddNode(&head, -1);
    AddNode(&head, 9);

    DeleteNegativeNumbers(head);

    printList(head);

    return 0;
}
```
### До лінійного списку F з m цілих чисел, більшість елементів якого дорівнюють 0, застосоване стисле зв'язане зберігання. Написати функцію для визначення кількості елементів із значенням 0,  номери яких належать інтервалу [i,j].
```
#include <iostream>
using namespace std;

struct Node {
    int num;
    int data;
    Node* next;
};

int isInRange(Node* head, int i, int j);

int main() {
    Node d{10, 1, nullptr};
    Node c{5, 32, &d};
    Node b{2, 3, &c};
    Node a{1, 3, &b};

    int i, j;

    cout << "i: ";
    cin >> i;
    cout << "j: ";
    cin >> j;
    cout << isInRange(&a, i, j) << endl;

    return 0;
}

int isInRange(Node* head, int i, int j){

    int nodesInRange = 0;
    int x=0;

    while(head != nullptr){

        if (j>i) {
        if(head->num >= i && head->num <= j){
            nodesInRange++;
        }
        
        head = head->next;
        
        } else {

            swap(i,j);
            if(head->num >= i && head->num <= j){
                nodesInRange++;
            }

            head = head->next;
        }

    }

   return j-nodesInRange-i+1;

}
```
