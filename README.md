# PinaringanImanSantoso
tugas linked list OTH Circular Double Linked List

Source Code 


        #include <stdio.h>
        #include <stdlib.h>
        
        typedef struct Node {
            int data;
            struct Node* next;
            struct Node* prev;
        } Node;
        
        Node* createNode(int data) {
            Node* newNode = (Node*)malloc(sizeof(Node));
            newNode->data = data;
            newNode->next = newNode->prev = NULL;
            return newNode;
        }
        
        void append(Node** head_ref, int data) {
            Node* newNode = createNode(data);
            if (*head_ref == NULL) {
                *head_ref = newNode;
                newNode->next = newNode->prev = newNode;
            } else {
                Node* last = (*head_ref)->prev;
                last->next = newNode;
                newNode->prev = last;
                newNode->next = *head_ref;
                (*head_ref)->prev = newNode;
            }
        }
        
        void printList(Node* head) {
            if (head == NULL) return;
            Node* temp = head;
            do {
                printf("Address: %p, Data: %d\n", (void*)temp, temp->data);
                temp = temp->next;
            } while (temp != head);
        }
        
        void printListReverse(Node* head) {
            if (head == NULL) return;
            Node* temp = head->prev;
            Node* start = head->prev;
            do {
                printf("Address: %p, Data: %d\n", (void*)temp, temp->data);
                temp = temp->prev;
            } while (temp != start);
        }
        
        void swapNodes(Node** head_ref, Node* node1, Node* node2) {
            if (node1 == node2) return;
        
            Node *prev1 = node1->prev, *next1 = node1->next;
            Node *prev2 = node2->prev, *next2 = node2->next;
        
            if (next1 == node2) { // node1 is adjacent to node2
                node1->next = next2;
                node1->prev = node2;
                node2->next = node1;
                node2->prev = prev1;
                next2->prev = node1;
                prev1->next = node2;
            } else if (next2 == node1) { // node2 is adjacent to node1
                node2->next = next1;
                node2->prev = node1;
                node1->next = node2;
                node1->prev = prev2;
                next1->prev = node2;
                prev2->next = node1;
            } else { // nodes are not adjacent
                prev1->next = node2;
                next1->prev = node2;
                prev2->next = node1;
                next2->prev = node1;
        
                Node* temp = node1->next;
                node1->next = node2->next;
                node2->next = temp;
        
                temp = node1->prev;
                node1->prev = node2->prev;
                node2->prev = temp;
            }
        
            if (*head_ref == node1) {
                *head_ref = node2;
            } else if (*head_ref == node2) {
                *head_ref = node1;
            }
        }
        
        void sortList(Node** head_ref) {
            if (*head_ref == NULL) return;
            int swapped;
            Node* ptr1;
            Node* lptr = NULL;
            Node* head = *head_ref;
        
            do {
                swapped = 0;
                ptr1 = head;
        
                do {
                    if (ptr1->next == head) break;
                    if (ptr1->data > ptr1->next->data) {
                        swapNodes(head_ref, ptr1, ptr1->next);
                        swapped = 1;
                    }
                    ptr1 = ptr1->next;
                } while (ptr1->next != lptr);
        
                lptr = ptr1;
            } while (swapped);
        }
        
        int main() {
            Node* head = NULL;
            int N;
            printf("Enter the number of elements: ");
            scanf("%d", &N);
        
            for (int i = 0; i < N; ++i) {
                int data;
                printf("Enter element %d: ", i + 1);
                scanf("%d", &data);
                append(&head, data);
            }
        
            printf("List before sorting:\n");
            printList(head);
        
            printf("List in reverse order before sorting:\n");
            printListReverse(head);
        
            sortList(&head);
        
            printf("List after sorting:\n");
            printList(head);
        
            return 0;
        }

PENJELASAN CODE :
Struktur Node: Node menyimpan data dan pointer ke node berikutnya dan sebelumnya.
Fungsi createNode: Membuat node baru dengan data yang diberikan.
Fungsi append: Menambahkan node baru ke akhir dari linked list.
Fungsi printList: Mencetak data dan alamat memori dari node secara berurutan.
Fungsi printListReverse: Mencetak data dan alamat memori dari node secara mundur.
Fungsi swapNodes: Menukar dua node dalam linked list.
Fungsi sortList: Mengurutkan linked list menggunakan bubble sort.
Fungsi main: Membaca input, mencetak daftar sebelum dan sesudah pengurutan.
Program ini membaca input, membuat linked list, mencetak alamat memori dan data sebelum dan sesudah pengurutan, serta memastikan tidak ada manipulasi data atau alamat memori yang tidak sah.

            #include <stdio.h>
            #include <stdlib.h>
            
            // Definisi struktur Node untuk linked list ganda
            typedef struct Node {
                int data;              // Data yang disimpan dalam node
                struct Node* next;     // Pointer ke node berikutnya
                struct Node* prev;     // Pointer ke node sebelumnya
            } Node;
            
            // Fungsi untuk membuat node baru dengan data yang diberikan
            Node* createNode(int data) {
                Node* newNode = (Node*)malloc(sizeof(Node));  // Alokasi memori untuk node baru
                newNode->data = data;     // Set nilai data
                newNode->next = newNode->prev = NULL; // Set pointer next dan prev menjadi NULL
                return newNode;  // Mengembalikan node baru
            }
            
            // Fungsi untuk menambahkan node baru ke akhir linked list
            void append(Node** head_ref, int data) {
                Node* newNode = createNode(data);  // Membuat node baru
                if (*head_ref == NULL) {  // Jika linked list kosong
                    *head_ref = newNode;  // Node baru menjadi head
                    newNode->next = newNode->prev = newNode; // Node menunjuk dirinya sendiri
                } else {  // Jika linked list tidak kosong
                    Node* last = (*head_ref)->prev;  // Temukan node terakhir
                    last->next = newNode;  // Node terakhir menunjuk ke node baru
                    newNode->prev = last;  // Node baru menunjuk ke node terakhir
                    newNode->next = *head_ref;  // Node baru menunjuk ke head
                    (*head_ref)->prev = newNode; // Head menunjuk kembali ke node baru
                }
            }
            
            // Fungsi untuk mencetak linked list dalam urutan maju
            void printList(Node* head) {
                if (head == NULL) return;  // Jika linked list kosong, keluar
                Node* temp = head;  // Mulai dari head
                do {
                    printf("Address: %p, Data: %d\n", (void*)temp, temp->data);  // Cetak alamat dan data
                    temp = temp->next;  // Pindah ke node berikutnya
                } while (temp != head);  // Lanjutkan hingga kembali ke head
            }
            
            // Fungsi untuk mencetak linked list dalam urutan mundur
            void printListReverse(Node* head) {
                if (head == NULL) return;  // Jika linked list kosong, keluar
                Node* temp = head->prev;  // Mulai dari node terakhir
                Node* start = head->prev;  // Node terakhir untuk deteksi akhir
                do {
                    printf("Address: %p, Data: %d\n", (void*)temp, temp->data);  // Cetak alamat dan data
                    temp = temp->prev;  // Pindah ke node sebelumnya
                } while (temp != start);  // Lanjutkan hingga kembali ke node terakhir
            }
            
            // Fungsi untuk menukar dua node dalam linked list
            void swapNodes(Node** head_ref, Node* node1, Node* node2) {
                if (node1 == node2) return;  // Jika node yang sama, tidak perlu menukar
            
                Node *prev1 = node1->prev, *next1 = node1->next;
                Node *prev2 = node2->prev, *next2 = node2->next;
            
                if (next1 == node2) { // Jika node1 bersebelahan dengan node2
                    node1->next = next2;
                    node1->prev = node2;
                    node2->next = node1;
                    node2->prev = prev1;
                    next2->prev = node1;
                    prev1->next = node2;
                } else if (next2 == node1) { // Jika node2 bersebelahan dengan node1
                    node2->next = next1;
                    node2->prev = node1;
                    node1->next = node2;
                    node1->prev = prev2;
                    next1->prev = node2;
                    prev2->next = node1;
                } else { // Jika node tidak bersebelahan
                    prev1->next = node2;
                    next1->prev = node2;
                    prev2->next = node1;
                    next2->prev = node1;
            
                    Node* temp = node1->next;
                    node1->next = node2->next;
                    node2->next = temp;
            
                    temp = node1->prev;
                    node1->prev = node2->prev;
                    node2->prev = temp;
                }
            
                if (*head_ref == node1) {
                    *head_ref = node2;  // Update head jika node1 adalah head
                } else if (*head_ref == node2) {
                    *head_ref = node1;  // Update head jika node2 adalah head
                }
            }
            
            // Fungsi untuk mengurutkan linked list menggunakan bubble sort
            void sortList(Node** head_ref) {
                if (*head_ref == NULL) return;  // Jika linked list kosong, keluar
                int swapped;
                Node* ptr1;
                Node* lptr = NULL;
                Node* head = *head_ref;
            
                do {
                    swapped = 0;
                    ptr1 = head;
            
                    do {
                        if (ptr1->next == head) break;  // Jika sudah sampai akhir linked list
                        if (ptr1->data > ptr1->next->data) {  // Jika data lebih besar, tukar
                            swapNodes(head_ref, ptr1, ptr1->next);
                            swapped = 1;  // Tandai telah terjadi pertukaran
                        }
                        ptr1 = ptr1->next;  // Pindah ke node berikutnya
                    } while (ptr1->next != lptr);
            
                    lptr = ptr1;  // Update lptr untuk membatasi area yang sudah terurut
                } while (swapped);  // Ulangi hingga tidak ada pertukaran
            }
            
            // Fungsi utama
            int main() {
                Node* head = NULL;  // Inisialisasi head linked list
                int N;
                printf("Enter the number of elements: ");
                scanf("%d", &N);  // Baca jumlah elemen
            
                for (int i = 0; i < N; ++i) {
                    int data;
                    printf("Enter element %d: ", i + 1);
                    scanf("%d", &data);  // Baca elemen data
                    append(&head, data);  // Tambahkan ke linked list
                }
            
                printf("List before sorting:\n");
                printList(head);  // Cetak linked list sebelum diurutkan
            
                printf("List in reverse order before sorting:\n");
                printListReverse(head);  // Cetak linked list dalam urutan mundur
            
                sortList(&head);  // Urutkan linked list
            
                printf("List after sorting:\n");
                printList(head);  // Cetak linked list setelah diurutkan
            
                return 0;  // Selesai
            }



Hasil Output : ![WhatsApp Image 2024-05-18 at 19 16 03_327374bb](https://github.com/Santosyouknow/PinaringanImanSantoso/assets/161540041/dc5dbf6d-6c43-4573-8ab7-2baf99798116)

![WhatsApp Image 2024-05-18 at 19 16 19_fb92f78f](https://github.com/Santosyouknow/PinaringanImanSantoso/assets/161540041/6c2cdae6-d913-4a99-95d5-77d2128e1ed1)

