# LEZIONE 18 - Progetto Finale e Ripasso

## PARTE TEORICA (1h)

### 1. Metodologia di Sviluppo Software

Lo sviluppo di un programma non inizia dal codice, ma dalla **pianificazione**.

**Fasi dello sviluppo:**

1. **Analisi dei requisiti**
   - Cosa deve fare il programma?
   - Chi sono gli utenti?
   - Quali sono gli input/output?

2. **Design (Progettazione)**
   - Dividere il problema in sottoproblemi
   - Identificare le funzioni necessarie
   - Progettare le strutture dati
   - Creare flowchart/pseudocodice

3. **Implementazione**
   - Scrivere il codice
   - Seguire buone pratiche di programmazione
   - Commentare il codice

4. **Testing**
   - Test unitari (singole funzioni)
   - Test di integrazione (più funzioni insieme)
   - Test di sistema (programma completo)

5. **Debugging**
   - Identificare errori
   - Correggere bug
   - Verificare le correzioni

6. **Manutenzione**
   - Aggiungere nuove funzionalità
   - Ottimizzare prestazioni
   - Correggere bug trovati dagli utenti

**Approccio Top-Down:**
```
Problema grande
    ├── Sottoproblema 1
    │   ├── Funzione A
    │   └── Funzione B
    ├── Sottoproblema 2
    │   ├── Funzione C
    │   └── Funzione D
    └── Sottoproblema 3
        └── Funzione E
```

**Esempio: Sistema di gestione voti**
```
Sistema Gestione Voti
    ├── Gestione Studenti
    │   ├── aggiungiStudente()
    │   ├── rimuoviStudente()
    │   └── cercaStudente()
    ├── Gestione Voti
    │   ├── inserisciVoto()
    │   ├── modificaVoto()
    │   └── calcolaMedia()
    └── Report
        ├── stampaElenco()
        ├── stampaStatistiche()
        └── esportaCSV()
```

---

### 2. Debugging e Testing

**Debugging** è il processo di identificazione e risoluzione degli errori.

**Tecniche di debugging:**

**1. Print debugging (printf)**
```c
void funzioneComplessa(int x) {
    printf("[DEBUG] Input: x=%d\n", x);  // Traccia input
    
    int risultato = x * 2;
    printf("[DEBUG] risultato=%d\n", risultato);  // Traccia calcolo
    
    if (risultato > 100) {
        printf("[DEBUG] Condizione vera\n");  // Traccia flusso
        // ...
    }
}
```

**2. Assert (verifica assunzioni)**
```c
#include <assert.h>

void divide(int a, int b) {
    assert(b != 0);  // Verifica che b non sia zero
    printf("Risultato: %d\n", a / b);
}

void processaArray(int arr[], int dim) {
    assert(arr != NULL);  // Verifica che arr sia valido
    assert(dim > 0);      // Verifica dimensione positiva
    // ...
}
```

**3. Debugger (GDB)**
```bash
# Compilare con flag di debug
gcc -g programma.c -o programma

# Avviare debugger
gdb ./programma

# Comandi principali:
# break main          - breakpoint su main
# run                 - esegui programma
# step                - esegui una riga
# next                - esegui senza entrare nelle funzioni
# print variabile     - stampa valore variabile
# continue            - continua esecuzione
# quit                - esci
```

**Testing sistematico:**

```c
// Test unitario per funzione somma
void testSomma() {
    // Test caso normale
    assert(somma(2, 3) == 5);
    assert(somma(10, 20) == 30);
    
    // Test casi limite
    assert(somma(0, 0) == 0);
    assert(somma(-5, 5) == 0);
    assert(somma(-10, -20) == -30);
    
    printf("Tutti i test per somma() superati!\n");
}

// Test per array
void testOrdinaArray() {
    int test1[] = {3, 1, 4, 1, 5};
    int atteso1[] = {1, 1, 3, 4, 5};
    
    ordinaArray(test1, 5);
    
    for (int i = 0; i < 5; i++) {
        assert(test1[i] == atteso1[i]);
    }
    
    printf("Test ordinamento superato!\n");
}
```

**Tipi di errori:**

1. **Errori di sintassi** (compile-time)
   - Individuati dal compilatore
   - Es: manca punto e virgola, parentesi non chiusa

2. **Errori di logica** (run-time)
   - Il programma compila ma fa cose sbagliate
   - Es: algoritmo errato, condizione sbagliata

3. **Errori di runtime**
   - Crash durante l'esecuzione
   - Es: divisione per zero, accesso fuori array

---

### 3. Gestione degli Errori

**Codici di errore:**
```c
// Convenzione: 0 = successo, negativo = errore
int divide(int a, int b, int* risultato) {
    if (b == 0) {
        return -1;  // Errore: divisione per zero
    }
    *risultato = a / b;
    return 0;  // Successo
}

int main() {
    int risultato;
    int codice = divide(10, 2, &risultato);
    
    if (codice == 0) {
        printf("Risultato: %d\n", risultato);
    } else {
        printf("Errore: divisione per zero\n");
    }
    
    return 0;
}
```

**Validazione input:**
```c
int leggiInteroValido(int min, int max) {
    int valore;
    int valido = 0;
    
    do {
        printf("Inserisci un numero tra %d e %d: ", min, max);
        
        if (scanf("%d", &valore) != 1) {
            // Input non numerico
            printf("Errore: inserisci un numero!\n");
            while (getchar() != '\n');  // Pulisci buffer
            continue;
        }
        
        if (valore < min || valore > max) {
            printf("Errore: numero fuori range!\n");
        } else {
            valido = 1;
        }
        
    } while (!valido);
    
    return valore;
}
```

**Controllo puntatori:**
```c
void funzioneSicura(int* arr, int dim) {
    if (arr == NULL) {
        fprintf(stderr, "Errore: array NULL\n");
        return;
    }
    
    if (dim <= 0) {
        fprintf(stderr, "Errore: dimensione non valida\n");
        return;
    }
    
    // Procedi con l'elaborazione
    // ...
}
```

---

### 4. Ripasso Generale dei Concetti Principali

**Struttura del programma C:**
```c
// Include librerie
#include <stdio.h>
#include <stdlib.h>

// Costanti
#define MAX 100

// Prototipi
void menu();
void operazioniBase();
void operazioniAvanzate();
void conversioni();
void calcolatoreScientifica();
void risolutoreEquazioni();
void statistiche();
double leggiDouble(const char* messaggio);

int main() {
    int scelta;
    
    printf("╔══════════════════════════════════════╗\n");
    printf("║    CALCOLATRICE AVANZATA             ║\n");
    printf("╚══════════════════════════════════════╝\n");
    
    do {
        menu();
        printf("Scelta: ");
        scanf("%d", &scelta);
        while (getchar() != '\n');
        printf("\n");
        
        switch(scelta) {
            case 1:
                operazioniBase();
                break;
            case 2:
                operazioniAvanzate();
                break;
            case 3:
                conversioni();
                break;
            case 4:
                calcolatoreScientifica();
                break;
            case 5:
                risolutoreEquazioni();
                break;
            case 6:
                statistiche();
                break;
            case 0:
                printf("Arrivederci!\n");
                break;
            default:
                printf("Scelta non valida!\n");
        }
        
        if (scelta != 0) {
            printf("\nPremi INVIO per continuare...");
            getchar();
        }
        
    } while (scelta != 0);
    
    return 0;
}

void menu() {
    printf("\n");
    printf("╔══════════════════════════════════════╗\n");
    printf("║           MENU PRINCIPALE            ║\n");
    printf("╠══════════════════════════════════════╣\n");
    printf("║ 1. Operazioni base (+, -, *, /)     ║\n");
    printf("║ 2. Operazioni avanzate               ║\n");
    printf("║ 3. Conversioni                       ║\n");
    printf("║ 4. Calcolatrice scientifica          ║\n");
    printf("║ 5. Risolutore equazioni              ║\n");
    printf("║ 6. Statistiche array                 ║\n");
    printf("║ 0. Esci                              ║\n");
    printf("╚══════════════════════════════════════╝\n");
}

void operazioniBase() {
    printf("=== OPERAZIONI BASE ===\n\n");
    
    double a = leggiDouble("Primo numero");
    double b = leggiDouble("Secondo numero");
    
    printf("\n");
    printf("Risultati:\n");
    printf("%.2f + %.2f = %.2f\n", a, b, a + b);
    printf("%.2f - %.2f = %.2f\n", a, b, a - b);
    printf("%.2f * %.2f = %.2f\n", a, b, a * b);
    
    if (b != 0) {
        printf("%.2f / %.2f = %.2f\n", a, b, a / b);
    } else {
        printf("%.2f / %.2f = Errore (divisione per zero)\n", a, b);
    }
}

void operazioniAvanzate() {
    printf("=== OPERAZIONI AVANZATE ===\n\n");
    printf("1. Potenza\n");
    printf("2. Radice quadrata\n");
    printf("3. Fattoriale\n");
    printf("4. MCD (Massimo Comun Divisore)\n");
    printf("5. mcm (minimo comune multiplo)\n");
    printf("Scelta: ");
    
    int scelta;
    scanf("%d", &scelta);
    while (getchar() != '\n');
    printf("\n");
    
    switch(scelta) {
        case 1: {
            double base = leggiDouble("Base");
            double esp = leggiDouble("Esponente");
            printf("\n%.2f ^ %.2f = %.2f\n", base, esp, pow(base, esp));
            break;
        }
        case 2: {
            double num = leggiDouble("Numero");
            if (num >= 0) {
                printf("\n√%.2f = %.2f\n", num, sqrt(num));
            } else {
                printf("\nErrore: radice di numero negativo!\n");
            }
            break;
        }
        case 3: {
            int n;
            printf("Numero: ");
            scanf("%d", &n);
            while (getchar() != '\n');
            
            if (n < 0) {
                printf("\nErrore: fattoriale non definito per negativi!\n");
            } else {
                long long fatt = 1;
                for (int i = 2; i <= n; i++) {
                    fatt *= i;
                }
                printf("\n%d! = %lld\n", n, fatt);
            }
            break;
        }
        case 4: {
            int a, b;
            printf("Primo numero: ");
            scanf("%d", &a);
            printf("Secondo numero: ");
            scanf("%d", &b);
            while (getchar() != '\n');
            
            int temp;
            int x = a, y = b;
            while (y != 0) {
                temp = y;
                y = x % y;
                x = temp;
            }
            printf("\nMCD(%d, %d) = %d\n", a, b, x);
            break;
        }
        case 5: {
            int a, b;
            printf("Primo numero: ");
            scanf("%d", &a);
            printf("Secondo numero: ");
            scanf("%d", &b);
            while (getchar() != '\n');
            
            // mcm = (a * b) / MCD(a, b)
            int temp;
            int x = a, y = b;
            while (y != 0) {
                temp = y;
                y = x % y;
                x = temp;
            }
            int mcd = x;
            int mcm = (a * b) / mcd;
            printf("\nmcm(%d, %d) = %d\n", a, b, mcm);
            break;
        }
        default:
            printf("Scelta non valida!\n");
    }
}

void conversioni() {
    printf("=== CONVERSIONI ===\n\n");
    printf("1. Celsius ↔ Fahrenheit\n");
    printf("2. Km ↔ Miglia\n");
    printf("3. Kg ↔ Libbre\n");
    printf("4. Decimale → Binario\n");
    printf("Scelta: ");
    
    int scelta;
    scanf("%d", &scelta);
    while (getchar() != '\n');
    printf("\n");
    
    switch(scelta) {
        case 1: {
            printf("1. Celsius → Fahrenheit\n");
            printf("2. Fahrenheit → Celsius\n");
            printf("Scelta: ");
            int direzione;
            scanf("%d", &direzione);
            while (getchar() != '\n');
            
            double temp = leggiDouble("Temperatura");
            
            if (direzione == 1) {
                double f = temp * 9.0 / 5.0 + 32.0;
                printf("\n%.2f°C = %.2f°F\n", temp, f);
            } else {
                double c = (temp - 32.0) * 5.0 / 9.0;
                printf("\n%.2f°F = %.2f°C\n", temp, c);
            }
            break;
        }
        case 2: {
            printf("1. Km → Miglia\n");
            printf("2. Miglia → Km\n");
            printf("Scelta: ");
            int direzione;
            scanf("%d", &direzione);
            while (getchar() != '\n');
            
            double distanza = leggiDouble("Distanza");
            
            if (direzione == 1) {
                double miglia = distanza * 0.621371;
                printf("\n%.2f km = %.2f miglia\n", distanza, miglia);
            } else {
                double km = distanza / 0.621371;
                printf("\n%.2f miglia = %.2f km\n", distanza, km);
            }
            break;
        }
        case 3: {
            printf("1. Kg → Libbre\n");
            printf("2. Libbre → Kg\n");
            printf("Scelta: ");
            int direzione;
            scanf("%d", &direzione);
            while (getchar() != '\n');
            
            double peso = leggiDouble("Peso");
            
            if (direzione == 1) {
                double libbre = peso * 2.20462;
                printf("\n%.2f kg = %.2f libbre\n", peso, libbre);
            } else {
                double kg = peso / 2.20462;
                printf("\n%.2f libbre = %.2f kg\n", peso, kg);
            }
            break;
        }
        case 4: {
            int num;
            printf("Numero decimale: ");
            scanf("%d", &num);
            while (getchar() != '\n');
            
            if (num < 0) {
                printf("\nErrore: inserisci un numero positivo!\n");
                break;
            }
            
            int binario[32];
            int i = 0;
            int temp = num;
            
            if (temp == 0) {
                printf("\n%d in binario = 0\n", num);
            } else {
                while (temp > 0) {
                    binario[i] = temp % 2;
                    temp /= 2;
                    i++;
                }
                
                printf("\n%d in binario = ", num);
                for (int j = i - 1; j >= 0; j--) {
                    printf("%d", binario[j]);
                }
                printf("\n");
            }
            break;
        }
        default:
            printf("Scelta non valida!\n");
    }
}

void calcolatoreScientifica() {
    printf("=== CALCOLATRICE SCIENTIFICA ===\n\n");
    printf("1. sin(x)\n");
    printf("2. cos(x)\n");
    printf("3. tan(x)\n");
    printf("4. log(x) base 10\n");
    printf("5. ln(x) logaritmo naturale\n");
    printf("6. e^x\n");
    printf("Scelta: ");
    
    int scelta;
    scanf("%d", &scelta);
    while (getchar() != '\n');
    printf("\n");
    
    double x;
    
    switch(scelta) {
        case 1:
            x = leggiDouble("x (in radianti)");
            printf("\nsin(%.2f) = %.4f\n", x, sin(x));
            break;
        case 2:
            x = leggiDouble("x (in radianti)");
            printf("\ncos(%.2f) = %.4f\n", x, cos(x));
            break;
        case 3:
            x = leggiDouble("x (in radianti)");
            printf("\ntan(%.2f) = %.4f\n", x, tan(x));
            break;
        case 4:
            x = leggiDouble("x");
            if (x > 0) {
                printf("\nlog₁₀(%.2f) = %.4f\n", x, log10(x));
            } else {
                printf("\nErrore: logaritmo di numero non positivo!\n");
            }
            break;
        case 5:
            x = leggiDouble("x");
            if (x > 0) {
                printf("\nln(%.2f) = %.4f\n", x, log(x));
            } else {
                printf("\nErrore: logaritmo di numero non positivo!\n");
            }
            break;
        case 6:
            x = leggiDouble("x");
            printf("\ne^%.2f = %.4f\n", x, exp(x));
            break;
        default:
            printf("Scelta non valida!\n");
    }
}

void risolutoreEquazioni() {
    printf("=== RISOLUTORE EQUAZIONI ===\n\n");
    printf("Equazione di secondo grado: ax² + bx + c = 0\n\n");
    
    double a = leggiDouble("Coefficiente a");
    double b = leggiDouble("Coefficiente b");
    double c = leggiDouble("Coefficiente c");
    
    if (a == 0) {
        printf("\nErrore: 'a' non può essere zero!\n");
        return;
    }
    
    double delta = b * b - 4 * a * c;
    
    printf("\nEquazione: %.2fx² + %.2fx + %.2f = 0\n", a, b, c);
    printf("Delta (Δ) = %.2f\n\n", delta);
    
    if (delta > 0) {
        double x1 = (-b + sqrt(delta)) / (2 * a);
        double x2 = (-b - sqrt(delta)) / (2 * a);
        printf("Due soluzioni reali:\n");
        printf("x₁ = %.4f\n", x1);
        printf("x₂ = %.4f\n", x2);
    } else if (delta == 0) {
        double x = -b / (2 * a);
        printf("Una soluzione (doppia):\n");
        printf("x = %.4f\n", x);
    } else {
        double realPart = -b / (2 * a);
        double imagPart = sqrt(-delta) / (2 * a);
        printf("Due soluzioni complesse:\n");
        printf("x₁ = %.4f + %.4fi\n", realPart, imagPart);
        printf("x₂ = %.4f - %.4fi\n", realPart, imagPart);
    }
}

void statistiche() {
    printf("=== STATISTICHE ARRAY ===\n\n");
    
    int n;
    printf("Quanti numeri vuoi inserire? ");
    scanf("%d", &n);
    while (getchar() != '\n');
    
    if (n <= 0 || n > 100) {
        printf("\nErrore: numero non valido!\n");
        return;
    }
    
    double numeri[100];
    
    printf("\nInserisci %d numeri:\n", n);
    for (int i = 0; i < n; i++) {
        printf("Numero %d: ", i + 1);
        scanf("%lf", &numeri[i]);
    }
    while (getchar() != '\n');
    
    // Calcoli
    double somma = 0;
    double max = numeri[0];
    double min = numeri[0];
    
    for (int i = 0; i < n; i++) {
        somma += numeri[i];
        if (numeri[i] > max) max = numeri[i];
        if (numeri[i] < min) min = numeri[i];
    }
    
    double media = somma / n;
    
    // Varianza e deviazione standard
    double varianza = 0;
    for (int i = 0; i < n; i++) {
        varianza += (numeri[i] - media) * (numeri[i] - media);
    }
    varianza /= n;
    double devStd = sqrt(varianza);
    
    // Mediana (ordina prima)
    // Bubble sort
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (numeri[j] > numeri[j + 1]) {
                double temp = numeri[j];
                numeri[j] = numeri[j + 1];
                numeri[j + 1] = temp;
            }
        }
    }
    
    double mediana;
    if (n % 2 == 0) {
        mediana = (numeri[n/2 - 1] + numeri[n/2]) / 2.0;
    } else {
        mediana = numeri[n/2];
    }
    
    // Risultati
    printf("\n╔════════════════════════════════════╗\n");
    printf("║         STATISTICHE                ║\n");
    printf("╠════════════════════════════════════╣\n");
    printf("║ Numero elementi:      %-12d ║\n", n);
    printf("║ Somma:                %-12.2f ║\n", somma);
    printf("║ Media:                %-12.2f ║\n", media);
    printf("║ Mediana:              %-12.2f ║\n", mediana);
    printf("║ Massimo:              %-12.2f ║\n", max);
    printf("║ Minimo:               %-12.2f ║\n", min);
    printf("║ Range:                %-12.2f ║\n", max - min);
    printf("║ Varianza:             %-12.2f ║\n", varianza);
    printf("║ Deviazione standard:  %-12.2f ║\n", devStd);
    printf("╚════════════════════════════════════╝\n");
}

double leggiDouble(const char* messaggio) {
    double valore;
    printf("%s: ", messaggio);
    while (scanf("%lf", &valore) != 1) {
        printf("Errore: inserisci un numero valido\n");
        printf("%s: ", messaggio);
        while (getchar() != '\n');
    }
    return valore;
}
```

---

## Idee per Altri Progetti

### Progetto 4: Gioco del Tris (Tic-Tac-Toe)
- Scacchiera 3×3
- Due giocatori o giocatore vs computer
- Controllo vittoria e pareggio

### Progetto 5: Rubrica Telefonica
- Gestione contatti (nome, telefono, email)
- Ricerca, aggiunta, modifica, cancellazione
- Ordinamento alfabetico
- Salvataggio su file

### Progetto 6: Gestione Magazzino
- Prodotti con codice, nome, quantità, prezzo
- Operazioni: carico, scarico, inventario
- Calcolo valore totale magazzino
- Report prodotti in esaurimento

### Progetto 7: Quiz Interattivo
- Database domande e risposte
- Punteggio e timer
- Categorie diverse
- Classifica giocatori

### Progetto 8: Simulatore di Conti Bancari
- Conti con saldo
- Operazioni: deposito, prelievo, bonifico
- Storico transazioni
- Calcolo interessi

---

## Checklist Progetto Finale

Prima di consegnare il progetto, verifica:

### Funzionalità
- ☐ Il programma compila senza errori
- ☐ Tutte le funzionalità richieste sono implementate
- ☐ Il programma non crasha con input normali
- ☐ Gli errori dell'utente sono gestiti correttamente

### Codice
- ☐ Il codice è ben indentato
- ☐ Le variabili hanno nomi significativi
- ☐ Le funzioni hanno un solo compito chiaro
- ☐ Non c'è codice duplicato
- ☐ Ci sono commenti dove necessario

### Testing
- ☐ Testato con input validi
- ☐ Testato con input non validi
- ☐ Testato casi limite (0, numeri grandi, stringhe vuote)
- ☐ Non ci sono memory leak (se usi allocazione dinamica)

### Documentazione
- ☐ README con istruzioni d'uso
- ☐ Commenti nelle funzioni principali
- ☐ Descrizione del progetto

---

## Consigli Finali

**Per il progetto finale:**
1. **Inizia semplice**: implementa prima le funzionalità base
2. **Testa frequentemente**: non aspettare di finire tutto
3. **Commit regolari**: se usi git, salva progressi frequenti
4. **Chiedi aiuto**: se ti blocchi, non esitare
5. **Documenta mentre scrivi**: non lasciare i commenti per la fine

**Per continuare a imparare:**
- Pratica regolarmente con esercizi
- Leggi codice di altri programmatori
- Partecipa a progetti open source
- Studia strutture dati avanzate
- Impara altri linguaggi (Python, Java, C++)

---

## Riepilogo del Corso

### Cosa hai imparato:

✓ **Basi della programmazione**: algoritmi, flowchart, pseudocodice  
✓ **Sistemi numerici**: binario, conversioni, rappresentazione dati  
✓ **Linguaggio C**: sintassi, tipi di dato, operatori  
✓ **Controllo flusso**: if-else, switch, cicli (while, for)  
✓ **Array**: monodimensionali, matrici, manipolazione  
✓ **Stringhe**: gestione, funzioni string.h  
✓ **Funzioni**: modularità, ricorsione, scope  
✓ **Puntatori**: memoria, passaggio riferimento, aritmetica  
✓ **Debugging**: tecniche, testing, gestione errori  
✓ **Progetto completo**: dalla pianificazione all'implementazione  

### Prossimi passi:
- Strutture e union
- Allocazione dinamica (malloc, free)
- File I/O avanzato
- Liste linkate
- Alberi e grafi
- Algoritmi di ordinamento avanzati
- Programmazione su sistemi embedded

---

## Risorse Utili

**Documentazione:**
- [C Reference](https://en.cppreference.com/w/c)
- [GNU C Library Manual](https://www.gnu.org/software/libc/manual/)

**Esercizi:**
- [HackerRank - C Programming](https://www.hackerrank.com/domains/c)
- [Exercism - C Track](https://exercism.org/tracks/c)
- [Project Euler](https://projecteuler.net/)

**Libri consigliati:**
- "The C Programming Language" - Kernighan & Ritchie
- "C Programming: A Modern Approach" - K.N. King

---

## Congratulazioni! 🎉

Hai completato il corso di **Fondamenti di Programmazione in C**!

Ora hai le basi solide per:
- Sviluppare programmi C completi
- Risolvere problemi complessi con algoritmi
- Lavorare su progetti più avanzati
- Imparare altri linguaggi di programmazione

**Continua a programmare e buona fortuna nel tuo percorso da sviluppatore!** 💻pi funzioni
void funzione1();
int funzione2(int x);

// Funzione principale
int main() {
    // Codice
    return 0;
}

// Definizioni funzioni
void funzione1() {
    // ...
}
```

**Tipi di dato:**
```c
char c = 'A';           // Carattere (1 byte)
int i = 42;             // Intero (4 byte)
float f = 3.14f;        // Float (4 byte)
double d = 3.14159;     // Double (8 byte)
```

**Operatori:**
```c
// Aritmetici: + - * / %
// Relazionali: < > <= >= == !=
// Logici: && || !
// Assegnamento: = += -= *= /=
```

**Strutture di controllo:**
```c
// If-else
if (condizione) { }
else if (condizione2) { }
else { }

// Switch
switch (valore) {
    case 1: break;
    default: break;
}

// While
while (condizione) { }

// Do-while
do { } while (condizione);

// For
for (init; condizione; incremento) { }
```

**Array:**
```c
int arr[10];                    // Dichiarazione
int arr2[] = {1, 2, 3, 4, 5};  // Inizializzazione
arr[0] = 10;                    // Accesso

// Matrice
int mat[3][4];
mat[0][0] = 1;
```

**Stringhe:**
```c
char str[50] = "Ciao";
strlen(str);     // Lunghezza
strcpy(dest, src);  // Copia
strcat(dest, src);  // Concatena
strcmp(s1, s2);     // Confronta
```

**Funzioni:**
```c
tipo nomeFunzione(parametri) {
    // corpo
    return valore;
}
```

**Puntatori:**
```c
int x = 10;
int* p = &x;   // p punta a x
*p = 20;       // Modifica x attraverso p
```

---

## PARTE PRATICA (3h) - PROGETTO FINALE

### Progetto 1: Gestionale Biblioteca

Sistema per gestire libri, prestiti e utenti.

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define MAX_LIBRI 100
#define MAX_UTENTI 50
#define MAX_TITOLO 100
#define MAX_AUTORE 100
#define MAX_NOME 50

// Strutture dati
typedef struct {
    int id;
    char titolo[MAX_TITOLO];
    char autore[MAX_AUTORE];
    int anno;
    int disponibile;  // 1 = disponibile, 0 = prestato
} Libro;

typedef struct {
    int id;
    char nome[MAX_NOME];
    int libroPrestato;  // -1 = nessuno, altrimenti id libro
} Utente;

// Variabili globali
Libro biblioteca[MAX_LIBRI];
Utente utenti[MAX_UTENTI];
int numLibri = 0;
int numUtenti = 0;

// Prototipi
void menu();
void aggiungiLibro();
void visualizzaLibri();
void cercaLibro();
void prestaLibro();
void restituisciLibro();
void aggiungiUtente();
void visualizzaUtenti();
void statistiche();
void salvasuFile();
void caricaDaFile();
int leggiIntero(const char* messaggio);
void leggiStringa(const char* messaggio, char* buffer, int dim);

int main() {
    int scelta;
    
    // Carica dati salvati
    caricaDaFile();
    
    printf("=== SISTEMA GESTIONE BIBLIOTECA ===\n\n");
    
    do {
        menu();
        scelta = leggiIntero("Scelta");
        printf("\n");
        
        switch(scelta) {
            case 1:
                aggiungiLibro();
                break;
            case 2:
                visualizzaLibri();
                break;
            case 3:
                cercaLibro();
                break;
            case 4:
                prestaLibro();
                break;
            case 5:
                restituisciLibro();
                break;
            case 6:
                aggiungiUtente();
                break;
            case 7:
                visualizzaUtenti();
                break;
            case 8:
                statistiche();
                break;
            case 9:
                salvasuFile();
                printf("Dati salvati!\n");
                break;
            case 0:
                salvasuFile();
                printf("Arrivederci!\n");
                break;
            default:
                printf("Scelta non valida!\n");
        }
        
        if (scelta != 0) {
            printf("\nPremi INVIO per continuare...");
            getchar();
        }
        
    } while (scelta != 0);
    
    return 0;
}

void menu() {
    printf("\n");
    printf("╔════════════════════════════════════╗\n");
    printf("║     GESTIONE BIBLIOTECA            ║\n");
    printf("╠════════════════════════════════════╣\n");
    printf("║ 1. Aggiungi libro                  ║\n");
    printf("║ 2. Visualizza tutti i libri        ║\n");
    printf("║ 3. Cerca libro                     ║\n");
    printf("║ 4. Presta libro                    ║\n");
    printf("║ 5. Restituisci libro               ║\n");
    printf("║ 6. Aggiungi utente                 ║\n");
    printf("║ 7. Visualizza utenti               ║\n");
    printf("║ 8. Statistiche                     ║\n");
    printf("║ 9. Salva su file                   ║\n");
    printf("║ 0. Esci                            ║\n");
    printf("╚════════════════════════════════════╝\n");
}

void aggiungiLibro() {
    if (numLibri >= MAX_LIBRI) {
        printf("Errore: biblioteca piena!\n");
        return;
    }
    
    Libro nuovoLibro;
    nuovoLibro.id = numLibri;
    
    printf("=== AGGIUNGI NUOVO LIBRO ===\n");
    leggiStringa("Titolo", nuovoLibro.titolo, MAX_TITOLO);
    leggiStringa("Autore", nuovoLibro.autore, MAX_AUTORE);
    nuovoLibro.anno = leggiIntero("Anno di pubblicazione");
    nuovoLibro.disponibile = 1;
    
    biblioteca[numLibri] = nuovoLibro;
    numLibri++;
    
    printf("\nLibro aggiunto con successo! ID: %d\n", nuovoLibro.id);
}

void visualizzaLibri() {
    if (numLibri == 0) {
        printf("Nessun libro in biblioteca.\n");
        return;
    }
    
    printf("=== ELENCO LIBRI ===\n\n");
    printf("%-4s %-30s %-25s %-6s %-12s\n", 
           "ID", "Titolo", "Autore", "Anno", "Stato");
    printf("─────────────────────────────────────────────────────────────────────────\n");
    
    for (int i = 0; i < numLibri; i++) {
        printf("%-4d %-30s %-25s %-6d %-12s\n",
               biblioteca[i].id,
               biblioteca[i].titolo,
               biblioteca[i].autore,
               biblioteca[i].anno,
               biblioteca[i].disponibile ? "Disponibile" : "Prestato");
    }
}

void cercaLibro() {
    if (numLibri == 0) {
        printf("Nessun libro in biblioteca.\n");
        return;
    }
    
    char ricerca[MAX_TITOLO];
    leggiStringa("Cerca per titolo o autore", ricerca, MAX_TITOLO);
    
    printf("\n=== RISULTATI RICERCA ===\n");
    int trovati = 0;
    
    for (int i = 0; i < numLibri; i++) {
        if (strstr(biblioteca[i].titolo, ricerca) != NULL ||
            strstr(biblioteca[i].autore, ricerca) != NULL) {
            
            printf("\nID: %d\n", biblioteca[i].id);
            printf("Titolo: %s\n", biblioteca[i].titolo);
            printf("Autore: %s\n", biblioteca[i].autore);
            printf("Anno: %d\n", biblioteca[i].anno);
            printf("Stato: %s\n", biblioteca[i].disponibile ? "Disponibile" : "Prestato");
            printf("───────────────────────────\n");
            trovati++;
        }
    }
    
    if (trovati == 0) {
        printf("Nessun libro trovato.\n");
    } else {
        printf("\nTrovati %d libri.\n", trovati);
    }
}

void prestaLibro() {
    if (numUtenti == 0) {
        printf("Nessun utente registrato! Aggiungi prima un utente.\n");
        return;
    }
    
    int idUtente = leggiIntero("ID utente");
    
    if (idUtente < 0 || idUtente >= numUtenti) {
        printf("Errore: utente non valido!\n");
        return;
    }
    
    if (utenti[idUtente].libroPrestato != -1) {
        printf("Errore: l'utente ha già un libro in prestito!\n");
        return;
    }
    
    int idLibro = leggiIntero("ID libro da prestare");
    
    if (idLibro < 0 || idLibro >= numLibri) {
        printf("Errore: libro non valido!\n");
        return;
    }
    
    if (!biblioteca[idLibro].disponibile) {
        printf("Errore: libro non disponibile!\n");
        return;
    }
    
    // Effettua il prestito
    biblioteca[idLibro].disponibile = 0;
    utenti[idUtente].libroPrestato = idLibro;
    
    printf("\nPrestito effettuato con successo!\n");
    printf("Utente: %s\n", utenti[idUtente].nome);
    printf("Libro: %s\n", biblioteca[idLibro].titolo);
}

void restituisciLibro() {
    int idUtente = leggiIntero("ID utente");
    
    if (idUtente < 0 || idUtente >= numUtenti) {
        printf("Errore: utente non valido!\n");
        return;
    }
    
    if (utenti[idUtente].libroPrestato == -1) {
        printf("Errore: l'utente non ha libri in prestito!\n");
        return;
    }
    
    int idLibro = utenti[idUtente].libroPrestato;
    
    // Restituzione
    biblioteca[idLibro].disponibile = 1;
    utenti[idUtente].libroPrestato = -1;
    
    printf("\nRestituzione effettuata con successo!\n");
    printf("Libro: %s\n", biblioteca[idLibro].titolo);
}

void aggiungiUtente() {
    if (numUtenti >= MAX_UTENTI) {
        printf("Errore: troppi utenti!\n");
        return;
    }
    
    Utente nuovoUtente;
    nuovoUtente.id = numUtenti;
    
    printf("=== AGGIUNGI NUOVO UTENTE ===\n");
    leggiStringa("Nome", nuovoUtente.nome, MAX_NOME);
    nuovoUtente.libroPrestato = -1;
    
    utenti[numUtenti] = nuovoUtente;
    numUtenti++;
    
    printf("\nUtente aggiunto con successo! ID: %d\n", nuovoUtente.id);
}

void visualizzaUtenti() {
    if (numUtenti == 0) {
        printf("Nessun utente registrato.\n");
        return;
    }
    
    printf("=== ELENCO UTENTI ===\n\n");
    printf("%-4s %-30s %-30s\n", "ID", "Nome", "Libro in Prestito");
    printf("────────────────────────────────────────────────────────────────\n");
    
    for (int i = 0; i < numUtenti; i++) {
        printf("%-4d %-30s ", utenti[i].id, utenti[i].nome);
        if (utenti[i].libroPrestato == -1) {
            printf("Nessuno\n");
        } else {
            printf("%s\n", biblioteca[utenti[i].libroPrestato].titolo);
        }
    }
}

void statistiche() {
    printf("=== STATISTICHE BIBLIOTECA ===\n\n");
    printf("Numero totale libri: %d\n", numLibri);
    
    int disponibili = 0;
    for (int i = 0; i < numLibri; i++) {
        if (biblioteca[i].disponibile) disponibili++;
    }
    
    printf("Libri disponibili: %d\n", disponibili);
    printf("Libri in prestito: %d\n", numLibri - disponibili);
    printf("Numero utenti: %d\n", numUtenti);
    
    if (numLibri > 0) {
        printf("Percentuale disponibilità: %.1f%%\n", 
               (float)disponibili / numLibri * 100);
    }
}

void salvasuFile() {
    FILE* file = fopen("biblioteca.dat", "wb");
    if (file == NULL) {
        printf("Errore nel salvataggio!\n");
        return;
    }
    
    fwrite(&numLibri, sizeof(int), 1, file);
    fwrite(biblioteca, sizeof(Libro), numLibri, file);
    fwrite(&numUtenti, sizeof(int), 1, file);
    fwrite(utenti, sizeof(Utente), numUtenti, file);
    
    fclose(file);
}

void caricaDaFile() {
    FILE* file = fopen("biblioteca.dat", "rb");
    if (file == NULL) {
        return;  // File non esiste, database vuoto
    }
    
    fread(&numLibri, sizeof(int), 1, file);
    fread(biblioteca, sizeof(Libro), numLibri, file);
    fread(&numUtenti, sizeof(int), 1, file);
    fread(utenti, sizeof(Utente), numUtenti, file);
    
    fclose(file);
    printf("Dati caricati: %d libri, %d utenti\n\n", numLibri, numUtenti);
}

int leggiIntero(const char* messaggio) {
    int valore;
    printf("%s: ", messaggio);
    while (scanf("%d", &valore) != 1) {
        printf("Errore: inserisci un numero valido\n");
        printf("%s: ", messaggio);
        while (getchar() != '\n');
    }
    getchar();  // Consuma newline
    return valore;
}

void leggiStringa(const char* messaggio, char* buffer, int dim) {
    printf("%s: ", messaggio);
    fgets(buffer, dim, stdin);
    buffer[strcspn(buffer, "\n")] = '\0';  // Rimuovi newline
}
```

---

### Progetto 2: Gioco dell'Impiccato

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <time.h>
#include <stdlib.h>

#define MAX_TENTATIVI 6
#define MAX_PAROLA 50

// Prototipi
void stampaImpiccato(int errori);
void stampaMaschera(const char* parola, const int* indovinate);
int verificaLettera(const char* parola, char lettera, int* indovinate);
int tutteIndovinate(const int* indovinate, int lunghezza);
char leggiLettera();

// Database parole
const char* parole[] = {
    "programmazione",
    "computer",
    "algoritmo",
    "variabile",
    "funzione",
    "puntatore",
    "array",
    "stringa",
    "ciclo",
    "condizione"
};

int main() {
    srand(time(NULL));
    
    char giocareAncora;
    
    do {
        // Scegli parola casuale
        int indice = rand() % 10;
        const char* parola = parole[indice];
        int lunghezza = strlen(parola);
        
        int indovinate[MAX_PAROLA] = {0};
        char lettereProvate[26] = {0};
        int numLettere = 0;
        int errori = 0;
        
        printf("\n");
        printf("╔══════════════════════════════╗\n");
        printf("║    GIOCO DELL'IMPICCATO      ║\n");
        printf("╚══════════════════════════════╝\n");
        printf("\n");
        printf("Indovina la parola! Hai %d tentativi.\n\n", MAX_TENTATIVI);
        
        // Ciclo di gioco
        while (errori < MAX_TENTATIVI && !tutteIndovinate(indovinate, lunghezza)) {
            stampaImpiccato(errori);
            printf("\n");
            stampaMaschera(parola, indovinate);
            printf("\n");
            
            // Mostra lettere provate
            if (numLettere > 0) {
                printf("Lettere provate: ");
                for (int i = 0; i < numLettere; i++) {
                    printf("%c ", lettereProvate[i]);
                }
                printf("\n");
            }
            
            printf("Tentativi rimasti: %d\n\n", MAX_TENTATIVI - errori);
            
            // Leggi lettera
            char lettera = leggiLettera();
            
            // Controlla se già provata
            int giàProvata = 0;
            for (int i = 0; i < numLettere; i++) {
                if (lettereProvate[i] == lettera) {
                    giàProvata = 1;
                    break;
                }
            }
            
            if (giàProvata) {
                printf("\n❌ Lettera già provata!\n\n");
                continue;
            }
            
            lettereProvate[numLettere++] = lettera;
            
            // Verifica lettera
            if (verificaLettera(parola, lettera, indovinate)) {
                printf("\n✓ Lettera corretta!\n\n");
            } else {
                errori++;
                printf("\n✗ Lettera sbagliata!\n\n");
            }
        }
        
        // Fine gioco
        printf("\n");
        stampaImpiccato(errori);
        printf("\n");
        
        if (tutteIndovinate(indovinate, lunghezza)) {
            printf("╔══════════════════════════════╗\n");
            printf("║    🎉 HAI VINTO! 🎉          ║\n");
            printf("╚══════════════════════════════╝\n");
        } else {
            printf("╔══════════════════════════════╗\n");
            printf("║    ☠️  HAI PERSO! ☠️          ║\n");
            printf("╚══════════════════════════════╝\n");
        }
        
        printf("\nLa parola era: %s\n\n", parola);
        
        printf("Vuoi giocare ancora? (s/n): ");
        scanf(" %c", &giocareAncora);
        while (getchar() != '\n');
        
    } while (tolower(giocareAncora) == 's');
    
    printf("\nGrazie per aver giocato!\n");
    
    return 0;
}

void stampaImpiccato(int errori) {
    printf("  ┌─────┐\n");
    
    if (errori >= 1) printf("  │     O\n");
    else printf("  │      \n");
    
    if (errori >= 4) printf("  │    /|＼\n");
    else if (errori >= 3) printf("  │    /|\n");
    else if (errori >= 2) printf("  │     |\n");
    else printf("  │      \n");
    
    if (errori >= 6) printf("  │    / ＼\n");
    else if (errori >= 5) printf("  │    /\n");
    else printf("  │      \n");
    
    printf("  │\n");
    printf("═════════\n");
}

void stampaMaschera(const char* parola, const int* indovinate) {
    printf("Parola: ");
    for (int i = 0; parola[i] != '\0'; i++) {
        if (indovinate[i]) {
            printf("%c ", parola[i]);
        } else {
            printf("_ ");
        }
    }
    printf("\n");
}

int verificaLettera(const char* parola, char lettera, int* indovinate) {
    int trovata = 0;
    for (int i = 0; parola[i] != '\0'; i++) {
        if (tolower(parola[i]) == tolower(lettera)) {
            indovinate[i] = 1;
            trovata = 1;
        }
    }
    return trovata;
}

int tutteIndovinate(const int* indovinate, int lunghezza) {
    for (int i = 0; i < lunghezza; i++) {
        if (!indovinate[i]) return 0;
    }
    return 1;
}

char leggiLettera() {
    char lettera;
    printf("Inserisci una lettera: ");
    scanf(" %c", &lettera);
    while (getchar() != '\n');
    return tolower(lettera);
}
```

---

### Progetto 3: Calcolatrice Avanzata

```c
#include <stdio.h>
#include <math.h>
#include <string.h>

// Prototi