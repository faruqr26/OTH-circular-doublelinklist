# OTH-circular-doublelinklist

SOURCE CODE : 
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node *next;
    struct Node *prev;
} Node;

Node *head = NULL;
Node *tail = NULL;

Node *createNode(int data) {
    Node *newNode = (Node *)malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = NULL;
    newNode->prev = NULL;
    return newNode;
}

void insertNode(int data) {
    Node *newNode = createNode(data);

    if (head == NULL) {
        head = newNode;
        tail = newNode;
        newNode->next = newNode;
        newNode->prev = newNode;
    } else {
        tail->next = newNode;
        newNode->prev = tail;
        newNode->next = head;
        head->prev = newNode;
        tail = newNode;
    }
}

void printList() {
    Node *curr = head;
    if (curr == NULL) {
        printf("List is empty\n");
        return;
    }

    do {
        printf("Address: %p, Data: %d\n", (unsigned long)curr, curr->data);
        curr = curr->next;
    } while (curr != head);
}

void swapNodes(Node *a, Node *b) {
    if (a->next == b) { 
        a->next = b->next;
        b->prev = a->prev;
        a->prev->next = b;
        b->next->prev = a;
        b->next = a;
        a->prev = b;
    } else {
        Node *tempNext = a->next;
        Node *tempPrev = a->prev;
        a->next = b->next;
        a->prev = b->prev;
        b->next = tempNext;
        b->prev = tempPrev;
        a->next->prev = a;
        a->prev->next = a;
        b->next->prev = b;
        b->prev->next = b;
    }

    if (head == a) {
        head = b;
    } else if (head == b) {
        head = a;
    }

    if (tail == a) {
        tail = b;
    } else if (tail == b) {
        tail = a;
    }
}

void sortList() {
    if (head == NULL) return;

    int swapped;
    Node *curr;

    do {
        swapped = 0;
        curr = head;

        do {
            Node *nextNode = curr->next;
            if (curr->data > nextNode->data) {
                swapNodes(curr, nextNode);
                swapped = 1;
            } else {
                curr = nextNode;
            }
        } while (curr != tail);
    } while (swapped);
}

int main() {
    int n;
    printf("Masukkan jumlah data: ");
    scanf("%d", &n);

    for (int i = 0; i < n; i++) {
        int data;
        printf("Masukkan data ke-%d: ", i + 1);
        scanf("%d", &data);
        insertNode(data);
    }

    printf("\nList sebelum pengurutan:\n");
    printList();

    sortList();

    printf("\nList setelah pengurutan:\n");
    printList();

    return 0;
}

SS CODE :
![Screenshot 2024-05-21 202954](https://github.com/faruqr26/OTH-circular-doublelinklist/assets/163359023/5523cc3d-4fa4-4589-88ef-73f7b4f3f2f6)
![Screenshot 2024-05-21 203014](https://github.com/faruqr26/OTH-circular-doublelinklist/assets/163359023/9c8f792d-5f1b-4285-ab44-7d325106931e)
![Screenshot 2024-05-21 203030](https://github.com/faruqr26/OTH-circular-doublelinklist/assets/163359023/728bcf22-2177-4813-9e79-8af902f9dc6d)
![Screenshot 2024-05-21 203047](https://github.com/faruqr26/OTH-circular-doublelinklist/assets/163359023/a4eed997-63ba-43a9-ab77-d90cc1bc528a)

SS OUTPUT : 
INPUT 1 
![Screenshot 2024-05-21 202858](https://github.com/faruqr26/OTH-circular-doublelinklist/assets/163359023/486879ec-804a-49a4-9d1e-364792853520)

INPUT 2
![Screenshot 2024-05-21 204342](https://github.com/faruqr26/OTH-circular-doublelinklist/assets/163359023/1e80d383-1d3d-461c-b54e-33611ef39766)

PENJELASAN CODE: 
Penjelasan code : 
1)	#include <stdio.h> //adalah direktif preprosesor yang menyertakan file header standard input-output dari pustaka C.
2)	#include <stdlib.h>//adalah direktif preprosesor yang menyertakan file header standar dari pustaka utilitas umum.
3)	typedef struct Node {// Mendefinisikan struktur Node untuk elemen list ganda (double linked list)
4)	int data;                // Data yang disimpan dalam node
5)	struct Node *next;       // Pointer ke node berikutnya
6)	struct Node *prev;       // Pointer ke node sebelumnya
7)	} Node;
8)	// Mendeklarasikan head dan tail sebagai pointer ke Node, inisialisasi dengan NULL
9)	Node *head = NULL;
10)	Node *tail = NULL;
11)	// Fungsi untuk membuat node baru dengan data yang diberikan
12)	Node *createNode(int data) {
13)	Node *newNode = (Node *)malloc(sizeof(Node));  // Mengalokasikan memori untuk node baru
14)	newNode->data = data;                          // Mengisi data pada node baru
15)	newNode->next = NULL;                          // Inisialisasi pointer next dengan NULL
16)	newNode->prev = NULL;                          // Inisialisasi pointer prev dengan NULL
17)	return newNode;                                // Mengembalikan pointer ke node baru
18)	// Fungsi untuk memasukkan node baru ke dalam list
19)	void insertNode(int data)
20)	Node *newNode = createNode(data);              // Membuat node baru dengan data yang diberikan

21)	if (head == NULL) {                            // Jika list kosong
22)	head = newNode;                            // Node baru menjadi head
23)	tail = newNode;                            // Node baru menjadi tail
24)	newNode->next = newNode;                   // Node baru menunjuk ke dirinya sendiri (circular list)
25)	newNode->prev = newNode;                   // Node baru menunjuk ke dirinya sendiri (circular list)
26)	} else {                                       // Jika list tidak kosong
27)	tail->next = newNode;                      // Node tail saat ini menunjuk ke node baru
28)	newNode->prev = tail;                      // Node baru menunjuk ke node tail saat ini
29)	newNode->next = head;                      // Node baru menunjuk ke head
30)	head->prev = newNode;                      // Head menunjuk ke node baru
31)	tail = newNode;                            // Node baru menjadi tail
32)	// Fungsi untuk mencetak list
33)	void printList() {
34)	Node *curr = head;                             // Pointer ke node saat ini, mulai dari head
35)	if (curr == NULL) {                            // Jika list kosong
36)	printf("List is empty\n");                 // Cetak pesan list kosong
37)	return;                                    // Keluar dari fungsi

38)	do {                                           // Loop untuk mencetak setiap node
39)	printf("Address: %p, Data: %d\n", (unsigned long)curr, curr->data); // Cetak alamat dan data node
40)	curr = curr->next;                         // Pindah ke node berikutnya
41)	} while (curr != head);                        // Berhenti jika kembali ke head

42)	// Fungsi untuk menukar dua node
43)	void swapNodes(Node *a, Node *b) {
44)	if (a->next == b) {                            // Jika b adalah node setelah a
45)	a->next = b->next;                        // a menunjuk ke node setelah b
46)	b->prev = a->prev;                        // b menunjuk ke node sebelum a
47)	a->prev->next = b;                        // Node sebelum a menunjuk ke b
48)	b->next->prev = a;                        // Node setelah b menunjuk ke a
49)	b->next = a;                              // b menunjuk ke a
50)	a->prev = b;                              // a menunjuk ke b
51)	} else {                                      // Jika a dan b tidak bersebelahan
52)	Node *tempNext = a->next;                 // Menyimpan sementara pointer next a
53)	Node *tempPrev = a->prev;                 // Menyimpan sementara pointer prev a
54)	a->next = b->next;                        // a menunjuk ke next b
55)	a->prev = b->prev;                        // a menunjuk ke prev b
56)	b->next = tempNext;                       // b menunjuk ke next sementara a
57)	b->prev = tempPrev;                       // b menunjuk ke prev sementara a
58)	a->next->prev = a;                        // Node setelah a menunjuk ke a
59)	a->prev->next = a;                        // Node sebelum a menunjuk ke a
60)	b->next->prev = b;                        // Node setelah b menunjuk ke b
61)	b->prev->next = b;                        // Node sebelum b menunjuk ke b

62)	if (head == a) {                              // Jika head adalah a
63)	head = b;                                 // Head menjadi b
64)	} else if (head == b) {                       // Jika head adalah b
65)	head = a;                                 // Head menjadi a

66)	if (tail == a) {                              // Jika tail adalah a
67)	tail = b;                                 // Tail menjadi b
68)	} else if (tail == b) {                       // Jika tail adalah b
69)	tail = a;                                 // Tail menjadi a

70)	// Fungsi untuk mengurutkan list menggunakan bubble sort
71)	void sortList() {
72)	if (head == NULL) return;                     // Jika list kosong, keluar dari fungsi

73)	int swapped;                                  // Flag untuk menandai pertukaran
74)	Node *curr;                                   // Pointer ke node saat ini

75)	do {
76)	swapped = 0;                              // Reset flag swapped
77)	curr = head;                              // Mulai dari head

78)	do {
79)	Node *nextNode = curr->next;          // Node setelah node saat ini
80)	if (curr->data > nextNode->data) {    // Jika data node saat ini lebih besar dari data node berikutnya
a.	swapNodes(curr, nextNode);        // Tukar node
b.	swapped = 1;                      // Set flag swapped
81)	} else {
a.	curr = nextNode;                  // Pindah ke node berikutnya

82)	} while (curr != tail);                   // Loop sampai kembali ke tail
83)	} while (swapped);                            // Ulangi selama masih ada pertukaran

84)	int main() {
85)	int n;                                        // Variabel untuk jumlah data
86)	printf("Masukkan jumlah data: ");
87)	scanf("%d", &n);                              // Membaca jumlah data

88)	for (int i = 0; i < n; i++) {                 // Loop untuk memasukkan data
89)	int data;                                 // Variabel untuk data
90)	printf("Masukkan data ke-%d: ", i + 1);
91)	scanf("%d", &data);                       // Membaca data
92)	insertNode(data);                         // Memasukkan data ke dalam list

93)	printf("\nList sebelum pengurutan:\n");
94)	printList();                                  // Mencetak list sebelum diurutkan

95)	sortList();                                   // Mengurutkan list

96)	printf("\nList setelah pengurutan:\n");
97)	printList();                                  // Mencetak list setelah diurutkan

98)	return 0;                                     // Mengembalikan nilai 0, menandakan program berakhir dengan sukses




