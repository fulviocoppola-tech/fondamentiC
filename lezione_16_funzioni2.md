# LEZIONE 16 - Funzioni: Parte 2

## PARTE TEORICA (2h)

### 1. Funzioni Ricorsive

Una **funzione ricorsiva** è una funzione che chiama se stessa. La ricorsione è una tecnica potente per risolvere problemi che possono essere scomposti in sottoproblemi simili ma più piccoli.

**Componenti essenziali della ricorsione:**
1. **Caso base**: condizione che termina la ricorsione
2. **Caso ricorsivo**: chiamata alla funzione stessa con parametri "più piccoli"

**Struttura tipica:**
```c
tipo funzioneRicorsiva(parametri) {
    if (caso_base) {
        return valore_base;  // STOP ricorsione
    }
    // Caso ricorsivo
    return funzioneRicorsiva(parametri_ridotti);
}
```

**Esempio classico: Fattoriale**
```
Matematicamente:
n! = n × (n-1)!
5! = 5 × 4! = 5 × 4 × 3! = 5 × 4 × 3 × 2 × 1 = 120

Caso base: 0! = 1, 1! = 1
```

```c
int fattoriale(int n) {
    // Caso base
    if (n == 0 || n == 1) {
        return 1;
    }
    // Caso ricorsivo
    return n * fattoriale(n - 1);
}

// Chiamata: fattoriale(5)
// 5 * fattoriale(4)
//     4 * fattoriale(3)
//         3 * fattoriale(2)
//             2 * fattoriale(1)
//                 return 1
//             return 2 * 1 = 2
//         return 3 * 2 = 6
//     return 4 * 6 = 24
// return 5 * 24 = 120
```

**Visualizzazione dello stack:**
```
Chiamata: fattoriale(5)

Stack:
┌────────────────┐
│ fattoriale(5)  │ → 5 * fattoriale(4)
├────────────────┤
│ fattoriale(4)  │ → 4 * fattoriale(3)
├────────────────┤
│ fattoriale(3)  │ → 3 * fattoriale(2)
├────────────────┤
│ fattoriale(2)  │ → 2 * fattoriale(1)
├────────────────┤
│ fattoriale(1)  │ → return 1 (caso base!)
└────────────────┘

Risalita dello stack (ritorno):
fattoriale(1) ritorna 1
fattoriale(2) ritorna 2 * 1 = 2
fattoriale(3) ritorna 3 * 2 = 6
fattoriale(4) ritorna 4 * 6 = 24
fattoriale(5) ritorna 5 * 24 = 120
```

**Esempio 2: Sequenza di Fibonacci**
```
Matematicamente:
fib(0) = 0
fib(1) = 1
fib(n) = fib(n-1) + fib(n-2)

Sequenza: 0, 1, 1, 2, 3, 5, 8, 13, 21...
```

```c
int fibonacci(int n) {
    // Casi base
    if (n == 0) return 0;
    if (n == 1) return 1;
    
    // Caso ricorsivo
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// fibonacci(5) = fibonacci(4) + fibonacci(3)
//              = [fib(3) + fib(2)] + [fib(2) + fib(1)]
//              = ...
//              = 5
```

**Esempio 3: Somma dei primi N numeri**
```c
int somma(int n) {
    if (n == 0) return 0;  // Caso base
    return n + somma(n - 1);  // Caso ricorsivo
}

// somma(5) = 5 + somma(4)
//          = 5 + 4 + somma(3)
//          = 5 + 4 + 3 + somma(2)
//          = 5 + 4 + 3 + 2 + somma(1)
//          = 5 + 4 + 3 + 2 + 1 + somma(0)
//          = 5 + 4 + 3 + 2 + 1 + 0
//          = 15
```

**ATTENZIONE: Ricorsione infinita!**
```c
// ERRORE: Manca il caso base!
int sbagliata(int n) {
    return n + sbagliata(n - 1);  // Non si ferma mai!
}

// ERRORE: Caso base irraggiungibile
int sbagliata2(int n) {
    if (n == 0) return 0;
    return n + sbagliata2(n + 1);  // n aumenta invece di diminuire!
}
```

**Ricorsione vs Iterazione:**

```c
// RICORSIVO
int fattorialeRic(int n) {
    if (n <= 1) return 1;
    return n * fattorialeRic(n - 1);
}

// ITERATIVO (con ciclo)
int fattorialeIter(int n) {
    int risultato = 1;
    for (int i = 2; i <= n; i++) {
        risultato *= i;
    }
    return risultato;
}

// Entrambi producono lo stesso risultato!
// Ma hanno caratteristiche diverse:
// - Ricorsivo: più elegante ma usa più memoria (stack)
// - Iterativo: più efficiente ma a volte meno chiaro
```

**Quando usare la ricorsione:**
- Problema naturalmente ricorsivo (alberi, grafi)
- Codice più chiaro e conciso
- Non ci sono problemi di performance critiche

**Quando evitare la ricorsione:**
- Problemi di memoria (stack overflow)
- Performance critiche
- Soluzioni iterative più semplici

---

### 2. Prototipazione delle Funzioni

La **prototipazione** (o dichiarazione forward) permette di dichiarare le funzioni prima di definirle.

**Perché è utile:**
- Organizzare il codice (main() in alto)
- Funzioni che si chiamano a vicenda
- Separare interfaccia da implementazione
- File header (.h) e file sorgente (.c)

**Sintassi del prototipo:**
```c
tipo nomeFunzione(tipo1 param1, tipo2 param2, ...);
```

**Esempio base:**
```c
#include <stdio.h>

// PROTOTIPI (dichiarazioni)
int somma(int a, int b);
int prodotto(int a, int b);
void stampaRisultato(int valore);

// MAIN (può stare in alto!)
int main() {
    int x = 10, y = 5;
    
    int s = somma(x, y);
    stampaRisultato(s);
    
    int p = prodotto(x, y);
    stampaRisultato(p);
    
    return 0;
}

// DEFINIZIONI (implementazioni)
int somma(int a, int b) {
    return a + b;
}

int prodotto(int a, int b) {
    return a * b;
}

void stampaRisultato(int valore) {
    printf("Risultato: %d\n", valore);
}
```

**Nomi dei parametri opzionali nel prototipo:**
```c
// Tutti validi:
int somma(int a, int b);           // Con nomi
int somma(int x, int y);           // Nomi diversi (OK!)
int somma(int, int);               // Senza nomi (OK!)

// La definizione può usare nomi qualsiasi:
int somma(int primo, int secondo) {
    return primo + secondo;
}
```

**Funzioni che si chiamano a vicenda (ricorsione mutua):**
```c
// PROTOTIPI NECESSARI!
int funzioneA(int n);
int funzioneB(int n);

int funzioneA(int n) {
    if (n <= 0) return 0;
    return n + funzioneB(n - 1);
}

int funzioneB(int n) {
    if (n <= 0) return 0;
    return n + funzioneA(n - 1);
}

// Senza prototipi: ERRORE! funzioneB non è ancora dichiarata quando
// viene usata in funzioneA
```

---

### 3. Funzioni void

Le funzioni **void** non restituiscono alcun valore. Sono utilizzate per eseguire azioni senza produrre un risultato.

**Caratteristiche:**
- Tipo di ritorno: `void`
- Non hanno `return` (o `return;` senza valore)
- Utili per: stampa, modifica parametri (con puntatori), operazioni su array

**Esempi:**
```c
// Stampa un messaggio
void saluta() {
    printf("Ciao!\n");
}

// Stampa con parametro
void salutaPersona(char nome[]) {
    printf("Ciao, %s!\n", nome);
}

// Operazioni senza ritorno
void disegnaLinea(int lunghezza) {
    for (int i = 0; i < lunghezza; i++) {
        printf("-");
    }
    printf("\n");
}

// Return anticipato (senza valore)
void stampaPositivo(int n) {
    if (n < 0) {
        printf("Numero negativo, non stampo!\n");
        return;  // Esce subito
    }
    printf("Numero: %d\n", n);
}

// Modifica array (gli array vengono modificati!)
void raddoppiaArray(int arr[], int dim) {
    for (int i = 0; i < dim; i++) {
        arr[i] *= 2;
    }
    // Non serve return
}

int main() {
    saluta();                    // Ciao!
    salutaPersona("Mario");      // Ciao, Mario!
    disegnaLinea(20);            // --------------------
    stampaPositivo(10);          // Numero: 10
    stampaPositivo(-5);          // Numero negativo...
    
    int numeri[] = {1, 2, 3, 4, 5};
    raddoppiaArray(numeri, 5);
    // numeri è ora {2, 4, 6, 8, 10}
    
    return 0;
}
```

**Errori comuni:**
```c
// ERRORE: tentativo di ritornare un valore da void
void sbagliata() {
    return 42;  // ERRORE!
}

// ERRORE: tentativo di usare il "risultato" di void
void funzioneVoid() {
    printf("Ciao\n");
}

int main() {
    int x = funzioneVoid();  // ERRORE! void non ritorna niente
    return 0;
}

// CORRETTO: chiamata senza assegnamento
int main() {
    funzioneVoid();  // OK
    return 0;
}
```

---

### 4. Variabili Locali, Globali e Static

**1. Variabili locali (automatic):**
```c
void funzione() {
    int locale = 10;  // Creata quando si entra nella funzione
    printf("%d\n", locale);
}  // Distrutta quando si esce dalla funzione

int main() {
    funzione();  // locale = 10
    funzione();  // locale = 10 (ricreata da zero!)
    return 0;
}
```

**2. Variabili globali:**
```c
int globale = 100;  // Visibile da tutte le funzioni

void incrementa() {
    globale++;  // Modifica la variabile globale
}

void stampa() {
    printf("globale = %d\n", globale);
}

int main() {
    stampa();      // 100
    incrementa();
    stampa();      // 101
    incrementa();
    stampa();      // 102
    return 0;
}
```

**Svantaggi delle variabili globali:**
- Difficile tracciare chi le modifica
- Possibili conflitti di nomi
- Riducono la modularità
- Complicano il debugging

**3. Variabili static:**

Le variabili **static locali** mantengono il loro valore tra chiamate successive.

```c
void contatore() {
    static int count = 0;  // Inizializzata UNA SOLA VOLTA
    count++;
    printf("Chiamata numero: %d\n", count);
}

int main() {
    contatore();  // Chiamata numero: 1
    contatore();  // Chiamata numero: 2
    contatore();  // Chiamata numero: 3
    contatore();  // Chiamata numero: 4
    return 0;
}
```

**Differenza tra locale normale e static:**
```c
void normale() {
    int x = 0;  // Ricreata ogni volta
    x++;
    printf("normale: x = %d\n", x);  // Sempre 1!
}

void statica() {
    static int x = 0;  // Inizializzata una volta sola
    x++;
    printf("statica: x = %d\n", x);  // Incrementa!
}

int main() {
    normale();  // x = 1
    normale();  // x = 1
    normale();  // x = 1
    
    statica();  // x = 1
    statica();  // x = 2
    statica();  // x = 3
    
    return 0;
}
```

**Utilizzi di static:**
- Contatori di chiamate
- Cache di risultati
- Generatori di numeri
- Stato interno di funzioni

**Esempio pratico: Generatore di ID:**
```c
int generaID() {
    static int ultimoID = 0;
    ultimoID++;
    return ultimoID;
}

int main() {
    printf("ID 1: %d\n", generaID());  // 1
    printf("ID 2: %d\n", generaID());  // 2
    printf("ID 3: %d\n", generaID());  // 3
    return 0;
}
```

---

### 5. Best Practices nella Scrittura di Funzioni

**1. Una funzione, un compito (Single Responsibility):**
```c
// MALE: fa troppe cose
void gestisciDati(int arr[], int dim) {
    // Legge dati
    // Ordina dati
    // Stampa dati
    // Calcola statistiche
}

// BENE: funzioni separate
void leggiDati(int arr[], int* dim);
void ordinaDati(int arr[], int dim);
void stampaDati(int arr[], int dim);
void calcolaStatistiche(int arr[], int dim);
```

**2. Nomi descrittivi:**
```c
// MALE: nomi oscuri
int f(int x, int y) { return x + y; }
void p(int a[]) { /* ... */ }

// BENE: nomi chiari
int calcolaSomma(int a, int b) { return a + b; }
void stampaArray(int array[]) { /* ... */ }
```

**3. Limita il numero di parametri (max 3-4):**
```c
// MALE: troppi parametri
void funzione(int a, int b, int c, int d, int e, int f);

// BENE: usa strutture o riduci parametri
struct Punto {
    int x, y;
};
void funzione(struct Punto p1, struct Punto p2);
```

**4. Validazione input:**
```c
int divide(int a, int b) {
    if (b == 0) {
        printf("Errore: divisione per zero!\n");
        return 0;  // o gestisci l'errore in altro modo
    }
    return a / b;
}

float calcolaMedia(int arr[], int dim) {
    if (dim <= 0 || arr == NULL) {
        return 0.0;  // Input non valido
    }
    // ... calcolo
}
```

**5. Documentazione con commenti:**
```c
/**
 * Calcola il fattoriale di un numero
 * @param n: numero intero non negativo
 * @return: fattoriale di n, o -1 se n < 0
 */
int fattoriale(int n) {
    if (n < 0) return -1;  // Errore
    if (n <= 1) return 1;
    return n * fattoriale(n - 1);
}
```

**6. Evita side effects nascosti:**
```c
// MALE: modifica variabile globale senza preavviso
int globale = 0;
int somma(int a, int b) {
    globale++;  // Side effect nascosto!
    return a + b;
}

// BENE: effetti evidenti nei parametri o documentati
void incrementaContatore(int* contatore) {
    (*contatore)++;  // Chiaro che modifica il parametro
}
```

**7. Keep It Simple (KISS):**
```c
// MALE: complicato inutilmente
int isEven(int n) {
    if (n % 2 == 0) {
        return 1;
    } else {
        return 0;
    }
}

// BENE: semplice
int isEven(int n) {
    return n % 2 == 0;
}
```

**8. DRY (Don't Repeat Yourself):**
```c
// MALE: codice duplicato
void operazione1() {
    printf("=== INIZIO ===\n");
    // logica...
    printf("=== FINE ===\n");
}

void operazione2() {
    printf("=== INIZIO ===\n");
    // altra logica...
    printf("=== FINE ===\n");
}

// BENE: estrai la logica comune
void stampaIntestazione() {
    printf("=== INIZIO ===\n");
}

void stampaPiede() {
    printf("=== FINE ===\n");
}

void operazione1() {
    stampaIntestazione();
    // logica...
    stampaPiede();
}
```

---

## PARTE PRATICA (2h)

### Esercizio 1: Fattoriale e Fibonacci Ricorsivi

```c
#include <stdio.h>

// Prototipi
int fattorialeRicorsivo(int n);
int fattorialeIterativo(int n);
int fibonacciRicorsivo(int n);
int fibonacciIterativo(int n);
void stampaSeparatore();

int main() {
    printf("=== FATTORIALE ===\n");
    
    for (int i = 0; i <= 10; i++) {
        printf("%d! = %d (ricorsivo) = %d (iterativo)\n", 
               i, fattorialeRicorsivo(i), fattorialeIterativo(i));
    }
    
    stampaSeparatore();
    printf("=== FIBONACCI ===\n");
    
    printf("Primi 15 numeri di Fibonacci:\n");
    for (int i = 0; i < 15; i++) {
        printf("fib(%d) = %d\n", i, fibonacciRicorsivo(i));
    }
    
    stampaSeparatore();
    printf("=== CONFRONTO PERFORMANCE ===\n");
    
    int n = 40;
    printf("\nCalcolo fib(%d)...\n", n);
    printf("Ricorsivo: %d\n", fibonacciRicorsivo(n));  // Lento!
    printf("Iterativo: %d\n", fibonacciIterativo(n));  // Veloce!
    
    return 0;
}

// Fattoriale ricorsivo
int fattorialeRicorsivo(int n) {
    if (n < 0) return -1;  // Errore
    if (n <= 1) return 1;  // Caso base
    return n * fattorialeRicorsivo(n - 1);  // Caso ricorsivo
}

// Fattoriale iterativo
int fattorialeIterativo(int n) {
    if (n < 0) return -1;
    int risultato = 1;
    for (int i = 2; i <= n; i++) {
        risultato *= i;
    }
    return risultato;
}

// Fibonacci ricorsivo (inefficiente per n grandi!)
int fibonacciRicorsivo(int n) {
    if (n == 0) return 0;  // Caso base
    if (n == 1) return 1;  // Caso base
    return fibonacciRicorsivo(n - 1) + fibonacciRicorsivo(n - 2);
}

// Fibonacci iterativo (efficiente)
int fibonacciIterativo(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    
    int a = 0, b = 1, temp;
    for (int i = 2; i <= n; i++) {
        temp = a + b;
        a = b;
        b = temp;
    }
    return b;
}

void stampaSeparatore() {
    printf("\n");
    for (int i = 0; i < 50; i++) printf("=");
    printf("\n\n");
}
```

---

### Esercizio 2: Torre di Hanoi (Ricorsione Avanzata)

```c
#include <stdio.h>

// Contatore statico di mosse
static int numeroMosse = 0;

// Prototipi
void hanoi(int n, char origine, char destinazione, char ausiliario);
void stampaIntestazione(int n);
void stampaRisultato(int n);

int main() {
    int n;
    
    printf("=== TORRE DI HANOI ===\n");
    printf("Inserisci il numero di dischi: ");
    scanf("%d", &n);
    
    if (n <= 0) {
        printf("Numero non valido!\n");
        return 1;
    }
    
    stampaIntestazione(n);
    
    numeroMosse = 0;
    hanoi(n, 'A', 'C', 'B');
    
    stampaRisultato(n);
    
    return 0;
}

/**
 * Risolve il problema della Torre di Hanoi
 * @param n: numero di dischi
 * @param origine: piolo di partenza
 * @param destinazione: piolo di arrivo
 * @param ausiliario: piolo di supporto
 */
void hanoi(int n, char origine, char destinazione, char ausiliario) {
    if (n == 1) {
        // Caso base: sposta un solo disco
        numeroMosse++;
        printf("Mossa %d: Sposta disco da %c a %c\n", 
               numeroMosse, origine, destinazione);
        return;
    }
    
    // Caso ricorsivo:
    // 1. Sposta n-1 dischi da origine ad ausiliario (usando destinazione)
    hanoi(n - 1, origine, ausiliario, destinazione);
    
    // 2. Sposta il disco più grande da origine a destinazione
    numeroMosse++;
    printf("Mossa %d: Sposta disco da %c a %c\n", 
           numeroMosse, origine, destinazione);
    
    // 3. Sposta n-1 dischi da ausiliario a destinazione (usando origine)
    hanoi(n - 1, ausiliario, destinazione, origine);
}

void stampaIntestazione(int n) {
    printf("\nRisoluzione con %d dischi:\n", n);
    printf("Piolo A (origine) → Piolo C (destinazione)\n");
    printf("Piolo B (ausiliario)\n\n");
}

void stampaRisultato(int n) {
    printf("\nTotale mosse: %d\n", numeroMosse);
    printf("Formula: 2^%d - 1 = %d\n", n, (1 << n) - 1);
}
```

---

### Esercizio 3: Funzioni per Array con Static

```c
#include <stdio.h>

#define MAX 100

// Prototipi
void aggiungiElemento(int arr[], int* dim, int elemento);
int rimuoviElemento(int arr[], int* dim, int indice);
void stampaArray(int arr[], int dim);
void stampaStatistiche();
int contaChiamate();

int main() {
    int array[MAX];
    int dimensione = 0;
    
    printf("=== GESTIONE ARRAY ===\n\n");
    
    // Aggiungi elementi
    aggiungiElemento(array, &dimensione, 10);
    aggiungiElemento(array, &dimensione, 20);
    aggiungiElemento(array, &dimensione, 30);
    aggiungiElemento(array, &dimensione, 40);
    aggiungiElemento(array, &dimensione, 50);
    
    stampaArray(array, dimensione);
    stampaStatistiche();
    
    // Rimuovi elemento
    printf("\nRimuovo elemento in posizione 2...\n");
    rimuoviElemento(array, &dimensione, 2);
    stampaArray(array, dimensione);
    
    // Aggiungi altri elementi
    aggiungiElemento(array, &dimensione, 60);
    aggiungiElemento(array, &dimensione, 70);
    
    stampaArray(array, dimensione);
    stampaStatistiche();
    
    printf("\nNumero totale di chiamate a funzioni: %d\n", contaChiamate());
    
    return 0;
}

// Aggiunge un elemento all'array
void aggiungiElemento(int arr[], int* dim, int elemento) {
    static int aggiunte = 0;  // Conta le aggiunte
    aggiunte++;
    
    if (*dim >= MAX) {
        printf("Errore: array pieno!\n");
        return;
    }
    
    arr[*dim] = elemento;
    (*dim)++;
    
    printf("Elemento %d aggiunto (totale aggiunte: %d)\n", elemento, aggiunte);
}

// Rimuove un elemento dall'array
int rimuoviElemento(int arr[], int* dim, int indice) {
    static int rimozioni = 0;  // Conta le rimozioni
    rimozioni++;
    
    if (indice < 0 || indice >= *dim) {
        printf("Errore: indice non valido!\n");
        return 0;
    }
    
    int elementoRimosso = arr[indice];
    
    // Shift elementi a sinistra
    for (int i = indice; i < *dim - 1; i++) {
        arr[i] = arr[i + 1];
    }
    
    (*dim)--;
    
    printf("Elemento %d rimosso (totale rimozioni: %d)\n", 
           elementoRimosso, rimozioni);
    
    return elementoRimosso;
}

// Stampa l'array
void stampaArray(int arr[], int dim) {
    printf("Array (%d elementi): [ ", dim);
    for (int i = 0; i < dim; i++) {
        printf("%d ", arr[i]);
    }
    printf("]\n");
}

// Stampa statistiche usando variabili static
void stampaStatistiche() {
    static int chiamate = 0;
    static int totaleElementi = 0;
    
    chiamate++;
    
    printf("\n--- Statistiche (chiamata #%d) ---\n", chiamate);
    printf("Questa funzione è stata chiamata %d volte\n", chiamate);
}

// Conta tutte le chiamate a funzioni
int contaChiamate() {
    static int totale = 0;
    totale++;
    return totale;
}
```

---

### Esercizio 4: Funzioni per Array (Refactoring)

```c
#include <stdio.h>

#define MAX 50

// Prototipi
void menu();
void eseguiScelta(int scelta, int arr[], int* dim);
void inputArray(int arr[], int* dim);
void stampaArray(int arr[], int dim);
void cercaElemento(int arr[], int dim);
void ordinaArray(int arr[], int dim);
void invertiArray(int arr[], int dim);
void calcolaStatistiche(int arr[], int dim);

int main() {
    int array[MAX];
    int dimensione = 0;
    int scelta;
    
    do {
        menu();
        printf("Scelta: ");
        scanf("%d", &scelta);
        printf("\n");
        
        eseguiScelta(scelta, array, &dimensione);
        
        if (scelta != 0) {
            printf("\nPremi INVIO per continuare...");
            getchar();  // Pulisci buffer
            getchar();  // Aspetta INVIO
        }
        
    } while (scelta != 0);
    
    printf("Arrivederci!\n");
    return 0;
}

void menu() {
    printf("\n");
    printf("=====================================\n");
    printf("         GESTIONE ARRAY\n");
    printf("=====================================\n");
    printf("1. Inserisci array\n");
    printf("2. Stampa array\n");
    printf("3. Cerca elemento\n");
    printf("4. Ordina array\n");
    printf("5. Inverti array\n");
    printf("6. Calcola statistiche\n");
    printf("0. Esci\n");
    printf("=====================================\n");
}

void eseguiScelta(int scelta, int arr[], int* dim) {
    switch (scelta) {
        case 1:
            inputArray(arr, dim);
            break;
        case 2:
            stampaArray(arr, *dim);
            break;
        case 3:
            cercaElemento(arr, *dim);
            break;
        case 4:
            ordinaArray(arr, *dim);
            break;
        case 5:
            invertiArray(arr, *dim);
            break;
        case 6:
            calcolaStatistiche(arr, *dim);
            break;
        case 0:
            break;
        default:
            printf("Scelta non valida!\n");
    }
}

void inputArray(int arr[], int* dim) {
    printf("Quanti elementi vuoi inserire (max %d)? ", MAX);
    scanf("%d", dim);
    
    if (*dim < 1 || *dim > MAX) {
        printf("Numero non valido!\n");
        *dim = 0;
        return;
    }
    
    printf("Inserisci %d elementi:\n", *dim);
    for (int i = 0; i < *dim; i++) {
        printf("Elemento %d: ", i + 1);
        scanf("%d", &arr[i]);
    }
    
    printf("Array inserito con successo!\n");
}

void stampaArray(int arr[], int dim) {
    if (dim == 0) {
        printf("Array vuoto!\n");
        return;
    }
    
    printf("Array (%d elementi):\n", dim);
    printf("[ ");
    for (int i = 0; i < dim; i++) {
        printf("%d ", arr[i]);
    }
    printf("]\n");
}

void cercaElemento(int arr[], int dim) {
    if (dim == 0) {
        printf("Array vuoto!\n");
        return;
    }
    
    int target;
    printf("Inserisci l'elemento da cercare: ");
    scanf("%d", &target);
    
    int trovato = 0;
    printf("Posizioni trovate:\n");
    for (int i = 0; i < dim; i++) {
        if (arr[i] == target) {
            printf("  Indice %d\n", i);
            trovato = 1;
        }
    }
    
    if (!trovato) {
        printf("  Elemento non trovato\n");
    }
}

void ordinaArray(int arr[], int dim) {
    if (dim == 0) {
        printf("Array vuoto!\n");
        return;
    }
    
    // Bubble sort
    for (int i = 0; i < dim - 1; i++) {
        for (int j = 0; j < dim - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    
    printf("Array ordinato!\n");
    stampaArray(arr, dim);
}

void invertiArray(int arr[], int dim) {
    if (dim == 0) {
        printf("Array vuoto!\n");
        return;
    }
    
    for (int i = 0; i < dim / 2; i++) {
        int temp = arr[i];
        arr[i] = arr[dim - 1 - i];
        arr[dim - 1 - i] = temp;
    }
    
    printf("Array invertito!\n");
    stampaArray(arr, dim);
}

void calcolaStatistiche(int arr[], int dim) {
    if (dim == 0) {
        printf("Array vuoto!\n");
        return;
    }
    
    int somma = 0;
    int max = arr[0];
    int min = arr[0];
    
    for (int i = 0; i < dim; i++) {
        somma += arr[i];
        if (arr[i] > max) max = arr[i];
        if (arr[i] < min) min = arr[i];
    }
    
    float media = (float)somma / dim;
    
    printf("=== STATISTICHE ===\n");
    printf("Elementi: %d\n", dim);
    printf("Somma: %d\n", somma);
    printf("Media: %.2f\n", media);
    printf("Massimo: %d\n", max);
    printf("Minimo: %d\n", min);
    printf("Range: %d\n", max - min);
}
```

---

### Esercizi da svolgere autonomamente:

1. **Potenza ricorsiva**: Implementa la funzione `pow(base, esp)` in modo ricorsivo

2. **Somma cifre**: Funzione ricorsiva che calcola la somma delle cifre di un numero (es: 123 → 1+2+3 = 6)

3. **MCD ricorsivo**: Implementa l'algoritmo di Euclide per il Massimo Comun Divisore in modo ricorsivo

4. **Palindromo ricorsivo**: Verifica se una stringa è palindroma usando ricorsione

5. **Conversione binario**: Funzione ricorsiva che converte un numero decimale in binario

6. **Ricerca binaria**: Implementa la ricerca binaria in modo ricorsivo

7. **Merge sort**: Implementa l'algoritmo di ordinamento merge sort (ricorsivo)

8. **Permutazioni**: Genera tutte le permutazioni di una stringa in modo ricorsivo

9. **Sottosequenze**: Genera tutte le sottosequenze di un array usando ricorsione

10. **Path finding**: Trova un percorso in una matrice da (0,0) a (n,m) usando ricorsione

---

## Approfondimento: Complessità della Ricorsione

### Fibonacci Ricorsivo - Analisi

Il Fibonacci ricorsivo è **molto inefficiente** perché ricalcola gli stessi valori molte volte:

```
fib(5) = fib(4) + fib(3)
         |         |
    fib(3)+fib(2)  fib(2)+fib(1)
    |      |       |
  fib(2)+fib(1) fib(1)+fib(0) ...
  |
fib(1)+fib(0)

Nota: fib(3) viene calcolato 2 volte
      fib(2) viene calcolato 3 volte
      fib(1) viene calcolato 5 volte!
```

**Complessità temporale**: O(2^n) - ESPONENZIALE!

**Soluzione: Memoization (cache dei risultati)**
```c
#define MAX_N 100
int memo[MAX_N];

void inizializzaMemo() {
    for (int i = 0; i < MAX_N; i++) {
        memo[i] = -1;  // Non calcolato
    }
}

int fibonacciMemo(int n) {
    if (n == 0) return 0;
    if (n == 1) return 1;
    
    // Se già calcolato, ritorna il valore memorizzato
    if (memo[n] != -1) {
        return memo[n];
    }
    
    // Calcola e memorizza
    memo[n] = fibonacciMemo(n - 1) + fibonacciMemo(n - 2);
    return memo[n];
}

// Uso:
int main() {
    inizializzaMemo();
    printf("fib(50) = %d\n", fibonacciMemo(50));  // Velocissimo!
    return 0;
}
```

---

## Riepilogo Concetti Chiave

✓ **Ricorsione**: funzione che chiama se stessa con caso base e caso ricorsivo  
✓ **Prototipazione**: dichiarazione forward per organizzare il codice  
✓ **Funzioni void**: eseguono azioni senza ritornare valori  
✓ **Variabili static**: mantengono il valore tra chiamate successive  
✓ **Best practices**: nomi chiari, validazione input, documentazione, KISS, DRY  
✓ Ricorsione vs iterazione: scegli in base al problema  

**Prossima lezione**: Approfondiremo i **puntatori**, l'aritmetica dei puntatori e il passaggio per riferimento!