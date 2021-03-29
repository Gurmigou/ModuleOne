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
### В елементах списку, що зберігається у зв'язному циклічномцу представленні, розміщенні цілі числа. Написати функцію копіювання списку, помінявши порядок елементів на оберненний.
```
#include <iostream>

using namespace std;

struct Node {
	int data;
	Node* next;
};

Node* addElement(Node* node, int data) {							// Функуция добавления елемента в циклический список
	if (!node) {													// Добавления первого элемента
		Node* new_element = new Node;
		new_element->data = data;
		node = new_element;
		node->next = node;											// Указатель на самого себя
	}	
	else {															// Добавление последующих элементов в конец
		Node* curr = node;
		while (curr->next != node) { curr = curr->next; }
		Node* new_element = new Node;
		new_element->data = data;
		new_element->next = node;
		curr->next = new_element;
	}
	return node;
}

void printList(Node* node) {										// Функция печати циклического списка
	Node* curr = node;
	while (curr != nullptr) {
		if (curr->next == node) { cout << curr->data; break; }		// Вывод крайнего элемента + break-point
		cout << curr->data << " ";
		curr = curr->next;
	}
}

Node* copyList(Node* node) {										// Функция копирования списка
	if (!node) { return nullptr; }
	Node* buff = node;
	Node* copy_start = new Node;
	Node* curr = copy_start;

	do {
		curr->data = buff->data; 
		if (buff->next == node) { curr->next = copy_start; break; }
		Node* new_element = new Node;
		curr->next = new_element;
		curr = new_element;
	}
	while (buff = buff->next);
	return copy_start;
}


// << Функция, требующаяся в задаче >>
Node* reversedCopy(Node* node) {									// Функция задающая копию списка node в обратном порядке
	if (!node) { return nullptr; }
	Node* result = copyList(node);									// Создание копии node
	Node* prev = result;
	while (prev->next != result) { prev = prev->next; }				// Последний элемент реверсивного списка -> начало нового списка
	Node* curr = result;

	while (curr->next != result) {									// Переворачиваем копию списка node
		Node* temp = curr->next;
		curr->next = prev;
		prev = curr;
		curr = temp;
	}
	Node* new_temp = curr->next;									// Закидываем последний элемент
	curr->next = prev;
	prev = curr;
	curr = new_temp;
	return prev;													// Возвращаем начало перевернутого списка
}
// << Функция, требующаяся в задаче >>

int main() {
	Node* node = new Node;
	node = nullptr;
	for (int i = 0; i < 22; ++i) {									// Заполняем элементами основной циклический список
		node = addElement(node, i);
	}
	cout << "Original list: ";  printList(node); cout << endl;
	Node* copy = reversedCopy(node);
	cout << "Reversed list: ";  printList(copy);
	return 0;
}
```
### До лінійного списку F з m цілих чисел, більшість елементів якого дорівнює 0, застосоване стисле зв'язне зберігання. Написати функцію для визначення кількості елементів із значенням 0.

```
#include <iostream>

using namespace std;


struct Node {														// Структура списка
	int data;
	Node* next;
};

struct Storage {													// Структура сжатого индексного хранения
	unsigned int index;
	int data;
};

struct StorageNode {												// Структура списка для сжатого индексного хранения
	Storage storage;
	StorageNode* next;
};


Node* addElement(Node* node, int data) {							// Функуция добавления елемента в обычный список
	if (!node) {													// Добавления первого элемента
		Node* new_element = new Node;
		new_element->data = data;
		new_element->next = nullptr;
		node = new_element;
	}
	else {															// Добавление последующих элементов
		Node* curr = node;
		while (curr->next != nullptr) { curr = curr->next; }
		Node* new_element = new Node;
		new_element->data = data;
		new_element->next = nullptr;
		curr->next = new_element;
	}
	return node;
}

void printList(Node* node) {										// Функция печати обычного списка
	Node* curr = node;
	while (curr != nullptr) {
		cout << curr->data << " ";
		curr = curr->next;
	} cout << endl;
}

StorageNode* zeroFreeStorageMaker(Node* node) {						// Функция формирования списка сжатого индексного хранения, 
	if (!node) { return nullptr; }									// который опускает все нулевые элементы
	StorageNode* start = nullptr;
	StorageNode* curr = start;
	unsigned int index = 0;

	while (node) {													// Формируем индексный список, пока не конец основного
		if (start == nullptr && node->data != 0) {					// Если существует ненулевой элемент в основном списке, то
			StorageNode* new_element = new StorageNode;				// создаём первый элемент списка сжатого индексного хранения
			new_element->storage.data = node->data;
			new_element->storage.index = index;
			new_element->next = nullptr;
			start = new_element;
			curr = start;
		}
		else if (start != nullptr && node->data != 0) {				// Если существует первый элемент индексного списка - создаем следующий
			StorageNode* new_element = new StorageNode;
			new_element->storage.data = node->data;
			new_element->storage.index = index;
			new_element->next = nullptr;
			curr->next = new_element;
			curr = curr->next;
		}
		node = node->next;
		index++;
	}
	return start;
}

// << Функция, требующаяся в задаче >>
int zeroesInListCounter(StorageNode* storage, Node* node) {			// Функция посчета нулевых элементов в списке
	unsigned int last_storage_index = 0;
	unsigned int last_node_index = 0;

	while (node) { last_node_index++; node = node->next; }			// Узнаем индекс последнего элемента в обычном списке
	while (storage) { last_storage_index++; storage = storage->next; } // Узнаем индекс последнего элемента в индексном списке
	return last_node_index - last_storage_index;
}
// << Функция, требующаяся в задаче >>

int main() {
	Node* node = new Node;
	node = nullptr;
	 
	for (int i = 0; i < 10; ++i) {									// Заполняем список 10 элементами (можно и больше)
		int a;
		cin >> a;
		node = addElement(node, a);
	}

	StorageNode* storage = zeroFreeStorageMaker(node);				// Создаем список индексного хранения

	cout << "zeroes count: " << zeroesInListCounter(storage, node);	// Узнаем количество нуулевых элементов
	return 0;
}
```
### Лінійний список F цілих чисел зберігається як послідовно-зв'язаний індексний список так, що числа, які маютьоднакову останню цифру, розміщуються в один підсписок. Написати функцію, яка додає до списку елемент зі значенням v, якщо такий елемент у списку відсутній.

```
#include <iostream>
#include <ctime>
#include <cmath>

using namespace std;

struct Node {													// Структура списка (в дальнейшем испольщуется как подсписок)
	int number;
	Node* next;
};

// << Функция, требующаяся в задаче >>
Node* addElement(Node* place, int number) {						// Добваление нового не повторяющегося элемента в подсписок
	if (!place) {												// Добавления первого элемента
		place = new Node;
		place->number = number;
		place->next = nullptr;
		return place;
	}
	else {														// Добавление последующих элементов
		Node* curr = place;
		while (curr != nullptr) {
			if (curr->number == number) { return place; }		// Если элемент уже имеется в списке, добавления не происходит
			if (curr->next == nullptr) { break; }
			curr = curr->next;
		}
		Node* new_element = new Node;
		new_element->number = number;
		new_element->next = nullptr;
		curr->next = new_element;
	}
	return place;
}
// << Функция, требующаяся в задаче >>

void printList(Node** n_arr) {									// Функция печати массива списков по последним цифрам
	for (int i = 0; i < 10; ++i) {
		Node* node = n_arr[i];
		cout << i << ") ";
		while (node != nullptr) {
			cout << node->number << " ";
			node = node->next;
		} cout << endl;
	}
}

int main() {
	Node* n_arr[10];											// Создание массива списков, упорядоченых по последним цифрам
	for (int i = 0; i < 10; ++i) {								// Заполнение массива
		Node* node = new Node;
		node = nullptr;
		n_arr[i] = node;
	}

	int arr[100];												// Массив с переменными

	cout << "Original array: ";
	srand(time(nullptr));
	for (size_t i = 0; i < 20; ++i) {							// Заполнение и вывод изначального массива с переменными
		int number = rand() % 100;
		arr[i] = pow(-1, rand() % 2) * (number % 100);
		cout << arr[i] << " ";
	} cout << endl << endl;

	for (size_t i = 0; i < 20; ++i) {							// Заполнение массива списков
		int index = abs(arr[i] % 10);
		n_arr[index] = addElement(n_arr[index], arr[i]);
	}
	printList(n_arr); cout << endl;

	int check;
	while (true) {
		cout << "Enter new value: ";
		cin >> check;
		int index = abs(check % 10);
		n_arr[index] = addElement(n_arr[index], check);
		printList(n_arr); cout << endl;
	}

	return 0;
}
```
### В елементах списку, що зберігається у зв'язному циклічномцу представленні, розміщенні цілі числа. Написати функцію копіювання списку, помінявши порядок елементів на оберненний.
```
#include <iostream>

using namespace std;

struct Node {
	int data;
	Node* next;
};

Node* addElement(Node* node, int data) {							// Функуция добавления елемента в циклический список
	if (!node) {													// Добавления первого элемента
		Node* new_element = new Node;
		new_element->data = data;
		node = new_element;
		node->next = node;											// Указатель на самого себя
	}	
	else {															// Добавление последующих элементов в конец
		Node* curr = node;
		while (curr->next != node) { curr = curr->next; }
		Node* new_element = new Node;
		new_element->data = data;
		new_element->next = node;
		curr->next = new_element;
	}
	return node;
}

void printList(Node* node) {										// Функция печати циклического списка
	Node* curr = node;
	while (curr != nullptr) {
		if (curr->next == node) { cout << curr->data; break; }		// Вывод крайнего элемента + break-point
		cout << curr->data << " ";
		curr = curr->next;
	}
}

Node* copyList(Node* node) {										// Функция копирования списка
	if (!node) { return nullptr; }
	Node* buff = node;
	Node* copy_start = new Node;
	Node* curr = copy_start;

	do {
		curr->data = buff->data; 
		if (buff->next == node) { curr->next = copy_start; break; }
		Node* new_element = new Node;
		curr->next = new_element;
		curr = new_element;
	}
	while (buff = buff->next);
	return copy_start;
}


// << Функция, требующаяся в задаче >>
Node* reversedCopy(Node* node) {									// Функция задающая копию списка node в обратном порядке
	if (!node) { return nullptr; }
	Node* result = copyList(node);									// Создание копии node
	Node* prev = result;
	while (prev->next != result) { prev = prev->next; }				// Последний элемент реверсивного списка -> начало нового списка
	Node* curr = result;

	while (curr->next != result) {									// Переворачиваем копию списка node
		Node* temp = curr->next;
		curr->next = prev;
		prev = curr;
		curr = temp;
	}
	Node* new_temp = curr->next;									// Закидываем последний элемент
	curr->next = prev;
	prev = curr;
	curr = new_temp;
	return prev;													// Возвращаем начало перевернутого списка
}
// << Функция, требующаяся в задаче >>

int main() {
	Node* node = new Node;
	node = nullptr;
	for (int i = 0; i < 22; ++i) {									// Заполняем элементами основной циклический список
		node = addElement(node, i);
	}
	cout << "Original list: ";  printList(node); cout << endl;
	Node* copy = reversedCopy(node);
	cout << "Reversed list: ";  printList(copy);
	return 0;
}
```
### До лінійного списку F з m цілих чисел, більшість елементів якого дорівнює 0, застосоване стисле зв'язне зберігання. Написати функцію для визначення кількості елементів із значенням 0.

```
#include <iostream>

using namespace std;


struct Node {														// Структура списка
	int data;
	Node* next;
};

struct Storage {													// Структура сжатого индексного хранения
	unsigned int index;
	int data;
};

struct StorageNode {												// Структура списка для сжатого индексного хранения
	Storage storage;
	StorageNode* next;
};


Node* addElement(Node* node, int data) {							// Функуция добавления елемента в обычный список
	if (!node) {													// Добавления первого элемента
		Node* new_element = new Node;
		new_element->data = data;
		new_element->next = nullptr;
		node = new_element;
	}
	else {															// Добавление последующих элементов
		Node* curr = node;
		while (curr->next != nullptr) { curr = curr->next; }
		Node* new_element = new Node;
		new_element->data = data;
		new_element->next = nullptr;
		curr->next = new_element;
	}
	return node;
}

void printList(Node* node) {										// Функция печати обычного списка
	Node* curr = node;
	while (curr != nullptr) {
		cout << curr->data << " ";
		curr = curr->next;
	} cout << endl;
}

StorageNode* zeroFreeStorageMaker(Node* node) {						// Функция формирования списка сжатого индексного хранения, 
	if (!node) { return nullptr; }									// который опускает все нулевые элементы
	StorageNode* start = nullptr;
	StorageNode* curr = start;
	unsigned int index = 0;

	while (node) {													// Формируем индексный список, пока не конец основного
		if (start == nullptr && node->data != 0) {					// Если существует ненулевой элемент в основном списке, то
			StorageNode* new_element = new StorageNode;				// создаём первый элемент списка сжатого индексного хранения
			new_element->storage.data = node->data;
			new_element->storage.index = index;
			new_element->next = nullptr;
			start = new_element;
			curr = start;
		}
		else if (start != nullptr && node->data != 0) {				// Если существует первый элемент индексного списка - создаем следующий
			StorageNode* new_element = new StorageNode;
			new_element->storage.data = node->data;
			new_element->storage.index = index;
			new_element->next = nullptr;
			curr->next = new_element;
			curr = curr->next;
		}
		node = node->next;
		index++;
	}
	return start;
}

// << Функция, требующаяся в задаче >>
int zeroesInListCounter(StorageNode* storage, Node* node) {			// Функция посчета нулевых элементов в списке
	unsigned int last_storage_index = 0;
	unsigned int last_node_index = 0;

	while (node) { last_node_index++; node = node->next; }			// Узнаем индекс последнего элемента в обычном списке
	while (storage) { last_storage_index++; storage = storage->next; } // Узнаем индекс последнего элемента в индексном списке
	return last_node_index - last_storage_index;
}
// << Функция, требующаяся в задаче >>

int main() {
	Node* node = new Node;
	node = nullptr;
	 
	for (int i = 0; i < 10; ++i) {									// Заполняем список 10 элементами (можно и больше)
		int a;
		cin >> a;
		node = addElement(node, a);
	}

	StorageNode* storage = zeroFreeStorageMaker(node);				// Создаем список индексного хранения

	cout << "zeroes count: " << zeroesInListCounter(storage, node);	// Узнаем количество нуулевых элементов
	return 0;
}
```
### Лінійний список F цілих чисел зберігається як послідовно-зв'язаний індексний список так, що числа, які маютьоднакову останню цифру, розміщуються в один підсписок. Написати функцію, яка додає до списку елемент зі значенням v, якщо такий елемент у списку відсутній.

```
#include <iostream>
#include <ctime>
#include <cmath>

using namespace std;

struct Node {													// Структура списка (в дальнейшем испольщуется как подсписок)
	int number;
	Node* next;
};

// << Функция, требующаяся в задаче >>
Node* addElement(Node* place, int number) {						// Добваление нового не повторяющегося элемента в подсписок
	if (!place) {												// Добавления первого элемента
		place = new Node;
		place->number = number;
		place->next = nullptr;
		return place;
	}
	else {														// Добавление последующих элементов
		Node* curr = place;
		while (curr != nullptr) {
			if (curr->number == number) { return place; }		// Если элемент уже имеется в списке, добавления не происходит
			if (curr->next == nullptr) { break; }
			curr = curr->next;
		}
		Node* new_element = new Node;
		new_element->number = number;
		new_element->next = nullptr;
		curr->next = new_element;
	}
	return place;
}
// << Функция, требующаяся в задаче >>

void printList(Node** n_arr) {									// Функция печати массива списков по последним цифрам
	for (int i = 0; i < 10; ++i) {
		Node* node = n_arr[i];
		cout << i << ") ";
		while (node != nullptr) {
			cout << node->number << " ";
			node = node->next;
		} cout << endl;
	}
}

int main() {
	Node* n_arr[10];											// Создание массива списков, упорядоченых по последним цифрам
	for (int i = 0; i < 10; ++i) {								// Заполнение массива
		Node* node = new Node;
		node = nullptr;
		n_arr[i] = node;
	}

	int arr[100];												// Массив с переменными

	cout << "Original array: ";
	srand(time(nullptr));
	for (size_t i = 0; i < 20; ++i) {							// Заполнение и вывод изначального массива с переменными
		int number = rand() % 100;
		arr[i] = pow(-1, rand() % 2) * (number % 100);
		cout << arr[i] << " ";
	} cout << endl << endl;

	for (size_t i = 0; i < 20; ++i) {							// Заполнение массива списков
		int index = abs(arr[i] % 10);
		n_arr[index] = addElement(n_arr[index], arr[i]);
	}
	printList(n_arr); cout << endl;

	int check;
	while (true) {
		cout << "Enter new value: ";
		cin >> check;
		int index = abs(check % 10);
		n_arr[index] = addElement(n_arr[index], check);
		printList(n_arr); cout << endl;
	}

	return 0;
}
```
