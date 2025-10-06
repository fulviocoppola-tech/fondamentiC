# LEZIONE 17 - Puntatori

## PARTE TEORICA (2h)

### 1. Concetto di Puntatore

Un **puntatore** è una variabile che contiene l'**indirizzo di memoria** di un'altra variabile.

**Analogia della vita reale:**
```
Immagina la memoria come un grande edificio con appartamenti:
- Ogni appartamento ha un NUMERO (indirizzo)
- Ogni appartamento contiene OGGETTI (valori)
- Un puntatore è come un POST-IT che dice "vai all'appartamento 1234"
```

**Rappresentazione in memoria:**
```
Memoria:
Indirizzo    Variabile    Valore
────────────────────────────────
0x1000       int x        42
0x1004       int y        10
0x1008       int* p       0x1000  (punta a x!)
0x100C       int* q       0x1004  (punta a y!)

p contiene l'INDIRIZZO di x (0x1000)
*p accede al VALORE di x (42)
```

**Perché i puntatori sono importanti?**
- **Passaggio per riferimento**: modificare variabili nelle funzioni
- **Array dinamici**: allocare memoria durante l'esecuzione
- **Strutture dati complesse**: liste, alberi, grafi
- **Efficienza**: evitare copie di grandi strutture
- **Manipolazione di stringhe**: modificare stringhe in modo efficiente

---

### 2. Operatori & (indirizzo) e * (deferenziazione)

**Operatore & (address-of)**: restituisce l'indirizzo di una variabile

```c
int x = 42;
int* p;

p = &x;  // p ora contiene l'indirizzo di x

printf("Valore di x: %d\n", x);           // 42
printf("Indirizzo di x: %p\n", &x);       // 0x7ffd... (esempio)
printf("Valore di p: %p\n", p);           // 0x7ffd... (stesso indirizzo!)
```

**Operatore * (dereference)**: accede al valore puntato

```c
int x = 42;
int* p = &x;

printf("Valore di x: %d\n", x);      // 42
printf("Valore puntato da p: %d\n", *p);  // 42 (stesso valore!)

*p = 100;  // Modifica il valore di x attraverso il puntatore!

printf("Nuovo valore di x: %d\n", x);     // 100
printf("Valore puntato da p: %d\n", *p);  // 100
```

**Attenzione alla dichiarazione:**
```c
int* p, q;    // p è un puntatore, q è un int normale!
int *p, *q;   // Entrambi sono puntatori (raccomandato)
int* p;       // p è un puntatore
int *p;       // Equivalente (stile diverso)
```

**Esempio completo:**
```c
#include <stdio.h>

int main() {
    int numero = 42;
    int* puntatore;
    
    puntatore = &numero;  // puntatore punta a numero
    
    printf("=== INFORMAZIONI ===\n");
    printf("Valore di numero: %d\n", numero);
    printf("Indirizzo di numero: %p\n", &numero);
    printf("\n");
    printf("Valore di puntatore: %p\n", puntatore);
    printf("Valore puntato da puntatore: %d\n", *puntatore);
    printf("Indirizzo di puntatore: %p\n", &puntatore);
    printf("\n");
    
    // Modifica attraverso il puntatore
    *puntatore = 100;
    printf("Dopo *puntatore = 100:\n");
    printf("Valore di numero: %d\n", numero);  // 100!
    printf("Valore puntato: %d\n", *puntatore);  // 100!
    
    return 0;
}
```

**Visualizzazione:**
```
Prima:
┌─────────────┐         ┌─────────────┐
│   numero    │         │  puntatore  │
│  valore: 42 │    ┌───│ valore: 0x.. │
│ ind: 0x1000 │◄───┘    │ ind: 0x1008 │
└─────────────┘         └─────────────┘

puntatore = &numero  → puntatore contiene 0x1000
*puntatore           → accede al valore 42

Dopo *puntatore = 100:
┌─────────────┐         ┌─────────────┐
│   numero    │         │  puntatore  │
│ valore: 100 │◄───────│ valore: 0x.. │
│ ind: 0x1000 │         │ ind: 0x1008 │
└─────────────┘         └─────────────┘
```

---

### 3. Dichiarazione e Inizializzazione di Puntatori

**Sintassi:**
```c
tipo* nomePuntatore;
```

**Tipi di puntatori:**
```c
int* p1;      // Puntatore a int
float* p2;    // Puntatore a float
char* p3;     // Puntatore a char
double* p4;   // Puntatore a double
```

**Inizializzazione:**
```c
// 1. Puntatore non inizializzato (PERICOLOSO!)
int* p;  // Contiene un indirizzo casuale (garbage)
// *p = 10;  // ERRORE! Accesso a memoria casuale

// 2. Inizializzazione a NULL (buona pratica)
int* p = NULL;  // Puntatore nullo, non punta a nulla
if (p == NULL) {
    printf("Puntatore non valido\n");
}

// 3. Inizializzazione con indirizzo
int x = 10;
int* p = &x;  // p punta a x
printf("%d\n", *p);  // 10

// 4. Assegnamento successivo
int y = 20;
int* q;
q = &y;  // OK
```

**Puntatori e costanti:**
```c
int x = 10, y = 20;

// Puntatore a costante (il valore non può essere modificato)
const int* p1 = &x;
// *p1 = 15;  // ERRORE! Non si può modificare *p1
p1 = &y;      // OK, il puntatore può cambiare

// Puntatore costante (il puntatore non può cambiare)
int* const p2 = &x;
*p2 = 15;     // OK, si può modificare il valore
// p2 = &y;   // ERRORE! Il puntatore è costante

// Puntatore costante a costante
const int* const p3 = &x;
// *p3 = 15;  // ERRORE!
// p3 = &y;   // ERRORE!
```

**ERRORI COMUNI:**
```c
int* p;
*p = 42;  // ERRORE! p non è inizializzato

int* p = NULL;
*p = 42;  // ERRORE! Dereferenziare NULL causa crash

int x = 10;
int* p = x;  // ERRORE! p deve contenere un indirizzo, non un valore
// Corretto: int* p = &x;

int* p;
int x = 10;
p = x;  // ERRORE! Assegnare valore a puntatore
// Corretto: p = &x;
```

---

### 4. Aritmetica dei Puntatori

I puntatori supportano operazioni aritmetiche, utili per navigare in array.

**Operazioni valide:**
- `p + n` : sposta il puntatore avanti di n elementi
- `p - n` : sposta il puntatore indietro di n elementi
- `p++` : sposta al prossimo elemento
- `p--` : sposta all'elemento precedente
- `p1 - p2` : distanza tra due puntatori (in elementi)

**Esempio con array:**
```c
int numeri[] = {10, 20, 30, 40, 50};
int* p = numeri;  // p punta al primo elemento

printf("%d\n", *p);      // 10 (primo elemento)
printf("%d\n", *(p+1));  // 20 (secondo elemento)
printf("%d\n", *(p+2));  // 30 (terzo elemento)

p++;  // Sposta al prossimo elemento
printf("%d\n", *p);      // 20

p += 2;  // Sposta di 2 posizioni
printf("%d\n", *p);      // 40
```

**Importante: Il salto dipende dal tipo!**
```c
int arr[] = {10, 20, 30};
int* p = arr;

// Se p è all'indirizzo 0x1000:
// p+1 NON è 0x1001, ma 0x1004! (sizeof(int) = 4 byte)
// p+2 è 0x1008

printf("Indirizzo p:   %p\n", p);      // 0x1000
printf("Indirizzo p+1: %p\n", p+1);    // 0x1004
printf("Indirizzo p+2: %p\n", p+2);    // 0x1008
```

**Rappresentazione in memoria:**
```
Array: int numeri[] = {10, 20, 30, 40, 50};

Memoria:
Indirizzo   Valore    Puntatore
─────────────────────────────────
0x1000      10    ← p (numeri)
0x1004      20    ← p + 1
0x1008      30    ← p + 2
0x100C      40    ← p + 3
0x1010      50    ← p + 4

*p = 10
*(p+1) = 20
*(p+2) = 30
```

**Notazioni equivalenti:**
```c
int arr[] = {10, 20, 30, 40, 50};
int* p = arr;

// Queste espressioni sono EQUIVALENTI:
arr[2]    ==    *(arr + 2)    ==    *(p + 2)    ==    p[2]
// Tutte accedono al terzo elemento (30)

// Il compilatore traduce arr[i] in *(arr + i)
```

**Iterare con puntatori:**
```c
int numeri[] = {10, 20, 30, 40, 50};
int* p;

// Metodo 1: con indice
for (int i = 0; i < 5; i++) {
    printf("%d ", *(numeri + i));
}

// Metodo 2: incrementando il puntatore
for (p = numeri; p < numeri + 5; p++) {
    printf("%d ", *p);
}

// Metodo 3: stile compatto
for (p = numeri; p < numeri + 5; ) {
    printf("%d ", *p++);  // Stampa e poi incrementa
}
```

---

### 5. Puntatori e Array

Gli array e i puntatori sono strettamente collegati in C.

**Il nome dell'array è un puntatore costante al primo elemento:**
```c
int arr[] = {10, 20, 30};

// arr è equivalente a &arr[0]
printf("%p\n", arr);      // Indirizzo del primo elemento
printf("%p\n", &arr[0]);  // Stesso indirizzo!

// Quindi:
int* p = arr;  // OK (senza &)
// è equivalente a:
int* p = &arr[0];
```

**Accesso agli elementi:**
```c
int arr[] = {10, 20, 30, 40, 50};

// Notazione array
printf("%d\n", arr[0]);  // 10
printf("%d\n", arr[2]);  // 30

// Notazione puntatore
printf("%d\n", *arr);      // 10 (primo elemento)
printf("%d\n", *(arr+2));  // 30 (terzo elemento)

// Con puntatore separato
int* p = arr;
printf("%d\n", p[0]);    // 10
printf("%d\n", p[2]);    // 30
printf("%d\n", *(p+2));  // 30
```

**Differenza importante: array vs puntatore**
```c
int arr[5] = {1, 2, 3, 4, 5};
int* p = arr;

// arr è COSTANTE, p è VARIABILE
p++;       // OK: p ora punta al secondo elemento
// arr++;  // ERRORE! Non si può modificare arr

// sizeof comportamento diverso
printf("%lu\n", sizeof(arr));  // 20 (5 * 4 byte)
printf("%lu\n", sizeof(p));    // 8 (dimensione di un puntatore su 64-bit)
```

**Array multidimensionali e puntatori:**
```c
int matrice[3][4] = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};

// matrice è un puntatore a un array di 4 int
// matrice[0] è un puntatore al primo elemento della prima riga

printf("%d\n", matrice[1][2]);     // 7
printf("%d\n", *(*(matrice+1)+2)); // 7 (equivalente!)

// Iterare con puntatori
int* p = &matrice[0][0];
for (int i = 0; i < 12; i++) {
    printf("%d ", *p++);
}
```

---

### 6. Passaggio per Riferimento

Con i puntatori possiamo modificare variabili all'interno delle funzioni.

**Problema del passaggio per valore:**
```c
void incrementa(int x) {
    x++;
    printf("Dentro funzione: x = %d\n", x);
}

int main() {
    int numero = 10;
    incrementa(numero);
    printf("Fuori funzione: numero = %d\n", numero);  // 10 (non cambiato!)
    return 0;
}
```

**Soluzione: passaggio per riferimento (con puntatori)**
```c
void incrementa(int* x) {
    (*x)++;  // Modifica il valore puntato
    printf("Dentro funzione: *x = %d\n", *x);
}

int main() {
    int numero = 10;
    incrementa(&numero);  // Passa l'indirizzo
    printf("Fuori funzione: numero = %d\n", numero);  // 11 (cambiato!)
    return 0;
}
```

**Swap di due variabili:**
```c
// SBAGLIATO: passaggio per valore
void swapSbagliato(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
    // Scambia solo le copie locali!
}

// CORRETTO: passaggio per riferimento
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 10, y = 20;
    
    printf("Prima: x=%d, y=%d\n", x, y);
    swap(&x, &y);
    printf("Dopo: x=%d, y=%d\n", x, y);  // x=20, y=10
    
    return 0;
}
```

**Funzioni che modificano array:**
```c
void raddoppiaArray(int arr[], int dim) {
    for (int i = 0; i < dim; i++) {
        arr[i] *= 2;
    }
}

// Equivalente con puntatore esplicito:
void raddoppiaArray2(int* arr, int dim) {
    for (int i = 0; i < dim; i++) {
        *(arr + i) *= 2;
    }
}

int main() {
    int numeri[] = {1, 2, 3, 4, 5};
    raddoppiaArray(numeri, 5);
    // numeri è ora {2, 4, 6, 8, 10}
    return 0;
}
```

**Restituzione di multipli valori:**
```c
void divisioneCompleta(int dividendo, int divisore, int* quoziente, int* resto) {
    *quoziente = dividendo / divisore;
    *resto = dividendo % divisore;
}

int main() {
    int q, r;
    divisioneCompleta(17, 5, &q, &r);
    printf("17 / 5 = %d con resto %d\n", q, r);  // 3 con resto 2
    return 0;
}
```

---

### 7. Puntatori e Stringhe

Le stringhe in C sono array di caratteri, quindi puntatori a char.

**Dichiarazione:**
```c
char str1[] = "Ciao";      // Array di caratteri
char* str2 = "Ciao";       // Puntatore a stringa letterale

// str1 è modificabile, str2 NON è modificabile (stringa in memoria read-only)
str1[0] = 'c';  // OK
// str2[0] = 'c';  // ERRORE! Comportamento indefinito
```

**Iterare con puntatori:**
```c
char stringa[] = "Hello";
char* p = stringa;

while (*p != '\0') {
    printf("%c ", *p);
    p++;
}
// Output: H e l l o
```

**Funzioni con stringhe e puntatori:**
```c
// Calcola lunghezza stringa
int lunghezza(char* str) {
    int len = 0;
    while (*str != '\0') {
        len++;
        str++;
    }
    return len;
}

// Copia stringa
void copia(char* dest, char* src) {
    while (*src != '\0') {
        *dest = *src;
        dest++;
        src++;
    }
    *dest = '\0';  // Aggiungi terminatore
}

// Versione compatta
void copia2(char* dest, char* src) {
    while ((*dest++ = *src++) != '\0');
}

int main() {
    char str1[] = "Ciao";
    char str2[50];
    
    printf("Lunghezza: %d\n", lunghezza(str1));
    copia(str2, str1);
    printf("Copia: %s\n", str2);
    
    return 0;
}
```

**Array di stringhe (array di puntatori):**
```c
char* giorni[] = {
    "Lunedì",
    "Martedì",
    "Mercoledì",
    "Giovedì",
    "Venerdì",
    "Sabato",
    "Domenica"
};

for (int i = 0; i < 7; i++) {
    printf("%s\n", giorni[i]);
}

// giorni è un array di puntatori a char
// giorni[0] punta a "Lunedì"
// giorni[1] punta a "Martedì"
// ecc.
```

---

## PARTE PRATICA (2h)

### Esercizio 1: Basi dei Puntatori

```c
#include <stdio.h>

int main() {
    int x = 42;
    int y = 100;
    int* p1;
    int* p2;
    
    printf("=== ESEMPIO BASE PUNTATORI ===\n\n");
    
    // Assegnamento
    p1 = &x;
    p2 = &y;
    
    printf("Variabili:\n");
    printf("x = %d (indirizzo: %p)\n", x, &x);
    printf("y = %d (indirizzo: %p)\n", y, &y);
    printf("\n");
    
    printf("Puntatori:\n");
    printf("p1 = %p (punta a x)\n", p1);
    printf("*p1 = %d (valore di x)\n", *p1);
    printf("&p1 = %p (indirizzo di p1)\n", &p1);
    printf("\n");
    printf("p2 = %p (punta a y)\n", p2);
    printf("*p2 = %d (valore di y)\n", *p2);
    printf("&p2 = %p (indirizzo di p2)\n", &p2);
    printf("\n");
    
    // Modifica attraverso puntatori
    printf("=== MODIFICA VALORI ===\n");
    printf("Prima: x=%d, y=%d\n", x, y);
    
    *p1 = 999;
    *p2 = 777;
    
    printf("Dopo *p1=999 e *p2=777:\n");
    printf("x=%d, y=%d\n", x, y);
    printf("\n");
    
    // Puntatori che puntano allo stesso indirizzo
    printf("=== PUNTATORI MULTIPLI ===\n");
    p2 = p1;  // p2 ora punta a x (come p1)
    
    printf("p1 e p2 puntano entrambi a x\n");
    printf("*p1 = %d, *p2 = %d\n", *p1, *p2);
    
    *p2 = 555;
    printf("Dopo *p2 = 555:\n");
    printf("x = %d, *p1 = %d, *p2 = %d\n", x, *p1, *p2);
    
    return 0;
}
```

---

### Esercizio 2: Swap e Passaggio per Riferimento

```c
#include <stdio.h>

// Prototipi
void swap(int* a, int* b);
void incrementa(int* x);
void triplo(int* x);
void divisioneCompleta(int dividendo, int divisore, int* q, int* r);
void statistiche(int arr[], int dim, int* somma, float* media, int* max, int* min);

int main() {
    // Test swap
    printf("=== SWAP ===\n");
    int x = 10, y = 20;
    printf("Prima: x=%d, y=%d\n", x, y);
    swap(&x, &y);
    printf("Dopo: x=%d, y=%d\n\n", x, y);
    
    // Test incrementa
    printf("=== INCREMENTA ===\n");
    int num = 5;
    printf("Prima: num=%d\n", num);
    incrementa(&num);
    printf("Dopo incrementa: num=%d\n", num);
    triplo(&num);
    printf("Dopo triplo: num=%d\n\n", num);
    
    // Test divisione completa
    printf("=== DIVISIONE COMPLETA ===\n");
    int q, r;
    divisioneCompleta(17, 5, &q, &r);
    printf("17 / 5 = %d con resto %d\n\n", q, r);
    
    // Test statistiche
    printf("=== STATISTICHE ARRAY ===\n");
    int array[] = {12, 45, 7, 23, 89, 34, 16};
    int dim = 7;
    int somma, max, min;
    float media;
    
    statistiche(array, dim, &somma, &media, &max, &min);
    
    printf("Array: ");
    for (int i = 0; i < dim; i++) {
        printf("%d ", array[i]);
    }
    printf("\n");
    printf("Somma: %d\n", somma);
    printf("Media: %.2f\n", media);
    printf("Massimo: %d\n", max);
    printf("Minimo: %d\n", min);
    
    return 0;
}

void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

void incrementa(int* x) {
    (*x)++;
}

void triplo(int* x) {
    *x = *x * 3;
}

void divisioneCompleta(int dividendo, int divisore, int* q, int* r) {
    if (divisore == 0) {
        *q = 0;
        *r = 0;
        return;
    }
    *q = dividendo / divisore;
    *r = dividendo % divisore;
}

void statistiche(int arr[], int dim, int* somma, float* media, int* max, int* min) {
    *somma = 0;
    *max = arr[0];
    *min = arr[0];
    
    for (int i = 0; i < dim; i++) {
        *somma += arr[i];
        if (arr[i] > *max) *max = arr[i];
        if (arr[i] < *min) *min = arr[i];
    }
    
    *media = (float)(*somma) / dim;
}
```

---

### Esercizio 3: Aritmetica dei Puntatori

```c
#include <stdio.h>

void stampaArrayConPuntatori(int* arr, int dim);
void invertiConPuntatori(int* arr, int dim);
int sommaConPuntatori(int* arr, int dim);
int* trovaMax(int* arr, int dim);

int main() {
    int numeri[] = {10, 20, 30, 40, 50, 60, 70};
    int dim = 7;
    
    printf("=== ARITMETICA DEI PUNTATORI ===\n\n");
    
    // Iterazione con puntatori
    printf("Array originale:\n");
    stampaArrayConPuntatori(numeri, dim);
    
    // Accesso con aritmetica
    printf("\nAccesso con aritmetica:\n");
    int* p = numeri;
    printf("*p = %d\n", *p);           // 10
    printf("*(p+2) = %d\n", *(p+2));   // 30
    printf("*(p+5) = %d\n", *(p+5));   // 60
    printf("\n");
    
    // Indirizzi
    printf("Indirizzi in memoria:\n");
    for (int i = 0; i < dim; i++) {
        printf("numeri[%d]: indirizzo=%p, valore=%d\n", 
               i, (numeri + i), *(numeri + i));
    }
    printf("\n");
    
    // Differenza tra indirizzi
    printf("Differenza tra puntatori:\n");
    int* p1 = &numeri[0];
    int* p2 = &numeri[6];
    printf("p2 - p1 = %ld elementi\n", p2 - p1);
    printf("Distanza in byte: %ld\n", (char*)p2 - (char*)p1);
    printf("\n");
    
    // Somma
    int somma = sommaConPuntatori(numeri, dim);
    printf("Somma di tutti gli elementi: %d\n\n", somma);
    
    // Trova massimo
    int* maxPtr = trovaMax(numeri, dim);
    printf("Elemento massimo: %d\n", *maxPtr);
    printf("Posizione: %ld\n\n", maxPtr - numeri);
    
    // Inversione
    printf("Inversione array:\n");
    invertiConPuntatori(numeri, dim);
    stampaArrayConPuntatori(numeri, dim);
    
    return 0;
}

void stampaArrayConPuntatori(int* arr, int dim) {
    int* p = arr;
    int* fine = arr + dim;
    
    printf("[ ");
    while (p < fine) {
        printf("%d ", *p);
        p++;
    }
    printf("]\n");
}

void invertiConPuntatori(int* arr, int dim) {
    int* inizio = arr;
    int* fine = arr + dim - 1;
    
    while (inizio < fine) {
        int temp = *inizio;
        *inizio = *fine;
        *fine = temp;
        inizio++;
        fine--;
    }
}

int sommaConPuntatori(int* arr, int dim) {
    int somma = 0;
    int* p = arr;
    int* fine = arr + dim;
    
    while (p < fine) {
        somma += *p;
        p++;
    }
    
    return somma;
}

int* trovaMax(int* arr, int dim) {
    int* maxPtr = arr;
    int* p = arr + 1;
    int* fine = arr + dim;
    
    while (p < fine) {
        if (*p > *maxPtr) {
            maxPtr = p;
        }
        p++;
    }
    
    return maxPtr;
}
```

---

### Esercizio 4: Manipolazione Stringhe con Puntatori

```c
#include <stdio.h>

// Prototipi
int lunghezzaStringa(char* str);
void copiaStringa(char* dest, char* src);
void concatenaStringa(char* dest, char* src);
int confrontaStringhe(char* str1, char* str2);
void invertiStringa(char* str);
int contaVocali(char* str);
void maiuscolo(char* str);
void minuscolo(char* str);

int main() {
    char str1[100] = "Ciao";
    char str2[100] = "Mondo";
    char str3[100];
    
    printf("=== MANIPOLAZIONE STRINGHE ===\n\n");
    
    // Lunghezza
    printf("Lunghezza di '%s': %d\n", str1, lunghezzaStringa(str1));
    printf("Lunghezza di '%s': %d\n\n", str2, lunghezzaStringa(str2));
    
    // Copia
    copiaStringa(str3, str1);
    printf("Copia di '%s': '%s'\n\n", str1, str3);
    
    // Concatenazione
    char str4[100] = "Ciao ";
    concatenaStringa(str4, "Mondo!");
    printf("Concatenazione: '%s'\n\n", str4);
    
    // Confronto
    printf("Confronto '%s' e '%s': %d\n", str1, str2, confrontaStringhe(str1, str2));
    printf("Confronto '%s' e '%s': %d\n\n", str1, str1, confrontaStringhe(str1, str1));
    
    // Inversione
    char str5[100] = "Programmazione";
    printf("Prima dell'inversione: '%s'\n", str5);
    invertiStringa(str5);
    printf("Dopo l'inversione: '%s'\n\n", str5);
    
    // Conta vocali
    char str6[] = "Linguaggio C";
    printf("Vocali in '%s': %d\n\n", str6, contaVocali(str6));
    
    // Maiuscolo/Minuscolo
    char str7[100] = "Hello World";
    printf("Originale: '%s'\n", str7);
    maiuscolo(str7);
    printf("Maiuscolo: '%s'\n", str7);
    minuscolo(str7);
    printf("Minuscolo: '%s'\n", str7);
    
    return 0;
}

int lunghezzaStringa(char* str) {
    int len = 0;
    while (*str != '\0') {
        len++;
        str++;
    }
    return len;
}

void copiaStringa(char* dest, char* src) {
    while (*src != '\0') {
        *dest = *src;
        dest++;
        src++;
    }
    *dest = '\0';
}

void concatenaStringa(char* dest, char* src) {
    // Sposta dest alla fine della stringa
    while (*dest != '\0') {
        dest++;
    }
    
    // Copia src in dest
    while (*src != '\0') {
        *dest = *src;
        dest++;
        src++;
    }
    *dest = '\0';
}

int confrontaStringhe(char* str1, char* str2) {
    while (*str1 != '\0' && *str2 != '\0') {
        if (*str1 != *str2) {
            return *str1 - *str2;
        }
        str1++;
        str2++;
    }
    return *str1 - *str2;
}

void invertiStringa(char* str) {
    char* inizio = str;
    char* fine = str;
    
    // Trova la fine della stringa
    while (*fine != '\0') {
        fine++;
    }
    fine--;  // Torna all'ultimo carattere
    
    // Scambia caratteri
    while (inizio < fine) {
        char temp = *inizio;
        *inizio = *fine;
        *fine = temp;
        inizio++;
        fine--;
    }
}

int contaVocali(char* str) {
    int contatore = 0;
    while (*str != '\0') {
        char c = *str;
        if (c == 'a' || c == 'e' || c == 'i' || c == 'o' || c == 'u' ||
            c == 'A' || c == 'E' || c == 'I' || c == 'O' || c == 'U') {
            contatore++;
        }
        str++;
    }
    return contatore;
}

void maiuscolo(char* str) {
    while (*str != '\0') {
        if (*str >= 'a' && *str <= 'z') {
            *str = *str - 32;  // Converti in maiuscolo
        }
        str++;
    }
}

void minuscolo(char* str) {
    while (*str != '\0') {
        if (*str >= 'A' && *str <= 'Z') {
            *str = *str + 32;  // Converti in minuscolo
        }
        str++;
    }
}
```

---

### Esercizio 5: Array di Puntatori

```c
#include <stdio.h>
#include <string.h>

void stampaMenu(char* opzioni[], int n);
void ordinaStringhe(char* arr[], int n);
void cercaStringa(char* arr[], int n, char* target);

int main() {
    printf("=== ARRAY DI PUNTATORI ===\n\n");
    
    // Array di stringhe (array di puntatori)
    char* giorni[] = {
        "Lunedì",
        "Martedì",
        "Mercoledì",
        "Giovedì",
        "Venerdì",
        "Sabato",
        "Domenica"
    };
    
    printf("Giorni della settimana:\n");
    for (int i = 0; i < 7; i++) {
        printf("%d. %s\n", i + 1, giorni[i]);
    }
    printf("\n");
    
    // Menu con array di puntatori
    char* menuOpzioni[] = {
        "Nuovo file",
        "Apri file",
        "Salva",
        "Salva con nome",
        "Esci"
    };
    
    stampaMenu(menuOpzioni, 5);
    printf("\n");
    
    // Ordinamento
    char* nomi[] = {"Mario", "Anna", "Luca", "Elena", "Carlo"};
    int n = 5;
    
    printf("Nomi prima dell'ordinamento:\n");
    for (int i = 0; i < n; i++) {
        printf("%s ", nomi[i]);
    }
    printf("\n");
    
    ordinaStringhe(nomi, n);
    
    printf("Nomi dopo l'ordinamento:\n");
    for (int i = 0; i < n; i++) {
        printf("%s ", nomi[i]);
    }
    printf("\n\n");
    
    // Ricerca
    cercaStringa(nomi, n, "Luca");
    cercaStringa(nomi, n, "Paolo");
    
    return 0;
}

void stampaMenu(char* opzioni[], int n) {
    printf("=== MENU ===\n");
    for (int i = 0; i < n; i++) {
        printf("%d. %s\n", i + 1, opzioni[i]);
    }
}

void ordinaStringhe(char* arr[], int n) {
    // Bubble sort per array di stringhe
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (strcmp(arr[j], arr[j + 1]) > 0) {
                // Scambia i puntatori
                char* temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

void cercaStringa(char* arr[], int n, char* target) {
    int trovato = 0;
    for (int i = 0; i < n; i++) {
        if (strcmp(arr[i], target) == 0) {
            printf("'%s' trovato in posizione %d\n", target, i);
            trovato = 1;
            break;
        }
    }
    if (!trovato) {
        printf("'%s' non trovato\n", target);
    }
}
```

---

### Esercizi da svolgere autonomamente:

1. **Funzione filtra**: Crea una funzione che filtra un array copiando solo gli elementi pari in un nuovo array usando puntatori

2. **Merge di array**: Unisci due array ordinati in un terzo array ordinato usando solo puntatori

3. **Palindromo con puntatori**: Verifica se una stringa è palindroma usando due puntatori (uno all'inizio, uno alla fine)

4. **Rotazione array**: Ruota un array di N posizioni a sinistra/destra usando puntatori

5. **Rimozione duplicati**: Rimuovi elementi duplicati da un array usando puntatori

6. **Matrice trasposta**: Traspongi una matrice usando puntatori (puntatore a puntatore)

7. **Tokenizzazione**: Dividi una stringa in parole separate usando puntatori (simile a strtok)

8. **Ricerca binaria**: Implementa la ricerca binaria usando puntatori invece di indici

9. **Lista dinamica**: Simula una lista dinamica con array e puntatori (aggiungi, rimuovi, cerca)

10. **Caesar cipher**: Implementa il cifrario di Cesare modificando la stringa attraverso puntatori

---

## Approfondimento: Puntatori a Puntatori

Un **puntatore a puntatore** è una variabile che contiene l'indirizzo di un puntatore.

```c
int x = 42;
int* p = &x;      // p punta a x
int** pp = &p;    // pp punta a p (che punta a x)

printf("Valore di x: %d\n", x);        // 42
printf("Valore di x via p: %d\n", *p);      // 42
printf("Valore di x via pp: %d\n", **pp);   // 42

// Modifica attraverso puntatore a puntatore
**pp = 100;
printf("Nuovo valore di x: %d\n", x);  // 100
```

**Rappresentazione:**
```
    x           p           pp
┌────────┐  ┌────────┐  ┌────────┐
│   42   │◄─│  0x..  │◄─│  0x..  │
└────────┘  └────────┘  └────────┘
  0x1000      0x1004      0x1008

pp → p → x
**pp = x
*pp = p
pp = &p
```

**Uso pratico: Array di stringhe**
```c
char* frutti[] = {"Mela", "Pera", "Banana"};
// frutti è un array di puntatori
// frutti è anche un puntatore a puntatore!

char** p = frutti;
printf("%s\n", *p);      // "Mela"
printf("%s\n", *(p+1));  // "Pera"
printf("%s\n", *(p+2));  // "Banana"
```

---

## Errori Comuni con i Puntatori

### 1. Puntatore non inizializzato
```c
int* p;
*p = 42;  // CRASH! p contiene un indirizzo casuale
```

### 2. Dereferenziare NULL
```c
int* p = NULL;
*p = 42;  // CRASH! Segmentation fault
```

### 3. Puntatore pendente (dangling pointer)
```c
int* funzione() {
    int x = 10;
    return &x;  // ERRORE! x viene distrutto
}

int* p = funzione();
*p = 20;  // COMPORTAMENTO INDEFINITO!
```

### 4. Memory leak (con allocazione dinamica)
```c
int* p = malloc(sizeof(int) * 100);
p = NULL;  // Memoria persa! Non si può più liberare
```

### 5. Confondere valore e indirizzo
```c
int x = 10;
int* p;
p = x;   // ERRORE! p deve contenere un indirizzo
// Corretto: p = &x;
```

### 6. Dimenticare le parentesi
```c
int* p;
*p++;   // Prima dereferenzia, poi incrementa p
(*p)++; // Incrementa il valore puntato (corretto!)
```

---

## Best Practices con i Puntatori

✓ **Inizializza sempre i puntatori**
```c
int* p = NULL;  // Buona pratica
```

✓ **Controlla NULL prima di dereferenziare**
```c
if (p != NULL) {
    *p = 10;
}
```

✓ **Usa const per puntatori a dati read-only**
```c
void stampaArray(const int* arr, int dim);
```

✓ **Evita aritmetica complessa dei puntatori**
```c
// Confuso:
*(p++ + 2) = 10;

// Più chiaro:
p[3] = 10;
```

✓ **Documenta chi è responsabile della memoria**
```c
// Chi deve liberare la memoria?
// Specificalo nei commenti!
```

---

## Riepilogo Concetti Chiave

✓ I **puntatori** contengono indirizzi di memoria  
✓ Operatore **&** restituisce l'indirizzo, **\*** dereferenzia  
✓ **Aritmetica dei puntatori**: p+1 salta sizeof(tipo) byte  
✓ **Array e puntatori** sono strettamente collegati  
✓ **Passaggio per riferimento** permette di modificare variabili  
✓ Le **stringhe** sono puntatori a char  
✓ Sempre **inizializzare** i puntatori e controllare NULL  

**Prossima lezione**: Approfondiremo il **progetto finale** e le tecniche di debugging e testing!