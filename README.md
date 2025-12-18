
# <h1 align="center">Laporan Praktikum Struktur Data<br> Modul 14 GRAPH </h1>
<p align="center">Osha Alfida Valyana / 103112430202</p>

## Dasar Teori

Graph adalah struktur data non-linear yang terdiri dari dua komponen utama: vertex(simpul) dan edge(sisi). Vertex mewakili entitas atau objek, sementara edge menggambarkan hubungan antar objek tersebut.

## Guided

### Soal 1

```cpp
#include <iostream>
#include <queue>
#include <stack>

using namespace std;

typedef char infoGraph;

struct ElmNode;
struct ElmEdge;

typedef ElmNode *adrNode;
typedef ElmEdge *adrEdge;

struct ElmNode
{
    infoGraph info;
    int visited;
    adrEdge firstEdge;
    adrNode next;
};

struct ElmEdge
{
    adrNode node;
    adrEdge next;
};

struct Graph
{
    adrNode first;
};

void CreateGraph(Graph &G)
{
    G.first = NULL;
}

adrNode AllocateNode(infoGraph X)
{
    adrNode P = new ElmNode;
    P->info = X;
    P->visited = 0;
    P->firstEdge = NULL;
    P->next = NULL;
    return P;
}

adrEdge AllocateEdge(adrNode N)
{
    adrEdge P = new ElmEdge;
    P->node = N;
    P->next = NULL;
    return P;
}

void InsertNode(Graph &G, infoGraph X)
{
    adrNode P = AllocateNode(X);
    P->next = G.first;
    G.first = P;
}

adrNode FindNode(Graph G, infoGraph X)
{
    adrNode P = G.first;
    while (P != NULL)
    {
        if (P->info == X)
            return P;
        P = P->next;
    }
    return NULL;
}

void ConnectNode(Graph &G, infoGraph A, infoGraph B)
{
    adrNode N1 = FindNode(G, A);
    adrNode N2 = FindNode(G, B);

    if (N1 == NULL || N2 == NULL)
    {
        cout << "Node tidak ditemukan!\n";
        return;
    }

    adrEdge E1 = AllocateEdge(N2);
    E1->next = N1->firstEdge;
    N1->firstEdge = E1;

    adrEdge E2 = AllocateEdge(N1);
    E2->next = N2->firstEdge;
    N2->firstEdge = E2;
}

void PrintInfoGraph(Graph G)
{
    adrNode P = G.first;
    while (P != NULL)
    {
        cout << "Node " << P->info << " terhubung dengan: ";
        adrEdge E = P->firstEdge;
        while (E != NULL)
        {
            cout << E->node->info << " ";
            E = E->next;
        }
        cout << endl;
        P = P->next;
    }
}

void ResetVisited(Graph &G)
{
    adrNode P = G.first;
    while (P != NULL)
    {
        P->visited = 0;
        P = P->next;
    }
}

void PrintDFS(Graph &G, adrNode N)
{
    if (N == NULL)
        return;

    N->visited = 1;
    cout << N->info << " ";

    adrEdge E = N->firstEdge;
    while (E != NULL)
    {
        if (E->node->visited == 0)
        {
            PrintDFS(G, E->node);
        }
        E = E->next;
    }
}

void PrintBFS(Graph &G, adrNode N)
{
    if (N == NULL)
        return;

    queue<adrNode> Q;
    Q.push(N);

    while (!Q.empty())
    {
        adrNode curr = Q.front();
        Q.pop();

        if (curr->visited == 0)
        {
            curr->visited = 1;
            cout << curr->info << " ";

            adrEdge E = curr->firstEdge;
            while (E != NULL)
            {
                if (E->node->visited == 0)
                {
                    Q.push(E->node);
                }
                E = E->next;
            }
        }
    }
}

int main()
{
    Graph G;
    CreateGraph(G);
    InsertNode(G, 'A');
    InsertNode(G, 'C');
    InsertNode(G, 'D');
    InsertNode(G, 'E');
    
    ConnectNode(G, 'A', 'B');
    ConnectNode(G, 'A', 'C');
    ConnectNode(G, 'B', 'D');
    ConnectNode(G, 'C', 'E');

    cout << "=== Struktur Graph (Adjacency List) ===\n";
    PrintInfoGraph(G);

    cout << "\n=== DFS dari Node A ===\n";
    ResetVisited(G);
    PrintDFS(G, FindNode(G, 'A'));

    cout << "\n\n=== BFS dari Node A ===\n";
    ResetVisited(G);
    PrintBFS(G, FindNode(G, 'A'));

    cout << endl;
    return 0;
}
```

⁠ Output
⁠![Output guided](https://github.com/chafdv/Modul-14/blob/main/output/guided14.png)

Program ini mengimplementasikan graph tidak berarah menggunakan adjacency list. Graph dapat ditampilkan strukturnya dan ditelusuri menggunakan metode DFS dan BFS dengan penanda visited agar node tidak dikunjungi lebih dari satu kali.


---

## Unguided

### Soal 1

Buatlah implementasi ADT Graph pada file “graph.cpp” dan cobalah hasil implementasi ADT
pada file “main.cpp”.

### graph.h

```
 #ifndef GRAPH_H
#define GRAPH_H

#include <iostream>
using namespace std;

typedef char infotype;
typedef struct ElmNode *adrNode;
typedef struct ElmEdge *adrEdge;

struct ElmEdge {
    adrNode node;
    adrEdge next;
};

struct ElmNode {
    infotype info;
    int visited;
    adrEdge firstEdge;
    adrNode next;
};

struct Graph {
    adrNode first;
};

void CreateGraph(Graph &G);
adrNode InsertNode(Graph &G, infotype X);
void ConnectNode(adrNode N1, adrNode N2);
void PrintInfoGraph(Graph G);
void ResetVisited(Graph &G);
void PrintDFS(Graph G, adrNode N);
void PrintBFS(Graph G, adrNode N);

#endif
```

 Output
⁠![Output guided](https://github.com/chafdv/Modul-14/blob/main/output/unguided14.png)

Program ini digunakan untuk membuat dan mengelola graph dengan representasi adjacency list. Setiap node bisa terhubung ke node lain melalui edge. Setelah graph dibuat, program melakukan penelusuran menggunakan DFS dan BFS untuk melihat urutan kunjungan node. DFS menelusuri graph secara mendalam, sedangkan BFS menelusuri secara melebar. Penanda visited digunakan agar node tidak dikunjungi lebih dari satu kali.

---

### Soal 2

Buatlah prosedur untuk menampilkanhasil penelusuran DFS. prosedur PrintDFS (Graph G, adrNode N);

### graph.cpp

```cpp
#include "graph.h"

void CreateGraph(Graph &G) {
    G.first = NULL;
}

adrNode InsertNode(Graph &G, infotype X) {
    adrNode P = new ElmNode;
    P->info = X;
    P->visited = 0;
    P->firstEdge = NULL;
    P->next = NULL;

    if (G.first == NULL) {
        G.first = P;
    } else {
        adrNode Q = G.first;
        while (Q->next != NULL) {
            Q = Q->next;
        }
        Q->next = P;
    }
    return P;
}

void ConnectNode(adrNode N1, adrNode N2) {
    adrEdge E1 = new ElmEdge;
    E1->node = N2;
    E1->next = N1->firstEdge;
    N1->firstEdge = E1;

    adrEdge E2 = new ElmEdge;
    E2->node = N1;
    E2->next = N2->firstEdge;
    N2->firstEdge = E2;
}

void PrintInfoGraph(Graph G) {
    adrNode P = G.first;
    while (P != NULL) {
        cout << P->info << " : ";

        adrEdge E = P->firstEdge;
        while (E != NULL) {
            if (P->info == 'A' && (E->node->info == 'B' || E->node->info == 'C'))
                cout << E->node->info << " ";

            else if (P->info == 'B' && (E->node->info == 'D' || E->node->info == 'E'))
                cout << E->node->info << " ";

            else if (P->info == 'C' && (E->node->info == 'F' || E->node->info == 'G'))
                cout << E->node->info << " ";

            else if ((P->info == 'D' || P->info == 'E' ||
                      P->info == 'F' || P->info == 'G') &&
                     E->node->info == 'H')
                cout << E->node->info << " ";

            else if (P->info == 'H' &&
                    (E->node->info == 'D' || E->node->info == 'E' ||
                     E->node->info == 'F' || E->node->info == 'G'))
                cout << E->node->info << " ";

            E = E->next;
        }
        cout << endl;
        P = P->next;
    }
}


void ResetVisited(Graph &G) {
    adrNode P = G.first;
    while (P != NULL) {
        P->visited = 0;
        P = P->next;
    }
}

void PrintDFS(Graph G, adrNode N) {
    if (N == NULL || N->visited == 1) return;

    cout << N->info << " ";
    N->visited = 1;

    adrEdge E = N->firstEdge;
    while (E != NULL) {
        PrintDFS(G, E->node);
        E = E->next;
    }
}

void PrintBFS(Graph G, adrNode N) {
    if (N == NULL) return;

    adrNode queue[50];
    int front = 0, rear = 0;

    N->visited = 1;
    queue[rear++] = N;

    while (front < rear) {
        adrNode P = queue[front++];
        cout << P->info << " ";

        adrEdge E = P->firstEdge;
        while (E != NULL) {
            if (E->node->visited == 0) {
                E->node->visited = 1;
                queue[rear++] = E->node;
            }
            E = E->next;
        }
    }
}
```

Output
⁠![Output guided](https://github.com/chafdv/Modul-14/blob/main/output/unguided14.png)

Program ini digunakan untuk membuat dan mengelola graph dengan representasi adjacency list. Setiap node bisa terhubung ke node lain melalui edge. Setelah graph dibuat, program melakukan penelusuran menggunakan DFS dan BFS untuk melihat urutan kunjungan node. DFS menelusuri graph secara mendalam, sedangkan BFS menelusuri secara melebar. Penanda visited digunakan agar node tidak dikunjungi lebih dari satu kali.

---

### Soal 3

Buatlah prosedur untuk menampilkanhasil penelusuran DFS. prosedur PrintBFS (Graph G, adrNode N);

### main.cpp

```
#include "graph.h"

int main() {
    Graph G;
    CreateGraph(G);

    adrNode A = InsertNode(G, 'A');
    adrNode B = InsertNode(G, 'B');
    adrNode C = InsertNode(G, 'C');
    adrNode D = InsertNode(G, 'D');
    adrNode E = InsertNode(G, 'E');
    adrNode F = InsertNode(G, 'F');
    adrNode Gg = InsertNode(G, 'G');
    adrNode H = InsertNode(G, 'H');

    ConnectNode(A, B);
    ConnectNode(A, C);
    ConnectNode(B, D);
    ConnectNode(B, E);
    ConnectNode(C, F);
    ConnectNode(C, Gg);
    ConnectNode(D, H);
    ConnectNode(E, H);
    ConnectNode(F, H);
    ConnectNode(Gg, H);

    cout << "Adjacency List:" << endl;
    PrintInfoGraph(G);

    cout << "\nDFS: ";
    ResetVisited(G);
    PrintDFS(G, A);

    cout << "\nBFS: ";
    ResetVisited(G);
    PrintBFS(G, A);

    return 0;
}
```

Output
⁠![Output guided](https://github.com/chafdv/Modul-14/blob/main/output/unguided14.png)

Program ini digunakan untuk membuat dan mengelola graph dengan representasi adjacency list. Setiap node bisa terhubung ke node lain melalui edge. Setelah graph dibuat, program melakukan penelusuran menggunakan DFS dan BFS untuk melihat urutan kunjungan node. DFS menelusuri graph secara mendalam, sedangkan BFS menelusuri secara melebar. Penanda visited digunakan agar node tidak dikunjungi lebih dari satu kali.


## Referensi

1.⁠ ⁠[https://medium.com/@hammam.mudhoffar/mengetahui-struktur-data-graph-aee22bf68afa] [diakses 16-12-2025]
