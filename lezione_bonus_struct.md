# LEZIONE BONUS - Struct, Liste e Alberi

## PARTE TEORICA (2h)

### 1. Introduzione alle Strutture (Struct)

Una **struct** (struttura) è un tipo di dato composto che permette di raggruppare variabili di tipi diversi sotto un unico nome.

**Perché usare le struct?**
- Organizzare dati correlati
- Creare tipi di dato personalizzati
- Rappresentare entità del mondo reale
- Base per strutture dati complesse

**Analogia della vita reale:**
```
Una "Persona" ha:
├── Nome (stringa)
├── Età (intero)
├── Altezza (float)
└── Email (stringa)

Invece di 4 variabili separate, uso 1 struct!
```

---

### 2. Dichiarazione e Definizione di Struct

**Sintassi base:**
```c
struct NomeStruttura {
    tipo1 campo1;
    tipo2 campo2;
    tipo3 campo3;
    // ...
};
```

**Esempio: Punto 2D**
```c
struct Punto {
    int x;
    int y;
};

// Dichiarazione variabile
struct Punto p1;

// Accesso ai campi
p1.x = 10;
p1.y = 20;

printf("Punto: (%d, %d)\n", p1.x, p1.y);
```

**Esempio: Studente**
```c
struct Studente {
    char nome[50];
    char cognome[50];
    int matricola;
    float media;
};

struct Studente s1;

strcpy(s1.nome, "Mario");
strcpy(s1.cognome, "Rossi");
s1.matricola = 12345;
s1.media = 27.5;

printf("Studente: %s %s\n", s1.nome, s1.cognome);
printf("Matricola: %d, Media: %.2f\n", s1.matricola, s1.media);
```

**Inizializzazione:**
```c
// Metodo 1: alla dichiarazione
struct Punto p1 = {10, 20};

// Metodo 2: campo per campo
struct Punto p2;
p2.x = 5;
p2.y = 15;

// Metodo 3: con nomi campi (C99+)
struct Studente s1 = {
    .nome = "Anna",
    .cognome = "Verdi",
    .matricola = 54321,
    .media = 28.5
};
```

---

### 3. Typedef - Semplificare la Sintassi

**typedef** permette di creare alias per i tipi di dato.

```c
// Senza typedef (sintassi verbosa)
struct Punto {
    int x;
    int y;
};
struct Punto p1;  // Devo sempre scrivere "struct"

// Con typedef
typedef struct {
    int x;
    int y;
} Punto;

Punto p1;  // Più semplice!
Punto p2 = {5, 10};
```

**Esempio completo:**
```c
typedef struct {
    char titolo[100];
    char autore[100];
    int anno;
    float prezzo;
} Libro;

Libro lib1 = {"Il Signore degli Anelli", "Tolkien", 1954, 29.90};
Libro lib2;

strcpy(lib2.titolo, "1984");
strcpy(lib2.autore, "Orwell");
lib2.anno = 1949;
lib2.prezzo = 15.50;
```

---

### 4. Array di Struct

```c
typedef struct {
    char nome[50];
    int voto;
} Studente;

Studente classe[30];  // Array di 30 studenti

// Inserimento
for (int i = 0; i < 3; i++) {
    printf("Nome studente %d: ", i+1);
    scanf("%s", classe[i].nome);
    printf("Voto: ");
    scanf("%d", &classe[i].voto);
}

// Accesso
for (int i = 0; i < 3; i++) {
    printf("%s: %d\n", classe[i].nome, classe[i].voto);
}

// Calcolo media
float somma = 0;
for (int i = 0; i < 3; i++) {
    somma += classe[i].voto;
}
float media = somma / 3;
printf("Media classe: %.2f\n", media);
```

---

### 5. Struct Annidate

Le struct possono contenere altre struct.

```c
typedef struct {
    int giorno;
    int mese;
    int anno;
} Data;

typedef struct {
    char via[100];
    char citta[50];
    int cap;
} Indirizzo;

typedef struct {
    char nome[50];
    char cognome[50];
    Data dataNascita;
    Indirizzo indirizzo;
} Persona;

Persona p1;

strcpy(p1.nome, "Luca");
strcpy(p1.cognome, "Bianchi");

p1.dataNascita.giorno = 15;
p1.dataNascita.mese = 3;
p1.dataNascita.anno = 1990;

strcpy(p1.indirizzo.via, "Via Roma 10");
strcpy(p1.indirizzo.citta, "Milano");
p1.indirizzo.cap = 20100;

printf("%s %s\n", p1.nome, p1.cognome);
printf("Nato il: %d/%d/%d\n", 
       p1.dataNascita.giorno, 
       p1.dataNascita.mese, 
       p1.dataNascita.anno);
printf("Indirizzo: %s, %s, %d\n", 
       p1.indirizzo.via, 
       p1.indirizzo.citta, 
       p1.indirizzo.cap);
```

---

### 6. Puntatori a Struct

```c
typedef struct {
    int x;
    int y;
} Punto;

Punto p1 = {10, 20};
Punto* ptr = &p1;

// Accesso con notazione freccia ->
printf("x: %d, y: %d\n", ptr->x, ptr->y);

// Equivalente a:
printf("x: %d, y: %d\n", (*ptr).x, (*ptr).y);

// Modifica attraverso puntatore
ptr->x = 50;
ptr->y = 60;

printf("Nuovo punto: (%d, %d)\n", p1.x, p1.y);  // 50, 60
```

**Passaggio a funzioni:**
```c
// Passaggio per valore (copia)
void stampaPerValore(Punto p) {
    printf("(%d, %d)\n", p.x, p.y);
}

// Passaggio per riferimento (efficiente)
void stampaPerRiferimento(Punto* p) {
    printf("(%d, %d)\n", p->x, p->y);
}

// Modifica attraverso puntatore
void sposta(Punto* p, int dx, int dy) {
    p->x += dx;
    p->y += dy;
}

int main() {
    Punto p = {10, 20};
    
    stampaPerValore(p);          // Copia
    stampaPerRiferimento(&p);    // Riferimento
    
    sposta(&p, 5, -10);
    printf("Dopo spostamento: (%d, %d)\n", p.x, p.y);  // 15, 10
    
    return 0;
}
```

---

### 7. Liste Linkate (Linked Lists)

Una **lista linkata** è una struttura dati dinamica formata da nodi collegati tramite puntatori.

**Struttura di un nodo:**
```
┌──────────┬──────────┐
│   Dato   │ Puntatore│──→ Prossimo nodo
└──────────┴──────────┘
```

**Implementazione:**
```c
typedef struct Nodo {
    int dato;
    struct Nodo* next;  // Puntatore al prossimo nodo
} Nodo;
```

**Visualizzazione lista:**
```
HEAD
  ↓
┌───┬───┐    ┌───┬───┐    ┌───┬───┐
│ 5 │ ●─┼───→│ 10│ ●─┼───→│ 15│NULL│
└───┴───┘    └───┴───┘    └───┴───┘
```

**Operazioni fondamentali:**

**1. Creazione nodo:**
```c
Nodo* creaNodo(int valore) {
    Nodo* nuovoNodo = (Nodo*)malloc(sizeof(Nodo));
    if (nuovoNodo == NULL) {
        printf("Errore allocazione memoria!\n");
        exit(1);
    }
    nuovoNodo->dato = valore;
    nuovoNodo->next = NULL;
    return nuovoNodo;
}
```

**2. Inserimento in testa:**
```c
void inserisciInTesta(Nodo** head, int valore) {
    Nodo* nuovoNodo = creaNodo(valore);
    nuovoNodo->next = *head;
    *head = nuovoNodo;
}

// Uso:
Nodo* lista = NULL;
inserisciInTesta(&lista, 10);
inserisciInTesta(&lista, 20);
inserisciInTesta(&lista, 30);
// Lista: 30 -> 20 -> 10 -> NULL
```

**3. Inserimento in coda:**
```c
void inserisciInCoda(Nodo** head, int valore) {
    Nodo* nuovoNodo = creaNodo(valore);
    
    if (*head == NULL) {
        *head = nuovoNodo;
        return;
    }
    
    Nodo* temp = *head;
    while (temp->next != NULL) {
        temp = temp->next;
    }
    temp->next = nuovoNodo;
}
```

**4. Stampa lista:**
```c
void stampaLista(Nodo* head) {
    Nodo* temp = head;
    printf("Lista: ");
    while (temp != NULL) {
        printf("%d -> ", temp->dato);
        temp = temp->next;
    }
    printf("NULL\n");
}
```

**5. Ricerca:**
```c
int cerca(Nodo* head, int valore) {
    Nodo* temp = head;
    int posizione = 0;
    
    while (temp != NULL) {
        if (temp->dato == valore) {
            return posizione;
        }
        temp = temp->next;
        posizione++;
    }
    return -1;  // Non trovato
}
```

**6. Cancellazione nodo:**
```c
void cancellaNodo(Nodo** head, int valore) {
    if (*head == NULL) return;
    
    // Se il nodo da cancellare è la testa
    if ((*head)->dato == valore) {
        Nodo* temp = *head;
        *head = (*head)->next;
        free(temp);
        return;
    }
    
    // Cerca il nodo da cancellare
    Nodo* temp = *head;
    while (temp->next != NULL && temp->next->dato != valore) {
        temp = temp->next;
    }
    
    // Se trovato, cancella
    if (temp->next != NULL) {
        Nodo* daEliminare = temp->next;
        temp->next = temp->next->next;
        free(daEliminare);
    }
}
```

**7. Liberare memoria:**
```c
void liberaLista(Nodo** head) {
    Nodo* temp;
    while (*head != NULL) {
        temp = *head;
        *head = (*head)->next;
        free(temp);
    }
}
```

---

### 8. Alberi Binari

Un **albero binario** è una struttura gerarchica dove ogni nodo ha al massimo due figli.

**Struttura:**
```
        10
       /  \
      5    15
     / \   / \
    3   7 12  20
```

**Implementazione nodo:**
```c
typedef struct NodoAlbero {
    int dato;
    struct NodoAlbero* sinistro;
    struct NodoAlbero* destro;
} NodoAlbero;
```

**Creazione nodo:**
```c
NodoAlbero* creaNodoAlbero(int valore) {
    NodoAlbero* nuovoNodo = (NodoAlbero*)malloc(sizeof(NodoAlbero));
    if (nuovoNodo == NULL) {
        printf("Errore allocazione memoria!\n");
        exit(1);
    }
    nuovoNodo->dato = valore;
    nuovoNodo->sinistro = NULL;
    nuovoNodo->destro = NULL;
    return nuovoNodo;
}
```

**Inserimento in BST (Binary Search Tree):**
```c
NodoAlbero* inserisci(NodoAlbero* radice, int valore) {
    if (radice == NULL) {
        return creaNodoAlbero(valore);
    }
    
    if (valore < radice->dato) {
        radice->sinistro = inserisci(radice->sinistro, valore);
    } else if (valore > radice->dato) {
        radice->destro = inserisci(radice->destro, valore);
    }
    
    return radice;
}
```

**Visite dell'albero:**

**1. Visita in ordine (In-Order):**
```c
void visitaInOrdine(NodoAlbero* radice) {
    if (radice == NULL) return;
    
    visitaInOrdine(radice->sinistro);
    printf("%d ", radice->dato);
    visitaInOrdine(radice->destro);
}
// Output per l'albero sopra: 3 5 7 10 12 15 20 (ordinato!)
```

**2. Visita pre-ordine (Pre-Order):**
```c
void visitaPreOrdine(NodoAlbero* radice) {
    if (radice == NULL) return;
    
    printf("%d ", radice->dato);
    visitaPreOrdine(radice->sinistro);
    visitaPreOrdine(radice->destro);
}
// Output: 10 5 3 7 15 12 20
```

**3. Visita post-ordine (Post-Order):**
```c
void visitaPostOrdine(NodoAlbero* radice) {
    if (radice == NULL) return;
    
    visitaPostOrdine(radice->sinistro);
    visitaPostOrdine(radice->destro);
    printf("%d ", radice->dato);
}
// Output: 3 7 5 12 20 15 10
```

**Ricerca in BST:**
```c
NodoAlbero* cerca(NodoAlbero* radice, int valore) {
    if (radice == NULL || radice->dato == valore) {
        return radice;
    }
    
    if (valore < radice->dato) {
        return cerca(radice->sinistro, valore);
    } else {
        return cerca(radice->destro, valore);
    }
}
```

**Altezza albero:**
```c
int altezza(NodoAlbero* radice) {
    if (radice == NULL) return 0;
    
    int altSin = altezza(radice->sinistro);
    int altDes = altezza(radice->destro);
    
    return 1 + (altSin > altDes ? altSin : altDes);
}
```

**Contare nodi:**
```c
int contaNodi(NodoAlbero* radice) {
    if (radice == NULL) return 0;
    return 1 + contaNodi(radice->sinistro) + contaNodi(radice->destro);
}
```

---

## PARTE PRATICA (2h)

### Esercizio 1: Sistema Gestione Studenti con Struct

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define MAX_STUDENTI 100
#define MAX_NOME 50

typedef struct {
    int giorno;
    int mese;
    int anno;
} Data;

typedef struct {
    char nome[MAX_NOME];
    char cognome[MAX_NOME];
    int matricola;
    Data dataNascita;
    float voti[10];
    int numVoti;
} Studente;

// Prototipi
void aggiungiStudente(Studente studenti[], int* n);
void visualizzaStudenti(Studente studenti[], int n);
void aggiungiVoto(Studente studenti[], int n);
void calcolaStatistiche(Studente studenti[], int n);
float calcolaMedia(Studente s);
void ordinaPerMedia(Studente studenti[], int n);

int main() {
    Studente studenti[MAX_STUDENTI];
    int numStudenti = 0;
    int scelta;
    
    do {
        printf("\n╔════════════════════════════════╗\n");
        printf("║   GESTIONE STUDENTI            ║\n");
        printf("╠════════════════════════════════╣\n");
        printf("║ 1. Aggiungi studente           ║\n");
        printf("║ 2. Visualizza studenti         ║\n");
        printf("║ 3. Aggiungi voto               ║\n");
        printf("║ 4. Statistiche                 ║\n");
        printf("║ 5. Ordina per media            ║\n");
        printf("║ 0. Esci                        ║\n");
        printf("╚════════════════════════════════╝\n");
        printf("Scelta: ");
        scanf("%d", &scelta);
        while (getchar() != '\n');
        
        switch(scelta) {
            case 1:
                aggiungiStudente(studenti, &numStudenti);
                break;
            case 2:
                visualizzaStudenti(studenti, numStudenti);
                break;
            case 3:
                aggiungiVoto(studenti, numStudenti);
                break;
            case 4:
                calcolaStatistiche(studenti, numStudenti);
                break;
            case 5:
                ordinaPerMedia(studenti, numStudenti);
                printf("Studenti ordinati per media!\n");
                break;
            case 0:
                printf("Arrivederci!\n");
                break;
            default:
                printf("Scelta non valida!\n");
        }
        
        if (scelta != 0) {
            printf("\nPremi INVIO...");
            getchar();
        }
        
    } while (scelta != 0);
    
    return 0;
}

void aggiungiStudente(Studente studenti[], int* n) {
    if (*n >= MAX_STUDENTI) {
        printf("Errore: numero massimo studenti raggiunto!\n");
        return;
    }
    
    Studente nuovo;
    
    printf("\n=== NUOVO STUDENTE ===\n");
    printf("Nome: ");
    fgets(nuovo.nome, MAX_NOME, stdin);
    nuovo.nome[strcspn(nuovo.nome, "\n")] = '\0';
    
    printf("Cognome: ");
    fgets(nuovo.cognome, MAX_NOME, stdin);
    nuovo.cognome[strcspn(nuovo.cognome, "\n")] = '\0';
    
    printf("Matricola: ");
    scanf("%d", &nuovo.matricola);
    
    printf("Data di nascita (gg mm aaaa): ");
    scanf("%d %d %d", &nuovo.dataNascita.giorno, 
          &nuovo.dataNascita.mese, &nuovo.dataNascita.anno);
    while (getchar() != '\n');
    
    nuovo.numVoti = 0;
    
    studenti[*n] = nuovo;
    (*n)++;
    
    printf("\nStudente aggiunto con successo!\n");
}

void visualizzaStudenti(Studente studenti[], int n) {
    if (n == 0) {
        printf("\nNessuno studente presente.\n");
        return;
    }
    
    printf("\n╔════════════════════════════════════════════════════════════╗\n");
    printf("║                    ELENCO STUDENTI                         ║\n");
    printf("╚════════════════════════════════════════════════════════════╝\n\n");
    
    for (int i = 0; i < n; i++) {
        printf("Matricola: %d\n", studenti[i].matricola);
        printf("Nome: %s %s\n", studenti[i].nome, studenti[i].cognome);
        printf("Data di nascita: %02d/%02d/%d\n", 
               studenti[i].dataNascita.giorno,
               studenti[i].dataNascita.mese,
               studenti[i].dataNascita.anno);
        
        if (studenti[i].numVoti > 0) {
            printf("Voti: ");
            for (int j = 0; j < studenti[i].numVoti; j++) {
                printf("%.1f ", studenti[i].voti[j]);
            }
            printf("\nMedia: %.2f\n", calcolaMedia(studenti[i]));
        } else {
            printf("Nessun voto registrato\n");
        }
        printf("─────────────────────────────────────────────────────────\n");
    }
}

void aggiungiVoto(Studente studenti[], int n) {
    if (n == 0) {
        printf("\nNessuno studente presente.\n");
        return;
    }
    
    int matricola;
    printf("Matricola studente: ");
    scanf("%d", &matricola);
    
    int trovato = -1;
    for (int i = 0; i < n; i++) {
        if (studenti[i].matricola == matricola) {
            trovato = i;
            break;
        }
    }
    
    if (trovato == -1) {
        printf("Studente non trovato!\n");
        return;
    }
    
    if (studenti[trovato].numVoti >= 10) {
        printf("Errore: massimo 10 voti per studente!\n");
        return;
    }
    
    float voto;
    printf("Voto (18-30): ");
    scanf("%f", &voto);
    
    if (voto < 18 || voto > 30) {
        printf("Voto non valido!\n");
        return;
    }
    
    studenti[trovato].voti[studenti[trovato].numVoti] = voto;
    studenti[trovato].numVoti++;
    
    printf("Voto aggiunto con successo!\n");
}

float calcolaMedia(Studente s) {
    if (s.numVoti == 0) return 0.0;
    
    float somma = 0;
    for (int i = 0; i < s.numVoti; i++) {
        somma += s.voti[i];
    }
    return somma / s.numVoti;
}

void calcolaStatistiche(Studente studenti[], int n) {
    if (n == 0) {
        printf("\nNessuno studente presente.\n");
        return;
    }
    
    printf("\n=== STATISTICHE ===\n");
    printf("Numero studenti: %d\n\n", n);
    
    float sommaMediaGenerale = 0;
    int studentiConVoti = 0;
    float mediaMax = 0;
    float mediaMin = 30;
    int indiceMigliore = -1;
    int indicePeggiore = -1;
    
    for (int i = 0; i < n; i++) {
        if (studenti[i].numVoti > 0) {
            float media = calcolaMedia(studenti[i]);
            sommaMediaGenerale += media;
            studentiConVoti++;
            
            if (media > mediaMax) {
                mediaMax = media;
                indiceMigliore = i;
            }
            if (media < mediaMin) {
                mediaMin = media;
                indicePeggiore = i;
            }
        }
    }
    
    if (studentiConVoti > 0) {
        printf("Media generale: %.2f\n", sommaMediaGenerale / studentiConVoti);
        printf("\nMigliore studente: %s %s (Media: %.2f)\n",
               studenti[indiceMigliore].nome,
               studenti[indiceMigliore].cognome,
               mediaMax);
        printf("Studente con media più bassa: %s %s (Media: %.2f)\n",
               studenti[indicePeggiore].nome,
               studenti[indicePeggiore].cognome,
               mediaMin);
    } else {
        printf("Nessuno studente con voti registrati.\n");
    }
}

void ordinaPerMedia(Studente studenti[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            float media1 = calcolaMedia(studenti[j]);
            float media2 = calcolaMedia(studenti[j + 1]);
            
            if (media1 < media2) {
                Studente temp = studenti[j];
                studenti[j] = studenti[j + 1];
                studenti[j + 1] = temp;
            }
        }
    }
}
```

---

### Esercizio 2: Lista Linkata - Gestione Todo List

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_TASK 100

typedef struct Nodo {
    int id;
    char descrizione[MAX_TASK];
    int completato;
    struct Nodo* next;
} Nodo;

// Prototipi
Nodo* creaNodo(int id, const char* descrizione);
void inserisciTask(Nodo** head, const char* descrizione);
void visualizzaTasks(Nodo* head);
void completaTask(Nodo* head, int id);
void eliminaTask(Nodo** head, int id);
void liberaLista(Nodo** head);
int contaTasks(Nodo* head);

int idGlobale = 1;

int main() {
    Nodo* todoList = NULL;
    int scelta;
    
    do {
        printf("\n╔════════════════════════════════╗\n");
        printf("║        TODO LIST               ║\n");
        printf("╠════════════════════════════════╣\n");
        printf("║ 1. Aggiungi task               ║\n");
        printf("║ 2. Visualizza tasks            ║\n");
        printf("║ 3. Completa task               ║\n");
        printf("║ 4. Elimina task                ║\n");
        printf("║ 0. Esci                        ║\n");
        printf("╚════════════════════════════════╝\n");
        printf("Tasks attivi: %d\n", contaTasks(todoList));
        printf("Scelta: ");
        scanf("%d", &scelta);
        while (getchar() != '\n');
        
        switch(scelta) {
            case 1: {
                char desc[MAX_TASK];
                printf("Descrizione task: ");
                fgets(desc, MAX_TASK, stdin);
                desc[strcspn(desc, "\n")] = '\0';
                inserisciTask(&todoList, desc);
                break;
            }
            case 2:
                visualizzaTasks(todoList);
                break;
            case 3: {
                int id;
                printf("ID task da completare: ");
                scanf("%d", &id);
                while (getchar() != '\n');
                completaTask(todoList, id);
                break;
            }
            case 4: {
                int id;
                printf("ID task da eliminare: ");
                scanf("%d", &id);
                while (getchar() != '\n');
                eliminaTask(&todoList, id);
                break;
            }
            case 0:
                printf("Arrivederci!\n");
                break;
            default:
                printf("Scelta non valida!\n");
        }
        
        if (scelta != 0) {
            printf("\nPremi INVIO...");
            getchar();
        }
        
    } while (scelta != 0);
    
    liberaLista(&todoList);
    return 0;
}

Nodo* creaNodo(int id, const char* descrizione) {
    Nodo* nuovoNodo = (Nodo*)malloc(sizeof(Nodo));
    if (nuovoNodo == NULL) {
        printf("Errore allocazione memoria!\n");
        exit(1);
    }
    nuovoNodo->id = id;
    strcpy(nuovoNodo->descrizione, descrizione);
    nuovoNodo->completato = 0;
    nuovoNodo->next = NULL;
    return nuovoNodo;
}

void inserisciTask(Nodo** head, const char* descrizione) {
    Nodo* nuovoNodo = creaNodo(idGlobale++, descrizione);
    
    if (*head == NULL) {
        *head = nuovoNodo;
    } else {
        Nodo* temp = *head;
        while (temp->next != NULL) {
            temp = temp->next;
        }
        temp->next = nuovoNodo;
    }
    
    printf("Task aggiunto con ID: %d\n", nuovoNodo->id);
}

void visualizzaTasks(Nodo* head) {
    if (head == NULL) {
        printf("\nNessun task presente.\n");
        return;
    }
    
    printf("\n╔════════════════════════════════════════════════════════╗\n");
    printf("║                    TASKS                               ║\n");
    printf("╚════════════════════════════════════════════════════════╝\n\n");
    
    Nodo* temp = head;
    while (temp != NULL) {
        printf("[%c] ID: %d - %s\n", 
               temp->completato ? 'X' : ' ',
               temp->id,
               temp->descrizione);
        temp = temp->next;
    }
}

void completaTask(Nodo* head, int id) {
    Nodo* temp = head;
    
    while (temp != NULL) {
        if (temp->id == id) {
            temp->completato = 1;
            printf("Task %d completato!\n", id);
            return;
        }
        temp = temp->next;
    }
    
    printf("Task con ID %d non trovato!\n", id);
}

void eliminaTask(Nodo** head, int id) {
    if (*head == NULL) {
        printf("Lista vuota!\n");
        return;
    }
    
    // Se il primo nodo è quello da eliminare
    if ((*head)->id == id) {
        Nodo* temp = *head;
        *head = (*head)->next;
        free(temp);
        printf("Task %d eliminato!\n", id);
        return;
    }
    
    // Cerca il nodo da eliminare
    Nodo* temp = *head;
    while (temp->next != NULL && temp->next->id != id) {
        temp = temp->next;
    }
    
    if (temp->next != NULL) {
        Nodo* daEliminare = temp->next;
        temp->next = temp->next->next;
        free(daEliminare);
        printf("Task %d eliminato!\n", id);
    } else {
        printf("Task con ID %d non trovato!\n", id);
    }
}

void liberaLista(Nodo** head) {
    Nodo* temp;
    while (*head != NULL) {
        temp = *head;
        *head = (*head)->next;
        free(temp);
    }
}

int contaTasks(Nodo* head) {
    int count = 0;
    Nodo* temp = head;
    while (temp != NULL) {
        if (!temp->completato) count++;
        temp = temp->next;
    }
    return count;
}
```

---

### Esercizio 3: Albero Binario di Ricerca (BST)

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct NodoAlbero {
    int valore;
    struct NodoAlbero* sinistro;
    struct NodoAlbero* destro;
} NodoAlbero;

// Prototipi
NodoAlbero* creaNodo(int valore);
NodoAlbero* inserisci(NodoAlbero* radice, int valore);
NodoAlbero* cerca(NodoAlbero* radice, int valore);
void visitaInOrdine(NodoAlbero* radice);
void visitaPreOrdine(NodoAlbero* radice);
void visitaPostOrdine(NodoAlbero* radice);
int altezza(NodoAlbero* radice);
int contaNodi(NodoAlbero* radice);
int trovaMinimo(NodoAlbero* radice);
int trovaMassimo(NodoAlbero* radice);
NodoAlbero* cancella(NodoAlbero* radice, int valore);
void stampaAlbero(NodoAlbero* radice, int spazio);
void liberaAlbero(NodoAlbero* radice);

int main() {
    NodoAlbero* radice = NULL;
    int scelta, valore;
    
    do {
        printf("\n╔════════════════════════════════════╗\n");
        printf("║    ALBERO BINARIO DI RICERCA       ║\n");
        printf("╠════════════════════════════════════╣\n");
        printf("║ 1. Inserisci valore                ║\n");
        printf("║ 2. Cerca valore                    ║\n");
        printf("║ 3. Cancella valore                 ║\n");
        printf("║ 4. Visita in ordine                ║\n");
        printf("║ 5. Visita pre-ordine               ║\n");
        printf("║ 6. Visita post-ordine              ║\n");
        printf("║ 7. Visualizza albero               ║\n");
        printf("║ 8. Statistiche                     ║\n");
        printf("║ 0. Esci                            ║\n");
        printf("╚════════════════════════════════════╝\n");
        printf("Scelta: ");
        scanf("%d", &scelta);
        
        switch(scelta) {
            case 1:
                printf("Valore da inserire: ");
                scanf("%d", &valore);
                radice = inserisci(radice, valore);
                printf("Valore %d inserito!\n", valore);
                break;
                
            case 2:
                printf("Valore da cercare: ");
                scanf("%d", &valore);
                if (cerca(radice, valore) != NULL) {
                    printf("Valore %d trovato nell'albero!\n", valore);
                } else {
                    printf("Valore %d non presente nell'albero.\n", valore);
                }
                break;
                
            case 3:
                printf("Valore da cancellare: ");
                scanf("%d", &valore);
                radice = cancella(radice, valore);
                printf("Valore %d cancellato!\n", valore);
                break;
                
            case 4:
                printf("Visita in ordine: ");
                visitaInOrdine(radice);
                printf("\n");
                break;
                
            case 5:
                printf("Visita pre-ordine: ");
                visitaPreOrdine(radice);
                printf("\n");
                break;
                
            case 6:
                printf("Visita post-ordine: ");
                visitaPostOrdine(radice);
                printf("\n");
                break;
                
            case 7:
                printf("\nStruttura albero:\n");
                stampaAlbero(radice, 0);
                break;
                
            case 8:
                if (radice == NULL) {
                    printf("Albero vuoto!\n");
                } else {
                    printf("\n=== STATISTICHE ===\n");
                    printf("Numero nodi: %d\n", contaNodi(radice));
                    printf("Altezza: %d\n", altezza(radice));
                    printf("Valore minimo: %d\n", trovaMinimo(radice));
                    printf("Valore massimo: %d\n", trovaMassimo(radice));
                }
                break;
                
            case 0:
                printf("Arrivederci!\n");
                break;
                
            default:
                printf("Scelta non valida!\n");
        }
        
        if (scelta != 0) {
            printf("\nPremi INVIO...");
            while (getchar() != '\n');
            getchar();
        }
        
    } while (scelta != 0);
    
    liberaAlbero(radice);
    return 0;
}

NodoAlbero* creaNodo(int valore) {
    NodoAlbero* nuovoNodo = (NodoAlbero*)malloc(sizeof(NodoAlbero));
    if (nuovoNodo == NULL) {
        printf("Errore allocazione memoria!\n");
        exit(1);
    }
    nuovoNodo->valore = valore;
    nuovoNodo->sinistro = NULL;
    nuovoNodo->destro = NULL;
    return nuovoNodo;
}

NodoAlbero* inserisci(NodoAlbero* radice, int valore) {
    if (radice == NULL) {
        return creaNodo(valore);
    }
    
    if (valore < radice->valore) {
        radice->sinistro = inserisci(radice->sinistro, valore);
    } else if (valore > radice->valore) {
        radice->destro = inserisci(radice->destro, valore);
    }
    // Se valore == radice->valore, non inserire duplicati
    
    return radice;
}

NodoAlbero* cerca(NodoAlbero* radice, int valore) {
    if (radice == NULL || radice->valore == valore) {
        return radice;
    }
    
    if (valore < radice->valore) {
        return cerca(radice->sinistro, valore);
    } else {
        return cerca(radice->destro, valore);
    }
}

void visitaInOrdine(NodoAlbero* radice) {
    if (radice == NULL) return;
    
    visitaInOrdine(radice->sinistro);
    printf("%d ", radice->valore);
    visitaInOrdine(radice->destro);
}

void visitaPreOrdine(NodoAlbero* radice) {
    if (radice == NULL) return;
    
    printf("%d ", radice->valore);
    visitaPreOrdine(radice->sinistro);
    visitaPreOrdine(radice->destro);
}

void visitaPostOrdine(NodoAlbero* radice) {
    if (radice == NULL) return;
    
    visitaPostOrdine(radice->sinistro);
    visitaPostOrdine(radice->destro);
    printf("%d ", radice->valore);
}

int altezza(NodoAlbero* radice) {
    if (radice == NULL) return 0;
    
    int altSin = altezza(radice->sinistro);
    int altDes = altezza(radice->destro);
    
    return 1 + (altSin > altDes ? altSin : altDes);
}

int contaNodi(NodoAlbero* radice) {
    if (radice == NULL) return 0;
    return 1 + contaNodi(radice->sinistro) + contaNodi(radice->destro);
}

int trovaMinimo(NodoAlbero* radice) {
    if (radice == NULL) {
        printf("Albero vuoto!\n");
        return -1;
    }
    
    while (radice->sinistro != NULL) {
        radice = radice->sinistro;
    }
    return radice->valore;
}

int trovaMassimo(NodoAlbero* radice) {
    if (radice == NULL) {
        printf("Albero vuoto!\n");
        return -1;
    }
    
    while (radice->destro != NULL) {
        radice = radice->destro;
    }
    return radice->valore;
}

NodoAlbero* trovaMinimoNodo(NodoAlbero* nodo) {
    NodoAlbero* corrente = nodo;
    while (corrente && corrente->sinistro != NULL) {
        corrente = corrente->sinistro;
    }
    return corrente;
}

NodoAlbero* cancella(NodoAlbero* radice, int valore) {
    if (radice == NULL) return radice;
    
    if (valore < radice->valore) {
        radice->sinistro = cancella(radice->sinistro, valore);
    } else if (valore > radice->valore) {
        radice->destro = cancella(radice->destro, valore);
    } else {
        // Nodo con 0 o 1 figlio
        if (radice->sinistro == NULL) {
            NodoAlbero* temp = radice->destro;
            free(radice);
            return temp;
        } else if (radice->destro == NULL) {
            NodoAlbero* temp = radice->sinistro;
            free(radice);
            return temp;
        }
        
        // Nodo con 2 figli: trova il successore in-order
        NodoAlbero* temp = trovaMinimoNodo(radice->destro);
        radice->valore = temp->valore;
        radice->destro = cancella(radice->destro, temp->valore);
    }
    return radice;
}

void stampaAlbero(NodoAlbero* radice, int spazio) {
    if (radice == NULL) return;
    
    spazio += 5;
    
    stampaAlbero(radice->destro, spazio);
    
    printf("\n");
    for (int i = 5; i < spazio; i++) {
        printf(" ");
    }
    printf("%d\n", radice->valore);
    
    stampaAlbero(radice->sinistro, spazio);
}

void liberaAlbero(NodoAlbero* radice) {
    if (radice == NULL) return;
    
    liberaAlbero(radice->sinistro);
    liberaAlbero(radice->destro);
    free(radice);
}
```

---

### Esercizio 4: Rubrica con Lista Linkata

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_NOME 50
#define MAX_TELEFONO 20
#define MAX_EMAIL 50

typedef struct Contatto {
    char nome[MAX_NOME];
    char cognome[MAX_NOME];
    char telefono[MAX_TELEFONO];
    char email[MAX_EMAIL];
    struct Contatto* next;
} Contatto;

// Prototipi
void aggiungiContatto(Contatto** head);
void visualizzaContatti(Contatto* head);
void cercaContatto(Contatto* head);
void eliminaContatto(Contatto** head);
void modificaContatto(Contatto* head);
void ordinaContatti(Contatto** head);
void salvasuFile(Contatto* head);
void caricaDaFile(Contatto** head);
void liberaRubrica(Contatto** head);

int main() {
    Contatto* rubrica = NULL;
    int scelta;
    
    // Carica rubrica da file
    caricaDaFile(&rubrica);
    
    do {
        printf("\n╔════════════════════════════════════╗\n");
        printf("║          RUBRICA TELEFONICA        ║\n");
        printf("╠════════════════════════════════════╣\n");
        printf("║ 1. Aggiungi contatto               ║\n");
        printf("║ 2. Visualizza tutti i contatti     ║\n");
        printf("║ 3. Cerca contatto                  ║\n");
        printf("║ 4. Modifica contatto               ║\n");
        printf("║ 5. Elimina contatto                ║\n");
        printf("║ 6. Ordina per cognome              ║\n");
        printf("║ 7. Salva su file                   ║\n");
        printf("║ 0. Esci                            ║\n");
        printf("╚════════════════════════════════════╝\n");
        printf("Scelta: ");
        scanf("%d", &scelta);
        while (getchar() != '\n');
        
        switch(scelta) {
            case 1:
                aggiungiContatto(&rubrica);
                break;
            case 2:
                visualizzaContatti(rubrica);
                break;
            case 3:
                cercaContatto(rubrica);
                break;
            case 4:
                modificaContatto(rubrica);
                break;
            case 5:
                eliminaContatto(&rubrica);
                break;
            case 6:
                ordinaContatti(&rubrica);
                printf("Contatti ordinati!\n");
                break;
            case 7:
                salvasuFile(rubrica);
                printf("Rubrica salvata!\n");
                break;
            case 0:
                salvasuFile(rubrica);
                printf("Arrivederci!\n");
                break;
            default:
                printf("Scelta non valida!\n");
        }
        
        if (scelta != 0) {
            printf("\nPremi INVIO...");
            getchar();
        }
        
    } while (scelta != 0);
    
    liberaRubrica(&rubrica);
    return 0;
}

void aggiungiContatto(Contatto** head) {
    Contatto* nuovo = (Contatto*)malloc(sizeof(Contatto));
    if (nuovo == NULL) {
        printf("Errore allocazione memoria!\n");
        return;
    }
    
    printf("\n=== NUOVO CONTATTO ===\n");
    printf("Nome: ");
    fgets(nuovo->nome, MAX_NOME, stdin);
    nuovo->nome[strcspn(nuovo->nome, "\n")] = '\0';
    
    printf("Cognome: ");
    fgets(nuovo->cognome, MAX_NOME, stdin);
    nuovo->cognome[strcspn(nuovo->cognome, "\n")] = '\0';
    
    printf("Telefono: ");
    fgets(nuovo->telefono, MAX_TELEFONO, stdin);
    nuovo->telefono[strcspn(nuovo->telefono, "\n")] = '\0';
    
    printf("Email: ");
    fgets(nuovo->email, MAX_EMAIL, stdin);
    nuovo->email[strcspn(nuovo->email, "\n")] = '\0';
    
    nuovo->next = NULL;
    
    // Inserisci in coda
    if (*head == NULL) {
        *head = nuovo;
    } else {
        Contatto* temp = *head;
        while (temp->next != NULL) {
            temp = temp->next;
        }
        temp->next = nuovo;
    }
    
    printf("\nContatto aggiunto con successo!\n");
}

void visualizzaContatti(Contatto* head) {
    if (head == NULL) {
        printf("\nRubrica vuota.\n");
        return;
    }
    
    printf("\n╔════════════════════════════════════════════════════════════╗\n");
    printf("║                    RUBRICA CONTATTI                        ║\n");
    printf("╚════════════════════════════════════════════════════════════╝\n\n");
    
    Contatto* temp = head;
    int i = 1;
    
    while (temp != NULL) {
        printf("Contatto #%d\n", i);
        printf("Nome: %s %s\n", temp->nome, temp->cognome);
        printf("Telefono: %s\n", temp->telefono);
        printf("Email: %s\n", temp->email);
        printf("─────────────────────────────────────────────────────────\n");
        temp = temp->next;
        i++;
    }
}

void cercaContatto(Contatto* head) {
    if (head == NULL) {
        printf("\nRubrica vuota.\n");
        return;
    }
    
    char ricerca[MAX_NOME];
    printf("Cerca per nome o cognome: ");
    fgets(ricerca, MAX_NOME, stdin);
    ricerca[strcspn(ricerca, "\n")] = '\0';
    
    printf("\n=== RISULTATI RICERCA ===\n");
    
    Contatto* temp = head;
    int trovati = 0;
    
    while (temp != NULL) {
        if (strstr(temp->nome, ricerca) != NULL ||
            strstr(temp->cognome, ricerca) != NULL) {
            printf("\nNome: %s %s\n", temp->nome, temp->cognome);
            printf("Telefono: %s\n", temp->telefono);
            printf("Email: %s\n", temp->email);
            trovati++;
        }
        temp = temp->next;
    }
    
    if (trovati == 0) {
        printf("Nessun contatto trovato.\n");
    } else {
        printf("\nTrovati %d contatti.\n", trovati);
    }
}

void modificaContatto(Contatto* head) {
    if (head == NULL) {
        printf("\nRubrica vuota.\n");
        return;
    }
    
    char cognome[MAX_NOME];
    printf("Cognome del contatto da modificare: ");
    fgets(cognome, MAX_NOME, stdin);
    cognome[strcspn(cognome, "\n")] = '\0';
    
    Contatto* temp = head;
    
    while (temp != NULL) {
        if (strcmp(temp->cognome, cognome) == 0) {
            printf("\n=== MODIFICA CONTATTO ===\n");
            printf("Nome attuale: %s\n", temp->nome);
            printf("Nuovo nome (INVIO per mantenere): ");
            char buffer[MAX_NOME];
            fgets(buffer, MAX_NOME, stdin);
            if (buffer[0] != '\n') {
                buffer[strcspn(buffer, "\n")] = '\0';
                strcpy(temp->nome, buffer);
            }
            
            printf("Telefono attuale: %s\n", temp->telefono);
            printf("Nuovo telefono (INVIO per mantenere): ");
            fgets(buffer, MAX_TELEFONO, stdin);
            if (buffer[0] != '\n') {
                buffer[strcspn(buffer, "\n")] = '\0';
                strcpy(temp->telefono, buffer);
            }
            
            printf("Email attuale: %s\n", temp->email);
            printf("Nuova email (INVIO per mantenere): ");
            fgets(buffer, MAX_EMAIL, stdin);
            if (buffer[0] != '\n') {
                buffer[strcspn(buffer, "\n")] = '\0';
                strcpy(temp->email, buffer);
            }
            
            printf("\nContatto modificato!\n");
            return;
        }
        temp = temp->next;
    }
    
    printf("Contatto non trovato.\n");
}

void eliminaContatto(Contatto** head) {
    if (*head == NULL) {
        printf("\nRubrica vuota.\n");
        return;
    }
    
    char cognome[MAX_NOME];
    printf("Cognome del contatto da eliminare: ");
    fgets(cognome, MAX_NOME, stdin);
    cognome[strcspn(cognome, "\n")] = '\0';
    
    // Se è il primo nodo
    if (strcmp((*head)->cognome, cognome) == 0) {
        Contatto* temp = *head;
        *head = (*head)->next;
        free(temp);
        printf("Contatto eliminato!\n");
        return;
    }
    
    Contatto* temp = *head;
    while (temp->next != NULL && strcmp(temp->next->cognome, cognome) != 0) {
        temp = temp->next;
    }
    
    if (temp->next != NULL) {
        Contatto* daEliminare = temp->next;
        temp->next = temp->next->next;
        free(daEliminare);
        printf("Contatto eliminato!\n");
    } else {
        printf("Contatto non trovato.\n");
    }
}

void ordinaContatti(Contatto** head) {
    if (*head == NULL || (*head)->next == NULL) return;
    
    int scambiato;
    Contatto* ptr1;
    Contatto* lptr = NULL;
    
    do {
        scambiato = 0;
        ptr1 = *head;
        
        while (ptr1->next != lptr) {
            if (strcmp(ptr1->cognome, ptr1->next->cognome) > 0) {
                // Scambia i dati
                char tempNome[MAX_NOME], tempCognome[MAX_NOME];
                char tempTelefono[MAX_TELEFONO], tempEmail[MAX_EMAIL];
                
                strcpy(tempNome, ptr1->nome);
                strcpy(tempCognome, ptr1->cognome);
                strcpy(tempTelefono, ptr1->telefono);
                strcpy(tempEmail, ptr1->email);
                
                strcpy(ptr1->nome, ptr1->next->nome);
                strcpy(ptr1->cognome, ptr1->next->cognome);
                strcpy(ptr1->telefono, ptr1->next->telefono);
                strcpy(ptr1->email, ptr1->next->email);
                
                strcpy(ptr1->next->nome, tempNome);
                strcpy(ptr1->next->cognome, tempCognome);
                strcpy(ptr1->next->telefono, tempTelefono);
                strcpy(ptr1->next->email, tempEmail);
                
                scambiato = 1;
            }
            ptr1 = ptr1->next;
        }
        lptr = ptr1;
    } while (scambiato);
}

void salvasuFile(Contatto* head) {
    FILE* file = fopen("rubrica.dat", "wb");
    if (file == NULL) {
        printf("Errore apertura file!\n");
        return;
    }
    
    Contatto* temp = head;
    while (temp != NULL) {
        fwrite(temp, sizeof(Contatto) - sizeof(Contatto*), 1, file);
        temp = temp->next;
    }
    
    fclose(file);
}

void caricaDaFile(Contatto** head) {
    FILE* file = fopen("rubrica.dat", "rb");
    if (file == NULL) {
        return;  // File non esiste
    }
    
    Contatto temp;
    while (fread(&temp, sizeof(Contatto) - sizeof(Contatto*), 1, file) == 1) {
        Contatto* nuovo = (Contatto*)malloc(sizeof(Contatto));
        if (nuovo == NULL) {
            printf("Errore allocazione memoria!\n");
            fclose(file);
            return;
        }
        
        strcpy(nuovo->nome, temp.nome);
        strcpy(nuovo->cognome, temp.cognome);
        strcpy(nuovo->telefono, temp.telefono);
        strcpy(nuovo->email, temp.email);
        nuovo->next = NULL;
        
        if (*head == NULL) {
            *head = nuovo;
        } else {
            Contatto* ptr = *head;
            while (ptr->next != NULL) {
                ptr = ptr->next;
            }
            ptr->next = nuovo;
        }
    }
    
    fclose(file);
    
    // Conta contatti caricati
    int count = 0;
    Contatto* temp2 = *head;
    while (temp2 != NULL) {
        count++;
        temp2 = temp2->next;
    }
    
    if (count > 0) {
        printf("Caricati %d contatti dalla rubrica.\n\n", count);
    }
}

void liberaRubrica(Contatto** head) {
    Contatto* temp;
    while (*head != NULL) {
        temp = *head;
        *head = (*head)->next;
        free(temp);
    }
}
```

---

## Esercizi da svolgere autonomamente:

1. **Biblioteca con struct**: Crea un sistema di gestione biblioteca con struct Libro e array di libri

2. **Lista doppiamente linkata**: Implementa una lista dove ogni nodo ha puntatore a next e prev

3. **Stack con lista**: Implementa uno stack (LIFO) usando una lista linkata

4. **Coda con lista**: Implementa una coda (FIFO) usando una lista linkata

5. **Merge di due liste ordinate**: Unisci due liste linkate ordinate in una sola lista ordinata

6. **Inversione lista**: Inverti una lista linkata (l'ultimo diventa il primo)

7. **Albero bilanciato**: Verifica se un albero binario è bilanciato

8. **Lista circ olare**: Implementa una lista circolare dove l'ultimo nodo punta al primo

9. **Grafo con liste di adiacenza**: Rappresenta un grafo usando liste linkate

10. **Dizionario con hash table**: Implementa un dizionario chiave-valore usando array e liste

---

## Riepilogo Concetti Chiave

✓ **Struct**: raggruppano dati correlati di tipi diversi  
✓ **typedef**: semplifica la sintassi delle struct  
✓ **Puntatori a struct**: accesso con operatore `->`  
✓ **Liste linkate**: struttura dati dinamica con nodi collegati  
✓ **Allocazione dinamica**: `malloc()` per creare nodi  
✓ **Alberi binari**: struttura gerarchica con massimo 2 figli per nodo  
✓ **BST**: albero binario di ricerca (sinistro < radice < destro)  
✓ **Ricorsione**: usata per visite e operazioni sugli alberi  
✓ **Memoria**: sempre liberare con `free()` ciò che è allocato  

**Prossimi argomenti avanzati**: Hash table, grafi, algoritmi di ordinamento avanzati, alberi AVL/Rosso-Nero!