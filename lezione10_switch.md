# LEZIONE 10 - Costrutto di Selezione: switch-case
**Durata: 4 ore | Teoria: 2h | Esercizi: 2h**

---

## PARTE TEORICA (2 ore)

### 10.1 Introduzione allo switch (15 min)

#### Cos'è lo switch?

Lo **switch** è un costrutto di selezione multipla che permette di scegliere tra diverse alternative in base al valore di una **singola espressione**.

**Analogia:** Come un centralino telefonico che smista le chiamate verso estensioni diverse.

#### Quando usare switch?

**✓ USA switch quando:**
- Confronti **una variabile** con **molte costanti**
- Hai **4+ alternative** discrete
- I valori sono **interi** o **caratteri**
- Vuoi codice più **leggibile** e **performante**

**✗ USA if-else quando:**
- Condizioni **complesse** (range, AND, OR)
- Confronti con **float**
- Poche alternative (2-3)
- Condizioni **non costanti**

#### Confronto if-else vs switch

**Con if-else (verboso):**
```c
int scelta = 2;

if (scelta == 1) {
    printf("Opzione 1\n");
} else if (scelta == 2) {
    printf("Opzione 2\n");
} else if (scelta == 3) {
    printf("Opzione 3\n");
} else if (scelta == 4) {
    printf("Opzione 4\n");
} else {
    printf("Opzione non valida\n");
}
```

**Con switch (più chiaro):**
```c
int scelta = 2;

switch (scelta) {
    case 1:
        printf("Opzione 1\n");
        break;
    case 2:
        printf("Opzione 2\n");
        break;
    case 3:
        printf("Opzione 3\n");
        break;
    case 4:
        printf("Opzione 4\n");
        break;
    default:
        printf("Opzione non valida\n");
}
```

---

### 10.2 Sintassi dello switch (20 min)

#### Struttura Base

```c
switch (espressione) {
    case costante1:
        // Istruzioni per costante1
        break;
    
    case costante2:
        // Istruzioni per costante2
        break;
    
    case costante3:
        // Istruzioni per costante3
        break;
    
    default:
        // Istruzioni se nessun case corrisponde
        break;  // Opzionale nell'ultimo caso
}
```

#### Componenti

**1. switch (espressione)**
- `espressione`: valutata UNA sola volta
- Deve essere **intero** o **carattere**
- Non può essere float, double, stringa

**2. case costante:**
- `costante`: deve essere **letterale** o **costante nota a compile-time**
- Non può essere variabile
- Ogni case deve essere **unico**

**3. break:**
- **Esce** dallo switch
- Senza break, l'esecuzione **continua** al caso successivo (fall-through)

**4. default:**
- Caso **opzionale**
- Eseguito se **nessun case** corrisponde
- Può essere in **qualsiasi posizione** (convenzionalmente alla fine)

#### Flow Chart: switch

```
          ┌──────────┐
          │Valuta    │
          │espressione│
          └────┬─────┘
               │
      ┌────────┼────────┬────────┐
      │        │        │        │
   case 1?  case 2?  case 3?  default
      │        │        │        │
      ▼        ▼        ▼        ▼
   ┌────┐  ┌────┐  ┌────┐  ┌────┐
   │Code│  │Code│  │Code│  │Code│
   └─┬──┘  └─┬──┘  └─┬──┘  └─┬──┘
     │       │       │       │
  break?  break?  break?     │
     │       │       │       │
     └───────┴───────┴───────┘
              │
         (continua)
```

#### Esempi Base

**Esempio 1: Menu semplice**
```c
int opzione;
printf("Scegli (1-3): ");
scanf("%d", &opzione);

switch (opzione) {
    case 1:
        printf("Hai scelto opzione 1\n");
        break;
    case 2:
        printf("Hai scelto opzione 2\n");
        break;
    case 3:
        printf("Hai scelto opzione 3\n");
        break;
    default:
        printf("Opzione non valida\n");
}
```

**Esempio 2: Giorni della settimana**
```c
int giorno;
printf("Numero giorno (1-7): ");
scanf("%d", &giorno);

switch (giorno) {
    case 1:
        printf("Lunedì\n");
        break;
    case 2:
        printf("Martedì\n");
        break;
    case 3:
        printf("Mercoledì\n");
        break;
    case 4:
        printf("Giovedì\n");
        break;
    case 5:
        printf("Venerdì\n");
        break;
    case 6:
        printf("Sabato\n");
        break;
    case 7:
        printf("Domenica\n");
        break;
    default:
        printf("Giorno non valido\n");
}
```

**Esempio 3: Caratteri**
```c
char voto;
printf("Voto letterale (A-F): ");
scanf(" %c", &voto);

switch (voto) {
    case 'A':
        printf("Ottimo! 90-100\n");
        break;
    case 'B':
        printf("Buono! 80-89\n");
        break;
    case 'C':
        printf("Discreto! 70-79\n");
        break;
    case 'D':
        printf("Sufficiente! 60-69\n");
        break;
    case 'F':
        printf("Insufficiente! <60\n");
        break;
    default:
        printf("Voto non riconosciuto\n");
}
```

---

### 10.3 L'istruzione break (15 min)

#### Ruolo del break

Il **break** esce immediatamente dallo switch, saltando tutti i case successivi.

**Con break (normale):**
```c
int x = 2;

switch (x) {
    case 1:
        printf("Uno\n");
        break;
    case 2:
        printf("Due\n");
        break;  // Esce qui
    case 3:
        printf("Tre\n");
        break;
}

// Output: Due
```

**Senza break (fall-through):**
```c
int x = 2;

switch (x) {
    case 1:
        printf("Uno\n");
        // Manca break!
    case 2:
        printf("Due\n");
        // Manca break!
    case 3:
        printf("Tre\n");
        break;
}

// Output: Due
//         Tre
```

#### Quando omettere break

**Fall-through intenzionale** (raro ma utile):

**Esempio 1: Più case stessa azione**
```c
char ch;
scanf(" %c", &ch);

switch (ch) {
    case 'a':
    case 'e':
    case 'i':
    case 'o':
    case 'u':
        printf("È una vocale\n");
        break;
    default:
        printf("È una consonante\n");
}
```

**Esempio 2: Azioni cumulative**
```c
int livello = 2;

switch (livello) {
    case 3:
        printf("Attiva scudo\n");
        // Fall-through intenzionale
    case 2:
        printf("Attiva arma\n");
        // Fall-through intenzionale
    case 1:
        printf("Attiva movimento\n");
        break;
    default:
        printf("Livello non valido\n");
}

// Con livello=2, output:
// Attiva arma
// Attiva movimento
```

**Esempio 3: Giorni del mese**
```c
int mese;
int giorni;
scanf("%d", &mese);

switch (mese) {
    case 1: case 3: case 5: case 7:
    case 8: case 10: case 12:
        giorni = 31;
        break;
    case 4: case 6: case 9: case 11:
        giorni = 30;
        break;
    case 2:
        giorni = 28;  // Ignoriamo bisestile per semplicità
        break;
    default:
        printf("Mese non valido\n");
        giorni = 0;
}

if (giorni > 0) {
    printf("Giorni: %d\n", giorni);
}
```

#### Best Practice: Commenta fall-through

```c
switch (x) {
    case 1:
        printf("Uno\n");
        // Intenzionale: fall-through al caso 2
    case 2:
        printf("Due\n");
        break;
}
```

---

### 10.4 Il caso default (10 min)

#### Scopo di default

**default** gestisce tutti i valori **non coperti** dai case espliciti.

**Senza default:**
```c
int x = 10;

switch (x) {
    case 1:
        printf("Uno\n");
        break;
    case 2:
        printf("Due\n");
        break;
    // x=10 non fa nulla!
}
```

**Con default:**
```c
int x = 10;

switch (x) {
    case 1:
        printf("Uno\n");
        break;
    case 2:
        printf("Due\n");
        break;
    default:
        printf("Valore non gestito: %d\n", x);
}
```

#### Posizione di default

**Convenzionalmente alla fine:**
```c
switch (x) {
    case 1:
        // ...
        break;
    case 2:
        // ...
        break;
    default:  // Alla fine (standard)
        // ...
}
```

**Ma può essere ovunque:**
```c
switch (x) {
    case 1:
        // ...
        break;
    default:  // In mezzo (funziona ma poco comune)
        // ...
        break;
    case 2:
        // ...
        break;
}
```

**Best Practice:** Metti default **sempre alla fine** per chiarezza.

#### default è opzionale?

**Sì**, ma **raccomandata** per:
- **Validazione input**
- **Gestione errori**
- **Debug** (cattura valori inaspettati)

```c
// Senza default (silenziosamente ignora valori invalidi)
switch (opzione) {
    case 1:
        // ...
        break;
    case 2:
        // ...
        break;
}

// Con default (segnala problema)
switch (opzione) {
    case 1:
        // ...
        break;
    case 2:
        // ...
        break;
    default:
        printf("ERRORE: opzione %d non valida\n", opzione);
}
```

---

### 10.5 Limitazioni dello switch (15 min)

#### 1. Solo Tipi Interi e Char

**✓ Funziona:**
```c
int n = 5;
switch (n) { /* ... */ }

char c = 'A';
switch (c) { /* ... */ }

enum Colore { ROSSO, VERDE, BLU };
enum Colore col = ROSSO;
switch (col) { /* ... */ }
```

**✗ NON funziona:**
```c
float f = 3.14;
switch (f) { /* ... */ }  // ERRORE!

double d = 2.718;
switch (d) { /* ... */ }  // ERRORE!

char str[] = "ciao";
switch (str) { /* ... */ }  // ERRORE!
```

#### 2. Solo Costanti nei case

**✓ Funziona:**
```c
#define MAX 100

switch (x) {
    case 1:        // Letterale
        break;
    case MAX:      // Costante definita
        break;
    case 'A':      // Carattere
        break;
}
```

**✗ NON funziona:**
```c
int limite = 100;

switch (x) {
    case limite:   // ERRORE! Variabile
        break;
    case x + 1:    // ERRORE! Espressione
        break;
}
```

#### 3. Non Supporta Range

**✗ NON funziona (sintassi desiderata):**
```c
switch (voto) {
    case 90-100:   // ERRORE! Range non supportato
        printf("A\n");
        break;
}
```

**✓ Soluzione con case multipli:**
```c
switch (voto) {
    case 90: case 91: case 92: case 93: case 94:
    case 95: case 96: case 97: case 98: case 99:
    case 100:
        printf("A\n");
        break;
}
```

**✓ Meglio: usa if-else per range:**
```c
if (voto >= 90 && voto <= 100) {
    printf("A\n");
} else if (voto >= 80 && voto < 90) {
    printf("B\n");
}
// ...
```

#### 4. case Duplicati Non Permessi

**✗ ERRORE:**
```c
switch (x) {
    case 1:
        printf("Primo\n");
        break;
    case 1:        // ERRORE! Duplicato
        printf("Altro\n");
        break;
}
```

---

### 10.6 switch vs if-else: Quando Usare Cosa (20 min)

#### Tabella Comparativa

| Criterio | switch | if-else |
|----------|--------|---------|
| **Tipo espressione** | Int, char, enum | Qualsiasi |
| **Condizioni** | Solo uguaglianza (==) | Qualsiasi (<, >, &&, \|\|) |
| **Range** | No (workaround verboso) | Sì, facile |
| **Float** | No | Sì |
| **Molte alternative** | Ottimo | Verboso |
| **Poche alternative** | OK | Meglio |
| **Leggibilità** | Alta (se appropriato) | Varia |
| **Performance** | Potenzialmente più veloce* | Dipende |

*Con molti case, il compilatore può usare jump table (O(1)) invece di confronti sequenziali (O(n)).

#### Esempi di Scelta

**Scenario 1: Menu con numeri → switch**
```c
int scelta;
scanf("%d", &scelta);

switch (scelta) {
    case 1: /* ... */ break;
    case 2: /* ... */ break;
    case 3: /* ... */ break;
    case 4: /* ... */ break;
    default: /* ... */
}
```

**Scenario 2: Range di valori → if-else**
```c
int voto;
scanf("%d", &voto);

if (voto >= 90) {
    printf("A\n");
} else if (voto >= 80) {
    printf("B\n");
} else if (voto >= 70) {
    printf("C\n");
}
```

**Scenario 3: Condizioni complesse → if-else**
```c
if (eta >= 18 && ha_patente && credito_sufficiente) {
    printf("Può noleggiare\n");
} else {
    printf("Non può noleggiare\n");
}
```

**Scenario 4: Carattere singolo → switch**
```c
char operatore;
scanf(" %c", &operatore);

switch (operatore) {
    case '+': /* addizione */ break;
    case '-': /* sottrazione */ break;
    case '*': /* moltiplicazione */ break;
    case '/': /* divisione */ break;
    default: printf("Operatore non valido\n");
}
```

**Scenario 5: Float → if-else (obbligatorio)**
```c
float temperatura;
scanf("%f", &temperatura);

if (temperatura < 0) {
    printf("Sotto zero\n");
} else if (temperatura < 15) {
    printf("Freddo\n");
} else {
    printf("Caldo\n");
}
```

#### Conversione if-else → switch

**Se hai:**
```c
if (x == 1) {
    // ...
} else if (x == 2) {
    // ...
} else if (x == 3) {
    // ...
} else {
    // ...
}
```

**Puoi convertire a:**
```c
switch (x) {
    case 1:
        // ...
        break;
    case 2:
        // ...
        break;
    case 3:
        // ...
        break;
    default:
        // ...
}
```

**Ma NON convertire se:**
```c
if (x > 10) {        // Range, non ==
if (x == 1 || y == 2) {  // Condizione composta
if (f == 3.14)       // Float
```

---

### 10.7 Esempi Pratici Avanzati (15 min)

#### Esempio 1: Calcolatrice

```c
#include <stdio.h>

int main() {
    float a, b, risultato;
    char operatore;
    
    printf("Inserisci espressione (es: 5 + 3): ");
    scanf("%f %c %f", &a, &operatore, &b);
    
    switch (operatore) {
        case '+':
            risultato = a + b;
            printf("%.2f + %.2f = %.2f\n", a, b, risultato);
            break;
        
        case '-':
            risultato = a - b;
            printf("%.2f - %.2f = %.2f\n", a, b, risultato);
            break;
        
        case '*':
        case 'x':  // Accetta sia * che x
            risultato = a * b;
            printf("%.2f × %.2f = %.2f\n", a, b, risultato);
            break;
        
        case '/':
            if (b != 0) {
                risultato = a / b;
                printf("%.2f ÷ %.2f = %.2f\n", a, b, risultato);
            } else {
                printf("ERRORE: Divisione per zero!\n");
            }
            break;
        
        default:
            printf("Operatore '%c' non riconosciuto\n", operatore);
    }
    
    return 0;
}
```

#### Esempio 2: Conversione Voto

```c
#include <stdio.h>

int main() {
    int voto;
    printf("Voto (0-100): ");
    scanf("%d", &voto);
    
    if (voto < 0 || voto > 100) {
        printf("Voto non valido\n");
        return 1;
    }
    
    // Converti a decine per usare switch
    int decina = voto / 10;
    
    switch (decina) {
        case 10:  // 100
        case 9:   // 90-99
            printf("Voto: A (Ottimo)\n");
            break;
        
        case 8:   // 80-89
            printf("Voto: B (Buono)\n");
            break;
        
        case 7:   // 70-79
            printf("Voto: C (Discreto)\n");
            break;
        
        case 6:   // 60-69
            printf("Voto: D (Sufficiente)\n");
            break;
        
        default:  // 0-59
            printf("Voto: F (Insufficiente)\n");
    }
    
    return 0;
}
```

#### Esempio 3: Stato Semaforo

```c
#include <stdio.h>

int main() {
    char colore;
    
    printf("Colore semaforo (R/G/Y): ");
    scanf(" %c", &colore);
    
    // Gestisci maiuscole e minuscole
    switch (colore) {
        case 'R':
        case 'r':
            printf("🔴 STOP - Fermati\n");
            break;
        
        case 'G':
        case 'g':
            printf("🟢 GO - Procedi\n");
            break;
        
        case 'Y':
        case 'y':
            printf("🟡 ATTENZIONE - Rallenta\n");
            break;
        
        default:
            printf("Colore non valido\n");
    }
    
    return 0;
}
```

#### Esempio 4: Menu Bancario

```c
#include <stdio.h>

int main() {
    float saldo = 1000.0;
    int scelta;
    float importo;
    
    do {
        printf("\n=== MENU BANCOMAT ===\n");
        printf("1. Visualizza saldo\n");
        printf("2. Deposita\n");
        printf("3. Preleva\n");
        printf("4. Esci\n");
        printf("Scelta: ");
        scanf("%d", &scelta);
        
        switch (scelta) {
            case 1:
                printf("Saldo attuale: %.2f€\n", saldo);
                break;
            
            case 2:
                printf("Importo da depositare: ");
                scanf("%f", &importo);
                if (importo > 0) {
                    saldo += importo;
                    printf("Deposito effettuato. Nuovo saldo: %.2f€\n", saldo);
                } else {
                    printf("Importo non valido\n");
                }
                break;
            
            case 3:
                printf("Importo da prelevare: ");
                scanf("%f", &importo);
                if (importo > 0 && importo <= saldo) {
                    saldo -= importo;
                    printf("Prelievo effettuato. Nuovo saldo: %.2f€\n", saldo);
                } else if (importo > saldo) {
                    printf("Saldo insufficiente\n");
                } else {
                    printf("Importo non valido\n");
                }
                break;
            
            case 4:
                printf("Arrivederci!\n");
                break;
            
            default:
                printf("Scelta non valida\n");
        }
    } while (scelta != 4);
    
    return 0;
}
```

---

## PARTE PRATICA (2 ore)

### ESERCIZI GUIDATI

#### Esercizio 1: switch Base (20 min)

**1.1** Menu semplice:

```c
#include <stdio.h>

int main() {
    int scelta;
    float a, b, risultato;

    printf("=== MENU SEMPLICE ===\n");
    printf("1. Saluta\n");
    printf("2. Esegui operazioni esempio (add, sub, mul)\n");
    printf("3. Esci\n");
    printf("Scelta: ");
    if (scanf("%d", &scelta) != 1) {
        printf("Input non valido\n");
        return 1;
    }

    switch (scelta) {
        case 1:
            printf("Ciao, mondo!\n");
            break;

        case 2:
            printf("Inserisci primo numero: ");
            if (scanf("%f", &a) != 1) { printf("Input non valido\n"); return 1; }
            printf("Inserisci secondo numero: ");
            if (scanf("%f", &b) != 1) { printf("Input non valido\n"); return 1; }

            printf("\n=== ESEMPI OPERAZIONI ===\n");
            risultato = a + b;
            printf("%.2f + %.2f = %.2f\n", a, b, risultato);

            risultato = a - b;
            printf("%.2f - %.2f = %.2f\n", a, b, risultato);

            risultato = a * b;
            printf("%.2f × %.2f = %.2f\n", a, b, risultato);
            break;

        case 3:
            printf("Uscita...\n");
            break;

        default:
            printf("Scelta non valida\n");
    }

    return 0;
}
```

#### Esercizio 2: Fall-through Intenzionale (20 min)

**2.1** Vocali e consonanti:

```c
#include <stdio.h>

int main() {
    char lettera;
    
    printf("Inserisci una lettera: ");
    scanf(" %c", &lettera);
    
    switch (lettera) {
        case 'a': case 'A':
        case 'e': case 'E':
        case 'i': case 'I':
        case 'o': case 'O':
        case 'u': case 'U':
            printf("'%c' è una VOCALE\n", lettera);
            break;
        
        default:
            if ((lettera >= 'a' && lettera <= 'z') ||
                (lettera >= 'A' && lettera <= 'Z')) {
                printf("'%c' è una CONSONANTE\n", lettera);
            } else {
                printf("'%c' non è una lettera\n", lettera);
            }
    }
    
    return 0;
}
```

**2.2** Giorni del mese:

```c
#include <stdio.h>

int main() {
    int mese;
    int giorni;
    
    printf("Mese (1-12): ");
    scanf("%d", &mese);
    
    switch (mese) {
        case 1:  // Gennaio
        case 3:  // Marzo
        case 5:  // Maggio
        case 7:  // Luglio
        case 8:  // Agosto
        case 10: // Ottobre
        case 12: // Dicembre
            giorni = 31;
            break;
        
        case 4:  // Aprile
        case 6:  // Giugno
        case 9:  // Settembre
        case 11: // Novembre
            giorni = 30;
            break;
        
        case 2:  // Febbraio
            giorni = 28;  // Semplificato
            break;
        
        default:
            printf("Mese non valido\n");
            return 1;
    }
    
    printf("Il mese %d ha %d giorni\n", mese, giorni);
    
    return 0;
}
```

**2.3** Classificazione carattere:

```c
#include <stdio.h>

int main() {
    char ch;
    
    printf("Inserisci carattere: ");
    scanf(" %c", &ch);
    
    switch (ch) {
        case '0': case '1': case '2': case '3': case '4':
        case '5': case '6': case '7': case '8': case '9':
            printf("È una CIFRA\n");
            break;
        
        case '+': case '-': case '*': case '/':
            printf("È un OPERATORE\n");
            break;
        
        case ' ': case '\t': case '\n':
            printf("È uno SPAZIO BIANCO\n");
            break;
        
        default:
            if ((ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z')) {
                printf("È una LETTERA\n");
            } else {
                printf("È un SIMBOLO SPECIALE\n");
            }
    }
    
    return 0;
}
```

---

#### Esercizio 3: Calcolatore Avanzato (25 min)

Sostituito blocco troncato con versione completa, usa math.h e gestisce input non valido e divisione per zero.

```c
#include <stdio.h>
#include <math.h>

int main() {
    int scelta;
    double a, b, risultato;

    printf("=== CALCOLATRICE AVANZATA ===\n");
    printf("1. Addizione\n");
    printf("2. Sottrazione\n");
    printf("3. Moltiplicazione\n");
    printf("4. Divisione\n");
    printf("5. Potenza (a^b)\n");
    printf("6. Radice quadrata (√a)\n");
    printf("7. Percentuale (b%% di a)\n");
    printf("Scegli operazione (1-7): ");

    if (scanf("%d", &scelta) != 1) {
        printf("Input non valido\n");
        return 1;
    }

    switch (scelta) {
        case 1:
            printf("Inserisci a e b: ");
            if (scanf("%lf %lf", &a, &b) != 2) { printf("Input non valido\n"); return 1; }
            risultato = a + b;
            printf("%.6g + %.6g = %.6g\n", a, b, risultato);
            break;

        case 2:
            printf("Inserisci a e b: ");
            if (scanf("%lf %lf", &a, &b) != 2) { printf("Input non valido\n"); return 1; }
            risultato = a - b;
            printf("%.6g - %.6g = %.6g\n", a, b, risultato);
            break;

        case 3:
            printf("Inserisci a e b: ");
            if (scanf("%lf %lf", &a, &b) != 2) { printf("Input non valido\n"); return 1; }
            risultato = a * b;
            printf("%.6g × %.6g = %.6g\n", a, b, risultato);
            break;

        case 4:
            printf("Inserisci dividendo e divisore: ");
            if (scanf("%lf %lf", &a, &b) != 2) { printf("Input non valido\n"); return 1; }
            if (b == 0.0) {
                printf("ERRORE: Divisione per zero!\n");
            } else {
                risultato = a / b;
                printf("%.6g ÷ %.6g = %.6g\n", a, b, risultato);
            }
            break;

        case 5:
            printf("Inserisci base e esponente: ");
            if (scanf("%lf %lf", &a, &b) != 2) { printf("Input non valido\n"); return 1; }
            risultato = pow(a, b);
            printf("%.6g ^ %.6g = %.6g\n", a, b, risultato);
            break;

        case 6:
            printf("Inserisci numero (a): ");
            if (scanf("%lf", &a) != 1) { printf("Input non valido\n"); return 1; }
            if (a < 0.0) {
                printf("ERRORE: radice di numero negativo\n");
            } else {
                risultato = sqrt(a);
                printf("√%.6g = %.6g\n", a, risultato);
            }
            break;

        case 7:
            printf("Inserisci valore e percentuale (a percent): ");
            if (scanf("%lf %lf", &a, &b) != 2) { printf("Input non valido\n"); return 1; }
            risultato = (a * b) / 100.0;
            printf("%.6g%% di %.6g = %.6g\n", b, a, risultato);
            break;

        default:
            printf("Operazione non valida\n");
    }

    return 0;
}
```

---

#### Esercizio 4: Conversioni (25 min)

**4.1** Convertitore temperature:

```c
#include <stdio.h>

int main() {
    int scelta;
    float temp, risultato;
    
    printf("=== CONVERTITORE TEMPERATURE ===\n");
    printf("1. Celsius → Fahrenheit\n");
    printf("2. Celsius → Kelvin\n");
    printf("3. Fahrenheit → Celsius\n");
    printf("4. Fahrenheit → Kelvin\n");
    printf("5. Kelvin → Celsius\n");
    printf("6. Kelvin → Fahrenheit\n");
    printf("\nScegli conversione: ");
    scanf("%d", &scelta);
    
    printf("Temperatura: ");
    scanf("%f", &temp);
    
    switch (scelta) {
        case 1:
            risultato = (temp * 9.0 / 5.0) + 32.0;
            printf("%.2f°C = %.2f°F\n", temp, risultato);
            break;
        
        case 2:
            risultato = temp + 273.15;
            printf("%.2f°C = %.2fK\n", temp, risultato);
            break;
        
        case 3:
            risultato = (temp - 32.0) * 5.0 / 9.0;
            printf("%.2f°F = %.2f°C\n", temp, risultato);
            break;
        
        case 4:
            risultato = (temp - 32.0) * 5.0 / 9.0 + 273.15;
            printf("%.2f°F = %.2fK\n", temp, risultato);
            break;
        
        case 5:
            risultato = temp - 273.15;
            printf("%.2fK = %.2f°C\n", temp, risultato);
            break;
        
        case 6:
            risultato = (temp - 273.15) * 9.0 / 5.0 + 32.0;
            printf("%.2fK = %.2f°F\n", temp, risultato);
            break;
        
        default:
            printf("Scelta non valida\n");
    }
    
    return 0;
}
```

**4.2** Convertitore numeri romani (1-10):

```c
#include <stdio.h>

int main() {
    int numero;
    
    printf("Inserisci numero (1-10): ");
    scanf("%d", &numero);
    
    printf("%d in romano: ", numero);
    
    switch (numero) {
        case 1:
            printf("I\n");
            break;
        case 2:
            printf("II\n");
            break;
        case 3:
            printf("III\n");
            break;
        case 4:
            printf("IV\n");
            break;
        case 5:
            printf("V\n");
            break;
        case 6:
            printf("VI\n");
            break;
        case 7:
            printf("VII\n");
            break;
        case 8:
            printf("VIII\n");
            break;
        case 9:
            printf("IX\n");
            break;
        case 10:
            printf("X\n");
            break;
        default:
            printf("Numero fuori range\n");
    }
    
    return 0;
}
```

---

#### Esercizio 5: Progetti Completi (30 min)

**5.1** Sistema di gestione studente:

```c
#include <stdio.h>

int main() {
    int scelta;
    char nome[50] = "Non impostato";
    int eta = 0;
    float media = 0.0;
    
    do {
        printf("\n=== GESTIONE STUDENTE ===\n");
        printf("1. Inserisci nome\n");
        printf("2. Inserisci età\n");
        printf("3. Inserisci media voti\n");
        printf("4. Visualizza dati\n");
        printf("5. Esci\n");
        printf("\nScelta: ");
        scanf("%d", &scelta);
        
        switch (scelta) {
            case 1:
                printf("Nome: ");
                scanf("%s", nome);
                printf("Nome salvato!\n");
                break;
            
            case 2:
                printf("Età: ");
                scanf("%d", &eta);
                if (eta > 0 && eta < 120) {
                    printf("Età salvata!\n");
                } else {
                    printf("Età non valida\n");
                    eta = 0;
                }
                break;
            
            case 3:
                printf("Media voti (0-30): ");
                scanf("%f", &media);
                if (media >= 0 && media <= 30) {
                    printf("Media salvata!\n");
                } else {
                    printf("Media non valida\n");
                    media = 0;
                }
                break;
            
            case 4:
                printf("\n=== DATI STUDENTE ===\n");
                printf("Nome: %s\n", nome);
                printf("Età: %d anni\n", eta);
                printf("Media: %.1f\n", media);
                
                if (media >= 18) {
                    printf("Esito: PROMOSSO\n");
                    if (media >= 27) {
                        printf("Qualifica: OTTIMO\n");
                    } else if (media >= 24) {
                        printf("Qualifica: BUONO\n");
                    } else {
                        printf("Qualifica: SUFFICIENTE\n");
                    }
                } else if (media > 0) {
                    printf("Esito: BOCCIATO\n");
                }
                break;
            
            case 5:
                printf("Uscita...\n");
                break;
            
            default:
                printf("Scelta non valida\n");
        }
    } while (scelta != 5);
    
    return 0;
}
```

**5.2** Gioco "Scelta Avventura":

```c
#include <stdio.h>

int main() {
    int scelta1, scelta2, scelta3;
    
    printf("=== AVVENTURA NEL CASTELLO ===\n\n");
    
    // Livello 1
    printf("Ti trovi davanti a un castello misterioso.\n");
    printf("1. Entra dalla porta principale\n");
    printf("2. Cerca un'entrata laterale\n");
    printf("3. Torna indietro\n");
    printf("\nScelta: ");
    scanf("%d", &scelta1);
    
    switch (scelta1) {
        case 1:
            printf("\nEntri dalla porta principale...\n");
            printf("Vedi due scale: una sale, una scende.\n");
            printf("1. Sali le scale\n");
            printf("2. Scendi le scale\n");
            printf("\nScelta: ");
            scanf("%d", &scelta2);
            
            switch (scelta2) {
                case 1:
                    printf("\nSali e trovi una stanza con un tesoro!\n");
                    printf("🏆 HAI VINTO!\n");
                    break;
                
                case 2:
                    printf("\nScendi e incontri un drago!\n");
                    printf("🐉 GAME OVER\n");
                    break;
                
                default:
                    printf("\nNon fai nulla e vieni catturato.\n");
                    printf("❌ GAME OVER\n");
            }
            break;
        
        case 2:
            printf("\nCerchi un'entrata laterale...\n");
            printf("Trovi una finestra aperta.\n");
            printf("1. Entra dalla finestra\n");
            printf("2. Continua a cercare\n");
            printf("\nScelta: ");
            scanf("%d", &scelta3);
            
            switch (scelta3) {
                case 1:
                    printf("\nEntri dalla finestra in una cucina.\n");
                    printf("Trovi cibo e una mappa segreta!\n");
                    printf("🗺️ HAI VINTO!\n");
                    break;
                
                case 2:
                    printf("\nContinui a cercare ma si fa buio.\n");
                    printf("Ti perdi nel bosco.\n");
                    printf("🌲 GAME OVER\n");
                    break;
                
                default:
                    printf("\nIndecisione fatale!\n");
                    printf("❌ GAME OVER\n");
            }
            break;
        
        case 3:
            printf("\nTorni indietro... saggia decisione!\n");
            printf("🏃 Sei salvo, ma niente avventura.\n");
            break;
        
        default:
            printf("\nNon fai una scelta valida.\n");
            printf("Un guardiano ti cattura!\n");
            printf("⚔️ GAME OVER\n");
    }
    
    return 0;
}
```

**5.3** Menu ristorante con ordine:

```c
#include <stdio.h>

int main() {
    int antipasto = 0, primo = 0, secondo = 0, dolce = 0;
    int scelta;
    float totale = 0.0;
    
    printf("=== RISTORANTE DA MARIO ===\n\n");
    
    // Antipasto
    printf("ANTIPASTI:\n");
    printf("1. Bruschette (5€)\n");
    printf("2. Prosciutto e melone (8€)\n");
    printf("3. Nessun antipasto\n");
    printf("Scelta: ");
    scanf("%d", &antipasto);
    
    switch (antipasto) {
        case 1:
            printf("Hai scelto: Bruschette\n");
            totale += 5.0;
            break;
        case 2:
            printf("Hai scelto: Prosciutto e melone\n");
            totale += 8.0;
            break;
        case 3:
            printf("Nessun antipasto\n");
            break;
        default:
            printf("Scelta non valida, salto...\n");
    }
    
    // Primo
    printf("\nPRIMI PIATTI:\n");
    printf("1. Spaghetti al pomodoro (10€)\n");
    printf("2. Risotto ai funghi (12€)\n");
    printf("3. Lasagne (11€)\n");
    printf("Scelta: ");
    scanf("%d", &primo);
    
    switch (primo) {
        case 1:
            printf("Hai scelto: Spaghetti al pomodoro\n");
            totale += 10.0;
            break;
        case 2:
            printf("Hai scelto: Risotto ai funghi\n");
            totale += 12.0;
            break;
        case 3:
            printf("Hai scelto: Lasagne\n");
            totale += 11.0;
            break;
        default:
            printf("Scelta non valida, salto...\n");
    }
    
    // Secondo
    printf("\nSECONDI PIATTI:\n");
    printf("1. Bistecca (15€)\n");
    printf("2. Pesce al forno (18€)\n");
    printf("3. Pollo arrosto (13€)\n");
    printf("4. Nessun secondo\n");
    printf("Scelta: ");
    scanf("%d", &secondo);
    
    switch (secondo) {
        case 1:
            printf("Hai scelto: Bistecca\n");
            totale += 15.0;
            break;
        case 2:
            printf("Hai scelto: Pesce al forno\n");
            totale += 18.0;
            break;
        case 3:
            printf("Hai scelto: Pollo arrosto\n");
            totale += 13.0;
            break;
        case 4:
            printf("Nessun secondo\n");
            break;
        default:
            printf("Scelta non valida, salto...\n");
    }
    
    // Dolce
    printf("\nDOLCI:\n");
    printf("1. Tiramisù (6€)\n");
    printf("2. Panna cotta (5€)\n");
    printf("3. Gelato (4€)\n");
    printf("4. Nessun dolce\n");
    printf("Scelta: ");
    scanf("%d", &dolce);
    
    switch (dolce) {
        case 1:
            printf("Hai scelto: Tiramisù\n");
            totale += 6.0;
            break;
        case 2:
            printf("Hai scelto: Panna cotta\n");
            totale += 5.0;
            break;
        case 3:
            printf("Hai scelto: Gelato\n");
            totale += 4.0;
            break;
        case 4:
            printf("Nessun dolce\n");
            break;
        default:
            printf("Scelta non valida, salto...\n");
    }
    
    // Conto finale
    printf("\n=== CONTO ===\n");
    printf("Totale: %.2f€\n", totale);
    
    if (totale > 50) {
        float sconto = totale * 0.10;
        printf("Sconto 10%%: -%.2f€\n", sconto);
        printf("Totale finale: %.2f€\n", totale - sconto);
    }
    
    printf("\nGrazie e buon appetito!\n");
    
    return 0;
}
```

---

### ESERCIZI AUTONOMI

#### Esercizio 6: Pratica Libera

**6.1** Crea un convertitore unità di misura (lunghezza, peso, volume).

**6.2** Sistema di voto elettorale: chiedi partito (1-5) e conta voti.

**6.3** Gioco "Carta più alta": confronta carte (1-13 = Asso-Re).

**6.4** Classificatore animali: mammifero/uccello/pesce/rettile con caratteristiche.

**6.5** Menu gestione libreria (aggiungi/cerca/rimuovi libri - simulato).

---

### ESERCIZI DA SVOLGERE A CASA

**Homework 1:** Convertitore valute completo:
- Menu con 5+ valute
- Conversioni bidirezionali
- Tassi di cambio hardcoded
- Visualizza tabella conversioni

**Homework 2:** Gioco battaglia navale (singola cella):
- Griglia 5×5 (25 posizioni)
- Utente sceglie posizione (1-25)
- Switch determina se colpito/mancato
- 3 tentativi
- Conta colpi e mancati

**Homework 3:** Sistema di prenotazione cinema:
- Menu film (1-5)
- Menu orario (mattina/pomeriggio/sera)
- Menu tipo biglietto (intero/ridotto/famiglia)
- Calcola prezzo totale con sconti
- Conferma prenotazione

**Homework 4:** Calcolatore IMC con consigli:
- Chiedi peso e altezza
- Calcola IMC
- Usa switch su decine di IMC
- Fornisci consigli personalizzati per categoria

**Homework 5:** Gioco "Indovina l'animale":
- Switch su caratteristiche (ha ali? ha pelliccia? vive in acqua?)
- Catena di domande
- Indovina animale in base a risposte

**Homework 6:** Traduttore numeri (1-100):
- Chiedi numero
- Traduci in italiano a parole
- Gestisci unità, decine, centinaia
- Usa switch multipli

---

## APPROFONDIMENTI

### Jump Table (Ottimizzazione Compilatore)

Quando usi switch con molti case consecutivi, il compilatore può generare una **jump table** invece di confronti sequenziali.

**Jump Table:**
```c
switch (x) {
    case 0: /* ... */ break;
    case 1: /* ... */ break;
    case 2: /* ... */ break;
    // ...
    case 99: /* ... */ break;
}
```

Il compilatore crea:
```
indirizzo_base + (x * sizeof(puntatore)) → salta direttamente al case
```

**Complessità:**
- if-else sequenziali: O(n) - n confronti nel caso peggiore
- Jump table: O(1) - un solo calcolo indirizzo

**Quando viene usata:**
- Case numericamente vicini (0-9, 1-10, etc.)
- Molti case (tipicamente 10+)
- Case consecutivi o con pochi buchi

### Duff's Device (Trick Avanzato)

Un uso estremamente avanzato (e raro) di switch con cicli:

```c
// NON usare in codice normale! Solo curiosità storica
void copy(char *to, char *from, int count) {
    int n = (count + 7) / 8;
    switch (count % 8) {
        case 0: do { *to++ = *from++;
        case 7:      *to++ = *from++;
        case 6:      *to++ = *from++;
        case 5:      *to++ = *from++;
        case 4:      *to++ = *from++;
        case 3:      *to++ = *from++;
        case 2:      *to++ = *from++;
        case 1:      *to++ = *from++;
               } while (--n > 0);
    }
}
```

**Perché funziona:** Fall-through intenzionale + loop unrolling.
**Usa moderno:** Compilatori ottimizzano automaticamente, non serve!

### Computed Goto (GCC Extension)

GCC supporta etichette come valori:

```c
void *labels[] = {&&case0, &&case1, &&case2};
goto *labels[x];  // Salta a etichetta calcolata

case0:
    printf("Case 0\n");
    goto end;
case1:
    printf("Case 1\n");
    goto end;
case2:
    printf("Case 2\n");
end:
    ;
```

Ancora più veloce di switch, ma **non portabile** (solo GCC/Clang).

### Enum con switch

**Ottima pratica:** Usa enum invece di numeri magici.

```c
enum Giorno {
    LUNEDI = 1,
    MARTEDI,
    MERCOLEDI,
    GIOVEDI,
    VENERDI,
    SABATO,
    DOMENICA
};

enum Giorno oggi = MERCOLEDI;

switch (oggi) {
    case LUNEDI:
        printf("Inizio settimana\n");
        break;
    case VENERDI:
        printf("Quasi weekend!\n");
        break;
    case SABATO:
    case DOMENICA:
        printf("Weekend!\n");
        break;
    default:
        printf("Giorno feriale\n");
}
```

**Vantaggi:**
- Codice auto-documentato
- Compilatore avvisa se manca un case
- Refactoring più facile

---

## RIEPILOGO CONCETTI CHIAVE

### Sintassi switch
```c
switch (espressione) {
    case valore1:
        // codice
        break;
    case valore2:
        // codice
        break;
    default:
        // codice
}
```

### Regole Fondamentali
- Espressione: **int** o **char** (no float)
- case: solo **costanti** (no variabili)
- break: **necessario** per uscire
- default: **opzionale** ma raccomandato

### Fall-through
```c
case 1:
case 2:
case 3:
    // Eseguito per 1, 2 o 3
    break;
```

---

## TABELLE DI RIFERIMENTO RAPIDO

### switch vs if-else

| Caratteristica | switch | if-else |
|----------------|--------|---------|
| Tipo supportato | int, char, enum | Qualsiasi |
| Condizione | Solo == | Qualsiasi |
| Range | No (workaround) | Sì |
| Float | No | Sì |
| Leggibilità (molti case) | Alta | Bassa |
| Performance (molti case) | Alta (jump table) | Media |

### Quando Usare Cosa

| Scenario | Usa |
|----------|-----|
| Menu numerico | switch |
| Singolo carattere | switch |
| Range di valori | if-else |
| Condizioni complesse | if-else |
| Float comparison | if-else |
| 2-3 alternative | if-else |
| 5+ alternative discrete | switch |

---

## PROBLEMI COMUNI E SOLUZIONI

### Problema 1: Dimenticare break

```c
// ❌ MALE
switch (x) {
    case 1:
        printf("Uno\n");
    case 2:  // Esegue anche questo!
        printf("Due\n");
        break;
}
```

**Soluzione:** Aggiungi sempre break (o commenta fall-through).

### Problema 2: Variabile nel case

```c
int limite = 10;
switch (x) {
    case limite:  // ❌ ERRORE! Non compila
        break;
}
```

**Soluzione:** Usa #define o enum.
```c
#define LIMITE 10
switch (x) {
    case LIMITE:  // ✓ OK
        break;
}
```

### Problema 3: Range nel case

```c
switch (voto) {
    case 90-100:  // ❌ ERRORE! Sintassi non valida
        printf("A\n");
        break;
}
```

**Soluzione:** Usa if-else o case multipli.
```c
if (voto >= 90 && voto <= 100) {
    printf("A\n");
} else if (voto >= 80 && voto < 90) {
    printf("B\n");
}
// ...
```

### Problema 4: Float in switch

```c
float x = 3.14;
switch (x) {  // ❌ ERRORE! Float non supportato
    // ...
}
```

**Soluzione:** Usa if-else.

### Problema 5: case Duplicati

```c
switch (x) {
    case 1:
        break;
    case 1:  // ❌ ERRORE! Duplicato
        break;
}
```

**Soluzione:** Ogni case deve essere unico.

---

## PUNTI CHIAVE DELLA LEZIONE

✓ **switch** ottimo per **selezione multipla** con valori discreti  
✓ Supporta solo **int, char, enum** (no float)  
✓ **break** necessario per uscire (altrimenti fall-through)  
✓ **case** solo con **costanti** (no variabili)  
✓ **default** gestisce casi non previsti  
✓ **Fall-through** utile per case multipli stessa azione  
✓ **Più leggibile** di molti if-else per menu/opzioni  
✓ **Jump table** rende switch O(1) vs if-else O(n)  
✓ **Commenta** fall-through intenzionali  
✓ **Usa enum** per codice auto-documentato

---

## PREPARAZIONE PER LA PROSSIMA LEZIONE

Nella prossima lezione studieremo:
- **Ciclo while**: iterazione con condizione iniziale
- **Ciclo do-while**: iterazione con condizione finale
- **Differenze while vs do-while**
- **Cicli infiniti e break**
- **Continue**: salta iterazione corrente
- **Cicli annidati**

Porta con te:
- Esercizi homework completati
- Domande su switch o fall-through
- Esempi di operazioni ripetitive
- Idee per programmi con cicli

---

**Eccellente! Ora padroneggi la selezione multipla! 🎯**

Con if-else e switch hai tutti gli strumenti per gestire decisioni semplici e complesse. Nella prossima lezione aggiungeremo i **cicli** per ripetere operazioni, completando così i tre costrutti fondamentali della programmazione strutturata (sequenza, selezione, iterazione).
