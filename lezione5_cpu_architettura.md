**Trend moderno:** Ibrido - CPU CISC (x86) con core RISC interno!

### Pipeline della CPU

La **pipeline** permette di eseguire più istruzioni contemporaneamente, sovrapponendo le fasi.

**Pipeline classica a 5 stadi:**
```
Stadio 1: IF  (Instruction Fetch)
Stadio 2: ID  (Instruction Decode)
Stadio 3: EX  (Execute)
Stadio 4: MEM (Memory Access)
Stadio 5: WB  (Write Back)
```

**Esecuzione senza pipeline:**
```
Tempo:  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15
Istr1: [IF][ID][EX][ME][WB]
Istr2:                    [IF][ID][EX][ME][WB]
Istr3:                                      [IF][ID][EX][ME][WB]

Tempo totale: 15 cicli per 3 istruzioni
```

**Esecuzione con pipeline:**
```
Tempo:  1  2  3  4  5  6  7  8  9
Istr1: [IF][ID][EX][ME][WB]
Istr2:    [IF][ID][EX][ME][WB]
Istr3:       [IF][ID][EX][ME][WB]

Tempo totale: 7 cicli per 3 istruzioni
Speedup teorico: 15/7 = 2.14×
```

**Problemi (Hazard):**

1. **Data Hazard:** Istruzione dipende dal risultato della precedente
```assembly
ADD R1, R2, R3    ; R1 = R2 + R3
SUB R4, R1, R5    ; R4 = R1 - R5 (dipende da R1!)
```
Soluzione: Forwarding (bypass), Stall (attesa)

2. **Control Hazard:** Salti modificano il flusso
```assembly
JMP LABEL         ; Quale istruzione caricare dopo?
```
Soluzione: Branch prediction (predizione salti)

3. **Structural Hazard:** Conflitto risorse hardware
```
Due istruzioni vogliono accedere alla memoria contemporaneamente
```
Soluzione: Cache separata per istruzioni e dati

**Branch Prediction:**
- CPU "indovina" se un salto verrà preso
- Se indovina giusto: nessun ritardo
- Se sbaglia: deve svuotare la pipeline (penalty 10-20 cicli)
- Accuratezza moderna: >95%

### Architetture Multi-Core

**Single-Core (tradizionale):**
```
┌──────────────┐
│     CPU      │
│   ┌────┐     │
│   │Core│     │
│   └────┘     │
└──────────────┘
```

**Multi-Core (moderno):**
```
┌──────────────────────────┐
│         CPU              │
│  ┌────┐  ┌────┐  ┌────┐ │
│  │Core│  │Core│  │Core│ │
│  │ 1  │  │ 2  │  │ 3  │ │
│  └────┘  └────┘  └────┘ │
│         ┌────┐           │
│         │Cache│          │
│         │ L3  │ (shared) │
│         └────┘           │
└──────────────────────────┘
```

**Vantaggi:**
- Più thread paralleli
- Maggiore throughput
- Minore consumo energetico (vs alta frequenza)

**Sfide:**
- Software deve essere parallelizzato
- Sincronizzazione tra core
- Coerenza cache (cache coherence)

### Hyper-Threading / SMT

**SMT (Simultaneous Multi-Threading):**
Un core fisico appare come 2 core logici al sistema operativo.

```
Core Fisico:
┌────────────────────────┐
│   ALU    │   ALU       │
│  ┌────┐  │  ┌────┐    │
│  │Reg │  │  │Reg │    │ ← 2 set di registri
│  │Set1│  │  │Set2│    │
│  └────┘  │  └────┘    │
└────────────────────────┘
  Thread 1    Thread 2
```

**Beneficio:** ~30% prestazioni in più con stesso hardware

### Spectre e Meltdown

**Vulnerabilità scoperte nel 2018** che sfruttano l'esecuzione speculativa.

**Esecuzione Speculativa:**
```assembly
if (x < array_size) {
    y = array[x];    ; CPU esegue PRIMA di verificare condizione!
}
```

La CPU esegue il codice "speculativamente" (in anticipo) per guadagnare tempo.

**Problema:** Se la condizione è falsa, il codice eseguito viene annullato MA le tracce rimangono nella cache!

**Attacco:** Leggere queste tracce permette di rubare dati sensibili (password, chiavi crittografiche).

**Mitigazione:** 
- Aggiornamenti microcodice CPU
- Patch sistema operativo
- Costo: ~5-30% prestazioni perse

### Consumo Energetico

**Formula potenza dinamica:**
```
P = C × V² × f

Dove:
C = capacitanza circuito
V = tensione
f = frequenza clock
```

**Problema:** Potenza cresce con il QUADRATO della tensione!

**Esempio:**
```
CPU a 1.0V, 2 GHz: P = C × 1.0² × 2 = 2C
CPU a 1.2V, 3 GHz: P = C × 1.2² × 3 = 4.32C

Aumento: 116% consumo per 50% prestazioni!
```

**Soluzione moderna:** DVFS (Dynamic Voltage and Frequency Scaling)
- Abbassa tensione/frequenza quando non serve prestazioni
- Power management intelligente

### Il Futuro: Quantum Computing

**Computer Quantistico:**
- Usa **qubit** invece di bit
- Qubit può essere 0, 1, o ENTRAMBI (sovrapposizione)
- Calcoli paralleli massicci

**Differenza fondamentale:**
```
Computer Classico:
bit = 0 o 1
8 bit = uno stato tra 256 possibili

Computer Quantistico:
qubit = 0 E 1 simultaneamente
8 qubit = tutti i 256 stati CONTEMPORANEAMENTE!
```

**Applicazioni:**
- Crittografia (rompere RSA in minuti)
- Simulazioni molecolari
- Ottimizzazione
- Machine learning

**Sfide:**
- Qubit estremamente fragili (decoerenza)
- Temperatura: vicino allo zero assoluto (-273°C)
- Correzione errori complessa
- Programmazione completamente diversa

**Stato attuale (2023):**
- IBM: 433 qubit
- Google: 70 qubit con correzione errori
- Ancora prototipi, non commerciali

---

## RIEPILOGO CONCETTI CHIAVE

### Architettura Von Neumann
```
┌─────────┐     ┌──────────┐
│   CPU   │◄───►│ MEMORIA  │
│ CU│ALU  │     │PROG+DATI │
└────┬────┘     └──────────┘
     │
  ┌──┴──┐
  │ I/O │
  └─────┘
```

### Componenti CPU
```
CPU = Control Unit + ALU + Registri

Registri speciali:
- PC: Program Counter (prossima istruzione)
- IR: Instruction Register (istruzione corrente)
- MAR: Memory Address Register
- MDR: Memory Data Register
- ACC: Accumulator
- PSW: Program Status Word (flag)
```

### Ciclo Fetch-Decode-Execute
```
1. FETCH:   PC → MAR → MDR → IR, PC++
2. DECODE:  IR → Control Unit → segnali controllo
3. EXECUTE: Esegui operazione (ALU, memoria, salto)

Ripeti all'infinito (fino a HALT)
```

### Gerarchia Memoria
```
Velocità ▲         Costo/Byte ▲      Capacità ▼

Registri (0.25 ns)
Cache L1 (1 ns)
Cache L2 (5 ns)
Cache L3 (15 ns)
RAM (100 ns)
SSD (100 μs)
HDD (10 ms)
```

---

## TABELLE DI RIFERIMENTO RAPIDO

### Tempi di Accesso Tipici

| Operazione | Latenza | Analogia |
|------------|---------|----------|
| Accesso registro | 0.25 ns | 1 sec |
| Cache L1 | 1 ns | 4 sec |
| Cache L2 | 4 ns | 16 sec |
| Cache L3 | 15 ns | 1 min |
| RAM | 100 ns | 6 min |
| SSD read | 100 μs | 4 giorni |
| HDD seek | 10 ms | 1 anno |
| Internet USA | 100 ms | 10 anni |

### Instruction Set (Esempio Semplificato)

| Categoria | Istruzione | Sintassi | Cicli |
|-----------|------------|----------|-------|
| Transfer | LOAD | LOAD reg, [addr] | 3 |
| Transfer | STORE | STORE [addr], reg | 3 |
| Transfer | MOV | MOV reg1, reg2 | 1 |
| Arithmetic | ADD | ADD reg1, reg2 | 1-2 |
| Arithmetic | SUB | SUB reg1, reg2 | 1-2 |
| Arithmetic | MUL | MUL reg1, reg2 | 3-10 |
| Arithmetic | DIV | DIV reg1, reg2 | 10-40 |
| Logic | AND | AND reg1, reg2 | 1 |
| Logic | OR | OR reg1, reg2 | 1 |
| Logic | NOT | NOT reg | 1 |
| Control | JMP | JMP addr | 2 |
| Control | JZ | JZ addr | 2 |
| Control | CALL | CALL addr | 3-5 |
| Control | RET | RET | 3-5 |

### Flag Register (PSW)

| Flag | Nome | Significato |
|------|------|-------------|
| Z | Zero | Risultato = 0 |
| N | Negative | Risultato < 0 |
| C | Carry | Riporto/prestito |
| V | Overflow | Overflow aritmetico |
| I | Interrupt | Interrupt abilitati |
| S | Sign | Bit di segno |
| P | Parity | Parità (conteggio bit) |

---

## PROBLEMI COMUNI E SOLUZIONI

### Problema 1: Collo di Bottiglia Von Neumann

**Sintomo:** CPU aspetta memoria
```
CPU velocità: 3 GHz
RAM velocità: 100 ns = 30 cicli!
CPU spende 90% tempo in attesa!
```

**Soluzioni:**
- **Cache**: memorie veloci vicine a CPU (L1, L2, L3)
- **Prefetching**: carica dati in anticipo
- **Out-of-order execution**: esegue altre istruzioni durante l'attesa

### Problema 2: Cache Miss

**Sintomo:** Prestazioni degradate

**Cause:**
- Accessi casuali a memoria
- Array troppo grandi per cache
- False sharing (multi-thread)

**Soluzioni:**
- Ottimizza ordine accessi (loop tiling)
- Usa strutture dati cache-friendly
- Padding per evitare false sharing

### Problema 3: Branch Misprediction

**Sintomo:** Pipeline svuotata frequentemente

```c
// Codice problematico
for (int i = 0; i < n; i++) {
    if (random() > 0.5) {  // Imprevedibile!
        sum += array[i];
    }
}
```

**Soluzioni:**
- Evita branch se possibile (branchless programming)
- Rendi pattern predicibili
- Usa conditional move (CMOV)

```c
// Versione branchless
for (int i = 0; i < n; i++) {
    int mask = -(array[i] > threshold);
    sum += array[i] & mask;
}
```

---

## DOMANDE DI AUTOVALUTAZIONE

**Rispondi senza guardare gli appunti:**

1. Quali sono i 5 componenti dell'architettura Von Neumann?
2. Qual è il ruolo del Program Counter (PC)?
3. Cosa fa l'ALU?
4. Quali sono le 3 fasi del ciclo FDE?
5. Cosa contiene l'Instruction Register (IR)?
6. Perché la cache è importante?
7. Qual è la differenza tra RISC e CISC?
8. Cosa significa "pipeline" nella CPU?
9. Cos'è un data hazard?
10. Come funziona il branch prediction?

**Risposte rapide:**
1. CPU (CU + ALU), Memoria, Input, Output, Bus
2. Contiene indirizzo prossima istruzione da eseguire
3. Esegue operazioni aritmetiche e logiche
4. Fetch (preleva), Decode (decodifica), Execute (esegue)
5. L'istruzione corrente in esecuzione
6. Velocizza accessi memoria (località temporale/spaziale)
7. RISC: poche istruzioni semplici; CISC: molte istruzioni complesse
8. Esecuzione sovrapposta di più istruzioni
9. Dipendenza dati tra istruzioni consecutive
10. CPU "indovina" se un salto sarà preso o no

---

## PUNTI CHIAVE DELLA LEZIONE

✓ **Von Neumann**: programma e dati nella stessa memoria  
✓ **CPU = CU + ALU + Registri**: cuore computazionale  
✓ **Ciclo FDE**: fetch → decode → execute (si ripete)  
✓ **PC** punta alla prossima istruzione, **IR** contiene quella corrente  
✓ **Gerarchia memoria**: velocità vs capacità (registri → RAM → disco)  
✓ **Cache** sfrutta località (temporale e spaziale)  
✓ **Assembly**: linguaggio a basso livello, vicino all'hardware  
✓ **Pipeline**: parallelismo a livello istruzione  
✓ **Branch prediction**: cruciale per prestazioni moderne  
✓ **Multi-core**: parallelismo a livello thread/processo

---

## PREPARAZIONE PER LA PROSSIMA LEZIONE

Nella prossima lezione inizieremo finalmente a programmare in C!

Studieremo:
- **Primo programma in C**: Hello World
- **Struttura programma**: include, main, return
- **Compilazione**: preprocessore, compiler, linker
- **Printf**: output formattato
- **Commenti**: documentare il codice

Porta con te:
- Ambiente C installato e funzionante (GCC, IDE)
- Esercizi homework completati
- Domande su architettura o assembly
- Curiosità sulla programmazione in C

**Verifica preparazione:**
- Compila e esegui un programma C di test
- Familiarizza con l'IDE/editor scelto
- Rivedi i concetti di variabile e tipo di dato

---

## GLOSSARIO TECNICO

- **ALU**: Arithmetic Logic Unit, esegue calcoli
- **Assembler**: Traduce assembly in linguaggio macchina
- **Branch**: Salto/deviazione nel flusso del programma
- **Bus**: Canali di comunicazione tra componenti
- **Cache**: Memoria veloce vicino alla CPU
- **CISC**: Complex Instruction Set Computer
- **Clock**: Segnale che sincronizza operazioni CPU
- **Control Unit (CU)**: Coordina esecuzione istruzioni
- **Decode**: Interpretazione istruzione
- **Execute**: Esecuzione effettiva istruzione
- **Fetch**: Prelievo istruzione da memoria
- **Hazard**: Conflitto nella pipeline
- **Instruction Set**: Insieme di istruzioni che CPU comprende
- **Interrupt**: Segnale che interrompe esecuzione normale
- **Latency**: Tempo per completare un'operazione
- **Opcode**: Codice operazione (ADD, SUB, MOV, etc.)
- **PC (Program Counter)**: Registro con indirizzo prossima istruzione
- **Pipeline**: Tecnica per eseguire più istruzioni in parallelo
- **RISC**: Reduced Instruction Set Computer
- **Register**: Memoria velocissima dentro CPU
- **Throughput**: Quantità operazioni per unità tempo
- **Von Neumann**: Architettura con programma in memoria

---

## PROGETTO FINALE LEZIONE 5

**Mini-Progetto: Simulatore CPU Semplificato**

Scrivi un programma (in qualsiasi linguaggio o pseudocodice) che simula una CPU semplificata con:

**Specifiche:**
- 4 registri a 8 bit: A, B, C, D
- 256 byte di memoria (0x00-0xFF)
- Program Counter (PC)
- Flag: Z (zero), N (negative)

**Instruction Set:**
```
LOAD reg, [addr]   - Carica da memoria
STORE [addr], reg  - Salva in memoria
MOV reg1, reg2     - Copia registro
ADD reg1, reg2     - Addizione
SUB reg1, reg2     - Sottrazione
JMP addr           - Salto incondizionato
JZ addr            - Salta se zero
HALT               - Ferma
```

**Funzionalità:**
1. Carica programma in memoria
2. Esegui ciclo FDE
3. Mostra stato dopo ogni istruzione (registri, PC, flag)
4. Conta cicli eseguiti
5. Mostra memoria finale

**Programma di test:**
```assembly
0x00: LOAD A, [0x50]    ; A = 10
0x02: LOAD B, [0x51]    ; B = 5
0x04: ADD A, B          ; A = 15
0x06: STORE [0x52], A   ; MEM[0x52] = 15
0x08: SUB A, B          ; A = 10
0x0A: SUB A, A          ; A = 0
0x0C: JZ 0x10           ; Salta (Z=1)
0x0E: LOAD C, [0x53]    ; NON eseguita
0x10: HALT

Dati:
MEM[0x50] = 10
MEM[0x51] = 5
```

**Output atteso:**
```
Ciclo 1: LOAD A, [0x50]
  Registri: A=10, B=0, C=0, D=0
  PC=0x02, Flag: Z=0, N=0

Ciclo 2: LOAD B, [0x51]
  Registri: A=10, B=5, C=0, D=0
  PC=0x04, Flag: Z=0, N=0

... (continua)

Ciclo 7: JZ 0x10
  Salto effettuato (Z=1)
  PC=0x10

Ciclo 8: HALT
  Programma terminato

Statistiche:
- Cicli totali: 8
- Istruzioni eseguite: 8
- Salti effettuati: 1

Memoria finale:
0x52: 15
```

**Estensioni (opzionali):**
- Aggiungi modalità di indirizzamento (immediato, indiretto)
- Implementa uno stack e istruzioni PUSH/POP
- Aggiungi più operazioni (AND, OR, XOR, shift)
- Visualizza grafico timeline esecuzione
- Simula cache semplice

---

## RISORSE AGGIUNTIVE

### Simulatori Online

**CPU Simulators:**
- **Little Man Computer**: https://peterhigginson.co.uk/LMC/
  Simulatore didattico semplice, ottimo per principianti

- **MARIE Simulator**: Simulatore architettura base
  Usato in molti corsi universitari

- **Logisim**: Simula circuiti digitali
  https://sourceforge.net/projects/circuit/

- **CPUlator**: https://cpulator.01xz.net/
  Simula ARM, MIPS, Nios II

### Video e Tutorial

- **Crash Course Computer Science**: Serie YouTube eccellente
- **Ben Eater**: Costruisce computer da zero (breadboard)
- **Computerphile**: Video su architetture avanzate

### Libri Consigliati

- "Computer Organization and Design" (Patterson & Hennessy)
  La "bibbia" dell'architettura dei computer
  
- "The Elements of Computing Systems" (Nisan & Schocken)
  Costruisci un computer da zero (progetto Nand2Tetris)

### Progetti Pratici

- **Nand2Tetris**: Costruisci computer completo da porte logiche
  https://www.nand2tetris.org/

- **RISC-V**: Impara architettura RISC moderna
  Documentazione gratuita, simulatori disponibili

---

**Complimenti! Ora conosci come funziona un computer "dentro"! 🎯**

Dalla prossima lezione inizieremo a programmare in C, mettendo in pratica tutti questi concetti teorici.

Il viaggio dalla teoria (architettura) alla pratica (programmazione) inizia ora!# LEZIONE 5 - CPU e Architettura del Computer: Ciclo Fetch-Decode-Execute
**Durata: 4 ore | Teoria: 2h | Esercizi: 2h**

---

## PARTE TEORICA (2 ore)

### 5.1 Introduzione all'Architettura del Computer (15 min)

#### Cos'è l'Architettura del Computer?

L'**architettura del computer** descrive la struttura e l'organizzazione dei componenti hardware e come interagiscono tra loro per eseguire programmi.

**Analogia:** Se il computer fosse una città:
- **CPU** = municipio (cervello decisionale)
- **RAM** = biblioteca (memoria di lavoro)
- **Hard Disk** = archivio storico (memoria permanente)
- **Bus** = strade (trasporto dati)
- **I/O** = porte della città (comunicazione con esterno)

#### Componenti Principali

```
┌─────────────────────────────────────────┐
│            COMPUTER SYSTEM              │
├─────────────────────────────────────────┤
│                                         │
│  ┌──────────┐         ┌─────────────┐  │
│  │   CPU    │◄───────►│   MEMORIA   │  │
│  │          │         │    (RAM)    │  │
│  └─────┬────┘         └─────────────┘  │
│        │                                │
│        │ BUS DI SISTEMA                 │
│        │                                │
│  ┌─────┴──────────────────────────┐    │
│  │     INPUT/OUTPUT DEVICES       │    │
│  │ Tastiera, Mouse, Monitor, ecc. │    │
│  └────────────────────────────────┘    │
│                                         │
└─────────────────────────────────────────┘
```

**1. CPU (Central Processing Unit):**
- Esegue le istruzioni
- Effettua calcoli e prende decisioni
- Coordina tutte le operazioni

**2. Memoria:**
- **RAM** (Random Access Memory): memoria principale, veloce, volatile
- **ROM** (Read-Only Memory): memoria permanente, contiene BIOS
- **Cache**: memoria ultra-veloce vicino alla CPU
- **Hard Disk/SSD**: memoria di massa, permanente, lenta

**3. Bus:**
- **Bus dati**: trasporta i dati
- **Bus indirizzi**: specifica dove leggere/scrivere
- **Bus controllo**: segnali di sincronizzazione

**4. Dispositivi I/O:**
- Input: tastiera, mouse, scanner, microfono
- Output: monitor, stampante, altoparlanti
- Input/Output: hard disk, scheda di rete, USB

---

### 5.2 Architettura di Von Neumann (20 min)

#### Storia

Proposta da **John von Neumann** nel 1945 per il computer EDVAC. È l'architettura base della maggior parte dei computer moderni.

#### Principi Fondamentali

**1. Stored Program Concept:**
- Programmi e dati sono memorizzati nella stessa memoria
- Le istruzioni sono trattate come dati
- Un programma può modificare se stesso (pericoloso ma potente!)

**2. Esecuzione Sequenziale:**
- Le istruzioni vengono eseguite una dopo l'altra
- Un contatore (Program Counter) tiene traccia della prossima istruzione

**3. Architettura a 5 Componenti:**

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  ┌─────────────┐              ┌─────────────────┐  │
│  │   UNITÀ DI  │              │     UNITÀ DI    │  │
│  │   CONTROLLO │◄────────────►│   ARITMETICO-   │  │
│  │    (CU)     │              │     LOGICA      │  │
│  └──────┬──────┘              │      (ALU)      │  │
│         │                     └─────────────────┘  │
│         │                              ▲           │
│         │                              │           │
│         ▼                              │           │
│  ┌─────────────────────────────────────┴────────┐  │
│  │              MEMORIA CENTRALE                │  │
│  │    (Programmi + Dati nello stesso spazio)   │  │
│  └──────┬────────────────────────────────┬──────┘  │
│         │                                │          │
│         ▼                                ▼          │
│  ┌────────────┐                  ┌────────────┐    │
│  │   INPUT    │                  │   OUTPUT   │    │
│  │  DEVICES   │                  │  DEVICES   │    │
│  └────────────┘                  └────────────┘    │
│                                                     │
└─────────────────────────────────────────────────────┘
```

#### Componenti nel Dettaglio

**1. Unità di Controllo (Control Unit - CU):**
- Coordina tutte le operazioni
- Decodifica le istruzioni
- Genera segnali di controllo
- Gestisce il flusso dati tra componenti

**2. Unità Aritmetico-Logica (ALU):**
- Esegue operazioni aritmetiche (+, -, ×, ÷)
- Esegue operazioni logiche (AND, OR, NOT, XOR)
- Esegue confronti (<, >, =, ≠)
- Gestisce shift e rotazioni di bit

**3. Memoria Centrale:**
- Memorizza istruzioni e dati
- Organizzata in celle con indirizzi univoci
- Accesso casuale (Random Access)
- Volatile (perde dati senza alimentazione)

**4. Dispositivi I/O:**
- Permettono comunicazione con il mondo esterno
- Gestiti da controller dedicati
- Possono usare DMA (Direct Memory Access)

#### Il Collo di Bottiglia di Von Neumann

**Problema:** CPU e memoria condividono lo stesso bus.

```
CPU ◄──────── BUS CONDIVISO ──────────► MEMORIA
      (solo un trasferimento alla volta)
```

**Conseguenze:**
- La CPU deve aspettare quando accede alla memoria
- Non può leggere istruzioni e dati contemporaneamente
- Limita le prestazioni (velocità limitata dal bus)

**Soluzioni moderne:**
- **Cache**: memoria veloce vicina alla CPU
- **Architettura Harvard**: bus separati per istruzioni e dati
- **Pipeline**: esecuzione sovrapposta di istruzioni
- **Multi-core**: più CPU lavorano in parallelo

---

### 5.3 La CPU in Dettaglio (25 min)

#### Struttura Interna della CPU

```
┌─────────────────────────────────────────────────────┐
│                       CPU                           │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌──────────────────────────────────────────────┐  │
│  │         UNITÀ DI CONTROLLO (CU)              │  │
│  │  ┌──────────┐  ┌──────────┐  ┌───────────┐  │  │
│  │  │ Decoder  │  │ Control  │  │ Sequencer │  │  │
│  │  │Istruzioni│  │  Logic   │  │           │  │  │
│  │  └──────────┘  └──────────┘  └───────────┘  │  │
│  └──────────────┬───────────────────────────────┘  │
│                 │                                   │
│  ┌──────────────┴───────────────────────────────┐  │
│  │    UNITÀ ARITMETICO-LOGICA (ALU)             │  │
│  │  ┌──────────┐  ┌──────────┐  ┌───────────┐  │  │
│  │  │Addizion. │  │  Logica  │  │ Comparator│  │  │
│  │  └──────────┘  └──────────┘  └───────────┘  │  │
│  └──────────────────────────────────────────────┘  │
│                 ▲           │                       │
│                 │           ▼                       │
│  ┌──────────────┴───────────┴───────────────────┐  │
│  │              REGISTRI                         │  │
│  │  ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐  │  │
│  │  │ PC │ │ IR │ │ MAR│ │ MDR│ │ ACC│ │ PSW│  │  │
│  │  └────┘ └────┘ └────┘ └────┘ └────┘ └────┘  │  │
│  │  + Registri Generali (AX, BX, CX, DX...)     │  │
│  └────────────────────────────────────────────────┘│
│                                                     │
└─────────────────────────────────────────────────────┘
```

#### Registri della CPU

I **registri** sono memorie ultra-veloci all'interno della CPU, usate per operazioni immediate.

**Registri Speciali:**

**1. Program Counter (PC) / Instruction Pointer (IP):**
- Contiene l'indirizzo della **prossima istruzione** da eseguire
- Incrementato automaticamente dopo ogni fetch
- Modificato da istruzioni di salto (jump, branch)

**2. Instruction Register (IR):**
- Contiene l'**istruzione corrente** in esecuzione
- Caricato durante la fase di fetch
- Decodificato dalla Control Unit

**3. Memory Address Register (MAR):**
- Contiene l'**indirizzo di memoria** da accedere
- Collegato al bus degli indirizzi

**4. Memory Data Register (MDR) / Memory Buffer Register (MBR):**
- Contiene il **dato** letto o da scrivere in memoria
- Collegato al bus dei dati

**5. Accumulator (ACC):**
- Registro di **lavoro principale** per operazioni ALU
- Memorizza risultati intermedi

**6. Program Status Word (PSW) / Flags Register:**
- Contiene **flag** di stato:
  - **Z** (Zero): risultato è zero
  - **N** (Negative): risultato è negativo
  - **C** (Carry): riporto nell'addizione
  - **V** (Overflow): overflow aritmetico
  - **I** (Interrupt): interrupt abilitati

**Registri Generali:**
- **AX, BX, CX, DX** (architettura x86)
- **R0-R15** (ARM)
- Usati per calcoli generici, parametri funzioni, variabili temporanee

#### ALU - Unità Aritmetico-Logica

L'**ALU** è il cuore computazionale della CPU.

**Operazioni Aritmetiche:**
```
- Addizione: A + B
- Sottrazione: A - B
- Moltiplicazione: A × B (spesso multi-ciclo)
- Divisione: A ÷ B (spesso multi-ciclo)
- Incremento: A + 1
- Decremento: A - 1
```

**Operazioni Logiche:**
```
- AND: A AND B (bit a bit)
- OR: A OR B
- NOT: NOT A (inversione)
- XOR: A XOR B (OR esclusivo)
- NAND, NOR (meno comuni)
```

**Operazioni di Shift:**
```
- Shift Left: << (sposta bit a sinistra)
- Shift Right: >> (sposta bit a destra)
- Rotate: rotazione circolare dei bit
```

**Operazioni di Confronto:**
```
- Uguaglianza: A = B
- Maggiore: A > B
- Minore: A < B
- Risultato memorizzato nei flag
```

**Esempio: Addizione con Carry**
```
Input: A = 11110000₂, B = 00001111₂
Operazione: A + B

  11110000
+ 00001111
----------
  11111111  (risultato)

Flag risultanti:
Z = 0 (risultato non zero)
N = 1 (risultato negativo se signed)
C = 0 (nessun carry finale)
V = 0 (nessun overflow)
```

#### Unità di Controllo

La **Control Unit** è il "direttore d'orchestra" della CPU.

**Compiti:**
1. **Fetch**: preleva istruzione da memoria
2. **Decode**: decodifica l'istruzione
3. **Execute**: coordina esecuzione
4. **Genera segnali**: controlla ALU, registri, memoria

**Sequencer:**
- Genera la temporizzazione (clock)
- Coordina le fasi di esecuzione
- Gestisce stati della CPU

**Decoder:**
- Converte opcode in segnali di controllo
- Identifica operandi
- Attiva circuiti appropriati

---

### 5.4 Memoria e Gerarchia (15 min)

#### Gerarchia di Memoria

Le memorie sono organizzate in una **gerarchia** basata su velocità, costo e capacità:

```
        ┌─────────────┐
        │  REGISTRI   │  ← Velocissimi, costosi, pochi byte
        ├─────────────┤
        │ CACHE L1    │  ← Molto veloci, ~32-64 KB per core
        ├─────────────┤
        │ CACHE L2    │  ← Veloci, ~256 KB - 1 MB per core
        ├─────────────┤
        │ CACHE L3    │  ← Condivisa, ~8-32 MB
        ├─────────────┤
        │     RAM     │  ← Veloce, ~8-64 GB
        ├─────────────┤
        │     SSD     │  ← Abbastanza veloce, ~256 GB - 2 TB
        ├─────────────┤
        │  HARD DISK  │  ← Lento, ~1-10 TB
        ├─────────────┤
        │   CLOUD     │  ← Lentissimo, illimitato
        └─────────────┘

Velocità  ▲        Costo per byte ▲       Capacità ▼
```

#### Tempi di Accesso Comparati

| Memoria | Tempo Accesso | Analogia Umana |
|---------|---------------|----------------|
| Registro | 0.25 ns | 1 secondo |
| Cache L1 | 1 ns | 4 secondi |
| Cache L2 | 3-10 ns | 12-40 secondi |
| Cache L3 | 10-20 ns | 40-80 secondi |
| RAM | 50-100 ns | 3-6 minuti |
| SSD | 50-150 μs | 2-6 giorni |
| Hard Disk | 5-10 ms | 6-12 mesi |
| Internet | 50-500 ms | 5-50 anni |

**Principio di Località:**
- **Località temporale**: se accedi a un dato, lo riaccederai presto
- **Località spaziale**: se accedi a un dato, accederai a dati vicini

La cache sfrutta questi principi per migliorare le prestazioni!

#### Organizzazione della RAM

La RAM è organizzata in **celle** (byte), ognuna con un **indirizzo** univoco.

```
Indirizzo | Contenuto (byte)
----------|------------------
0x0000    | 01001000  ('H')
0x0001    | 01100101  ('e')
0x0002    | 01101100  ('l')
0x0003    | 01101100  ('l')
0x0004    | 01101111  ('o')
0x0005    | 00000000  ('\0')
...
```

**Spazio di indirizzamento:**
- 32 bit: 2³² = 4 GB indirizzabili
- 64 bit: 2⁶⁴ = 16 exabyte indirizzabili (teorico)

**Allineamento:**
- Dati multi-byte dovrebbero iniziare a indirizzi multipli della loro dimensione
- `int` (4 byte): allineato a indirizzi multipli di 4
- Migliora le prestazioni di accesso

---

### 5.5 Il Ciclo Fetch-Decode-Execute (35 min)

#### Introduzione

Il **ciclo fetch-decode-execute** (FDE) è il processo fondamentale con cui la CPU esegue un programma. Si ripete continuamente finché il computer è acceso.

```
     ┌──────────────────┐
     │                  │
     ▼                  │
┌─────────┐             │
│  FETCH  │             │
└────┬────┘             │
     │                  │
     ▼                  │
┌─────────┐             │
│ DECODE  │             │
└────┬────┘             │
     │                  │
     ▼                  │
┌─────────┐             │
│ EXECUTE │─────────────┘
└─────────┘
```

#### Fase 1: FETCH (Prelievo)

**Obiettivo:** Prelevare la prossima istruzione dalla memoria.

**Passi dettagliati:**

1. **Copia PC in MAR**
   ```
   MAR ← PC
   ```
   - L'indirizzo della prossima istruzione viene messo nel MAR

2. **Invia indirizzo al bus**
   ```
   BUS_INDIRIZZI ← MAR
   ```
   - L'indirizzo viene inviato alla memoria

3. **Leggi dalla memoria**
   ```
   MDR ← MEMORIA[MAR]
   ```
   - L'istruzione viene caricata nel MDR

4. **Copia in IR**
   ```
   IR ← MDR
   ```
   - L'istruzione è ora nell'Instruction Register

5. **Incrementa PC**
   ```
   PC ← PC + lunghezza_istruzione
   ```
   - PC punta alla prossima istruzione

**Esempio pratico:**
```
Stato iniziale:
PC = 0x1000
MEMORIA[0x1000] = 0x8B45FC  (istruzione MOV)

Dopo FETCH:
IR = 0x8B45FC
PC = 0x1003  (incrementato di 3 byte)
```

#### Fase 2: DECODE (Decodifica)

**Obiettivo:** Interpretare l'istruzione e preparare l'esecuzione.

**Passi dettagliati:**

1. **Estrai Opcode**
   ```
   OPCODE ← IR[campo_opcode]
   ```
   - Identifica il tipo di operazione (ADD, SUB, LOAD, etc.)

2. **Estrai Operandi**
   ```
   OPERAND1 ← IR[campo_operando1]
   OPERAND2 ← IR[campo_operando2]
   ```
   - Identifica i dati su cui operare

3. **Determina Modalità di Indirizzamento**
   - Immediato: operando è un valore costante
   - Diretto: operando è un indirizzo di memoria
   - Indiretto: operando è un indirizzo che contiene un altro indirizzo
   - Registro: operando è in un registro

4. **Prepara Circuiteria**
   - La Control Unit attiva i segnali necessari
   - Prepara ALU per l'operazione
   - Prepara percorsi dati (data paths)

**Esempio di Decodifica:**
```
Istruzione: ADD AX, BX

Decodifica:
- OPCODE: ADD (addizione)
- OPERANDO1: AX (registro destinazione)
- OPERANDO2: BX (registro sorgente)
- Modalità: registro-registro
- Azione: AX ← AX + BX
```

#### Fase 3: EXECUTE (Esecuzione)

**Obiettivo:** Eseguire effettivamente l'operazione.

Le operazioni variano in base al tipo di istruzione:

**A. Istruzioni Aritmetiche/Logiche**
```
Esempio: ADD AX, BX

1. Leggi operandi dai registri
   temp1 ← AX
   temp2 ← BX

2. Esegui operazione in ALU
   risultato ← temp1 + temp2

3. Aggiorna flag
   if (risultato = 0) then Z ← 1
   if (risultato < 0) then N ← 1
   if (carry) then C ← 1

4. Scrivi risultato
   AX ← risultato
```

**B. Istruzioni di Load/Store**
```
Esempio: LOAD AX, [0x2000]

1. Calcola indirizzo effettivo
   MAR ← 0x2000

2. Leggi dalla memoria
   MDR ← MEMORIA[MAR]

3. Trasferisci a registro
   AX ← MDR
```

```
Esempio: STORE [0x2000], AX

1. Prepara dato
   MDR ← AX

2. Prepara indirizzo
   MAR ← 0x2000

3. Scrivi in memoria
   MEMORIA[MAR] ← MDR
```

**C. Istruzioni di Salto**
```
Esempio: JMP 0x3000  (salto incondizionato)

1. Aggiorna PC
   PC ← 0x3000
```

```
Esempio: JZ 0x3000  (salta se zero)

1. Controlla flag Z
   if (Z = 1) then
      PC ← 0x3000
   else
      PC ← PC  (nessun salto)
```

**D. Istruzioni di Call/Return**
```
Esempio: CALL 0x4000  (chiamata a funzione)

1. Salva indirizzo di ritorno
   PUSH PC  (sullo stack)

2. Salta alla funzione
   PC ← 0x4000
```

```
Esempio: RET  (ritorno da funzione)

1. Recupera indirizzo di ritorno
   PC ← POP  (dallo stack)
```

#### Esempio Completo: Esecuzione di un Programma

**Programma semplice:**
```assembly
0x1000: LOAD AX, [0x2000]   ; Carica valore da indirizzo 0x2000
0x1003: ADD AX, 5            ; Aggiungi 5
0x1006: STORE [0x2001], AX   ; Salva risultato
0x1009: HALT                 ; Ferma
```

**Dati in memoria:**
```
MEMORIA[0x2000] = 10
MEMORIA[0x2001] = 0 (inizialmente)
```

**Traccia esecuzione:**

**CICLO 1: LOAD AX, [0x2000]**
```
FETCH:
  PC = 0x1000
  MAR ← 0x1000
  IR ← MEMORIA[0x1000] = LOAD AX, [0x2000]
  PC ← 0x1003

DECODE:
  OPCODE = LOAD
  DEST = AX
  SOURCE = [0x2000] (indirizzamento diretto)

EXECUTE:
  MAR ← 0x2000
  MDR ← MEMORIA[0x2000] = 10
  AX ← 10

Stato: AX = 10, PC = 0x1003
```

**CICLO 2: ADD AX, 5**
```
FETCH:
  PC = 0x1003
  IR ← MEMORIA[0x1003] = ADD AX, 5
  PC ← 0x1006

DECODE:
  OPCODE = ADD
  DEST = AX
  SOURCE = 5 (immediato)

EXECUTE:
  temp1 ← AX = 10
  temp2 ← 5
  risultato ← 10 + 5 = 15
  AX ← 15
  Flag: Z=0, N=0, C=0

Stato: AX = 15, PC = 0x1006
```

**CICLO 3: STORE [0x2001], AX**
```
FETCH:
  PC = 0x1006
  IR ← MEMORIA[0x1006] = STORE [0x2001], AX
  PC ← 0x1009

DECODE:
  OPCODE = STORE
  DEST = [0x2001]
  SOURCE = AX

EXECUTE:
  MDR ← AX = 15
  MAR ← 0x2001
  MEMORIA[0x2001] ← 15

Stato: MEMORIA[0x2001] = 15, PC = 0x1009
```

**CICLO 4: HALT**
```
FETCH:
  PC = 0x1009
  IR ← MEMORIA[0x1009] = HALT
  PC ← 0x100A (ma non verrà usato)

DECODE:
  OPCODE = HALT

EXECUTE:
  Ferma esecuzione
  CPU entra in stato di stop

Stato: PROGRAMMA TERMINATO
```

**Risultato finale:**
```
AX = 15
MEMORIA[0x2001] = 15
PC = 0x100A
```

---

### 5.6 Istruzioni a Basso Livello e Assembly (10 min)

#### Cos'è il Linguaggio Assembly?

Il **linguaggio assembly** è il linguaggio di programmazione più vicino al linguaggio macchina, ma usa mnemonici leggibili invece di codici binari.

**Livelli di Linguaggio:**
```
Alto Livello (C):        int x = 5 + 3;

Assembly (x86):          MOV EAX, 5
                        ADD EAX, 3
                        MOV [x], EAX

Linguaggio Macchina:     B8 05 00 00 00
                        83 C0 03
                        A3 00 10 00 00
```

#### Tipi di Istruzioni Assembly

**1. Trasferimento Dati:**
```assembly
MOV dest, source    ; Copia dato
PUSH source         ; Inserisci nello stack
POP dest            ; Estrai dallo stack
LEA dest, source    ; Carica indirizzo effettivo
```

**2. Aritmetiche:**
```assembly
ADD dest, source    ; dest = dest + source
SUB dest, source    ; dest = dest - source
MUL source          ; AX = AL × source (8-bit)
DIV source          ; Divisione
INC dest            ; dest = dest + 1
DEC dest            ; dest = dest - 1
```

**3. Logiche:**
```assembly
AND dest, source    ; AND bit a bit
OR dest, source     ; OR bit a bit
XOR dest, source    ; XOR bit a bit
NOT dest            ; Complemento
SHL dest, count     ; Shift left
SHR dest, count     ; Shift right
```

**4. Controllo Flusso:**
```assembly
JMP label           ; Salto incondizionato
JZ label            ; Salta se zero
JNZ label           ; Salta se non zero
JE label            ; Salta se uguale
JNE label           ; Salta se diverso
JG label            ; Salta se maggiore
CALL function       ; Chiamata funzione
RET                 ; Ritorno da funzione
```

**5. Confronto:**
```assembly
CMP op1, op2        ; Confronta (op1 - op2), aggiorna flag
TEST op1, op2       ; Testa (op1 AND op2), aggiorna flag
```

#### Esempio: Somma di Due Numeri

**In C:**
```c
int a = 5;
int b = 3;
int c = a + b;
```

**In Assembly (x86):**
```assembly
; Definizione dati
a: .word 5
b: .word 3
c: .word 0

; Codice
MOV AX, [a]      ; Carica a in AX
ADD AX, [b]      ; Aggiungi b ad AX
MOV [c], AX      ; Salva risultato in c
```

**In Linguaggio Macchina (semplificato):**
```
A1 00 10        ; MOV AX, [0x1000]
03 06 02 10     ; ADD AX, [0x1002]
A3 04 10        ; MOV [0x1004], AX
```

#### Formato Istruzione

Le istruzioni macchina hanno un formato strutturato:

```
┌────────┬──────────┬──────────┬──────────┐
│ OPCODE │  MODE    │  OPERAND1│ OPERAND2 │
└────────┴──────────┴──────────┴──────────┘
   8 bit    2 bit      6 bit      varia
```

**Esempio: ADD AX, BX**
```
Formato: OPCODE(ADD) + MODE(reg-reg) + DEST(AX) + SRC(BX)
Binario: 00000001 00 000000 000001
Hex: 0x01 0x01
```

---

## PARTE PRATICA (2 ore)

### ESERCIZI GUIDATI

#### Esercizio 1: Simulazione Ciclo FDE (30 min)

**Simulatore semplificato di CPU**

Specifiche:
- 4 registri: A, B, C, D (8 bit ciascuno)
- Memoria: 256 byte (indirizzi 0x00-0xFF)
- Program Counter (PC)
- Instruction Register (IR)
- Flag: Z (zero), N (negative)

**Set di istruzioni:**
```
LOAD reg, [addr]   ; reg ← MEMORIA[addr]
STORE [addr], reg  ; MEMORIA[addr] ← reg
ADD reg1, reg2     ; reg1 ← reg1 + reg2
SUB reg1, reg2     ; reg1 ← reg1 - reg2
MOV reg1, reg2     ; reg1 ← reg2
JMP addr           ; PC ← addr
JZ addr            ; if Z=1 then PC ← addr
HALT               ; Ferma esecuzione
```

**1.1** Simula l'esecuzione del seguente programma:

```assembly
Indirizzo | Istruzione           | Commento
----------|---------------------|------------------------
0x10      | LOAD A, [0x50]      | Carica primo numero
0x12      | LOAD B, [0x51]      | Carica secondo numero
0x14      | ADD A, B            | Somma
0x16      | STORE [0x52], A     | Salva risultato
0x18      | HALT                | Fine
```

**Dati iniziali:**
```
MEMORIA[0x50] = 7
MEMORIA[0x51] = 3
PC = 0x10
Tutti i registri = 0
```

**Soluzione - Traccia completa:**

**CICLO 1: LOAD A, [0x50]**
```
FETCH:
  MAR ← PC = 0x10
  IR ← MEMORIA[0x10] = "LOAD A, [0x50]"
  PC ← 0x12

DECODE:
  OPCODE = LOAD
  DEST = A
  SOURCE = [0x50] (indirizzamento diretto)

EXECUTE:
  MAR ← 0x50
  MDR ← MEMORIA[0x50] = 7
  A ← 7

STATO DOPO CICLO 1:
  A = 7, B = 0, C = 0, D = 0
  PC = 0x12
  MEMORIA[0x52] = 0 (non ancora modificata)
```

**CICLO 2: LOAD B, [0x51]**
```
FETCH:
  MAR ← PC = 0x12
  IR ← MEMORIA[0x12] = "LOAD B, [0x51]"
  PC ← 0x14

DECODE:
  OPCODE = LOAD
  DEST = B
  SOURCE = [0x51]

EXECUTE:
  MAR ← 0x51
  MDR ← MEMORIA[0x51] = 3
  B ← 3

STATO DOPO CICLO 2:
  A = 7, B = 3, C = 0, D = 0
  PC = 0x14
```

**CICLO 3: ADD A, B**
```
FETCH:
  MAR ← PC = 0x14
  IR ← MEMORIA[0x14] = "ADD A, B"
  PC ← 0x16

DECODE:
  OPCODE = ADD
  DEST = A
  SOURCE = B

EXECUTE:
  temp1 ← A = 7
  temp2 ← B = 3
  risultato ← 7 + 3 = 10
  A ← 10
  
  Aggiorna flag:
  Z ← 0 (risultato non zero)
  N ← 0 (risultato positivo)

STATO DOPO CICLO 3:
  A = 10, B = 3, C = 0, D = 0
  PC = 0x16
  Flag: Z = 0, N = 0
```

**CICLO 4: STORE [0x52], A**
```
FETCH:
  MAR ← PC = 0x16
  IR ← MEMORIA[0x16] = "STORE [0x52], A"
  PC ← 0x18

DECODE:
  OPCODE = STORE
  DEST = [0x52]
  SOURCE = A

EXECUTE:
  MDR ← A = 10
  MAR ← 0x52
  MEMORIA[0x52] ← 10

STATO DOPO CICLO 4:
  A = 10, B = 3, C = 0, D = 0
  PC = 0x18
  MEMORIA[0x52] = 10
```

**CICLO 5: HALT**
```
FETCH:
  MAR ← PC = 0x18
  IR ← MEMORIA[0x18] = "HALT"
  PC ← 0x1A (non verrà usato)

DECODE:
  OPCODE = HALT

EXECUTE:
  Ferma CPU
  
STATO FINALE:
  A = 10, B = 3, C = 0, D = 0
  PC = 0x1A
  MEMORIA[0x52] = 10
  PROGRAMMA TERMINATO
```

**Riepilogo:**
- Cicli totali: 5
- Istruzioni eseguite: 5
- Accessi memoria: 9 (5 fetch istruzioni + 2 load + 1 store + 1 halt)
- Risultato: 7 + 3 = 10 memorizzato in 0x52 ✓

---

**1.2** Simula l'esecuzione con un salto condizionato:

```assembly
Indirizzo | Istruzione           | Commento
----------|---------------------|------------------------
0x20      | LOAD A, [0x60]      | Carica numero
0x22      | SUB A, A            | A = A - A (azzera A)
0x24      | JZ 0x28             | Salta se zero
0x26      | LOAD B, [0x61]      | Non eseguita!
0x28      | HALT                | Fine
```

**Dati iniziali:**
```
MEMORIA[0x60] = 5
MEMORIA[0x61] = 9
PC = 0x20
```

**Soluzione:**

**CICLO 1: LOAD A, [0x60]**
```
Dopo esecuzione:
  A = 5
  PC = 0x22
```

**CICLO 2: SUB A, A**
```
EXECUTE:
  A ← A - A = 5 - 5 = 0
  Z ← 1 (risultato è zero!)
  N ← 0

Dopo esecuzione:
  A = 0
  PC = 0x24
  Flag: Z = 1
```

**CICLO 3: JZ 0x28**
```
DECODE:
  OPCODE = JZ (Jump if Zero)
  TARGET = 0x28

EXECUTE:
  if (Z = 1) then
    PC ← 0x28  ✓ (Z è 1, quindi saltiamo!)
  else
    PC ← PC

Dopo esecuzione:
  PC = 0x28 (salto effettuato!)
  
Nota: L'istruzione a 0x26 viene SALTATA!
```

**CICLO 4: HALT**
```
Dopo esecuzione:
  PROGRAMMA TERMINATO
```

**Risultato:**
- A = 0, B = 0 (B non è mai stato caricato!)
- Istruzione a 0x26 NON eseguita
- Salto condizionato riuscito perché Z = 1

---

#### Esercizio 2: Codifica Istruzioni (25 min)

**Formato istruzione semplificato (16 bit):**
```
┌────────┬────────┬────────┬────────┐
│ OPCODE │  DEST  │ SOURCE │  MODE  │
│ 4 bit  │ 4 bit  │ 4 bit  │ 4 bit  │
└────────┴────────┴────────┴────────┘
```

**Codici OPCODE:**
```
0000 = LOAD
0001 = STORE
0010 = ADD
0011 = SUB
0100 = MOV
0101 = JMP
0110 = JZ
1111 = HALT
```

**Codici REGISTRI:**
```
0000 = A
0001 = B
0010 = C
0011 = D
```

**Modalità:**
```
0000 = Registro-Registro
0001 = Registro-Immediato
0010 = Registro-Memoria
0011 = Memoria-Registro
```

**2.1** Codifica l'istruzione `ADD A, B` in binario e esadecimale.

```
Soluzione:
OPCODE = ADD = 0010
DEST = A = 0000
SOURCE = B = 0001
MODE = Reg-Reg = 0000

Binario: 0010 0000 0001 0000
Esadecimale: 0x2010

Verifica:
- Bit 15-12: 0010 (ADD)
- Bit 11-8:  0000 (A)
- Bit 7-4:   0001 (B)
- Bit 3-0:   0000 (Reg-Reg)
```

**2.2** Codifica `LOAD A, [0x50]`

```
Soluzione:
OPCODE = LOAD = 0000
DEST = A = 0000
SOURCE = 0x50 (servono 2 byte per l'indirizzo!)

Formato esteso (32 bit):
┌────────┬────────┬────────┬────────┬────────────────┐
│ OPCODE │  DEST  │ UNUSED │  MODE  │    ADDRESS     │
│ 4 bit  │ 4 bit  │ 4 bit  │ 4 bit  │    16 bit      │
└────────┴────────┴────────┴────────┴────────────────┘

Binario: 0000 0000 0000 0010 0000000001010000
Esadecimale: 0x00020050

Nota: Istruzioni con operandi memoria sono più lunghe!
```

**2.3** Decodifica l'istruzione `0x3120`

```
Soluzione:
Binario: 0011 0001 0010 0000

OPCODE: 0011 = SUB
DEST: 0001 = B
SOURCE: 0010 = C
MODE: 0000 = Reg-Reg

Istruzione: SUB B, C
Significato: B ← B - C
```

---

#### Esercizio 3: Progettazione Programmi Assembly (30 min)

**3.1** Scrivi un programma che calcola il massimo tra due numeri.

```assembly
; Input: MEMORIA[0x70] = num1, MEMORIA[0x71] = num2
; Output: MEMORIA[0x72] = max

START:
    LOAD A, [0x70]      ; A = num1
    LOAD B, [0x71]      ; B = num2
    SUB A, B            ; A = num1 - num2
    JZ EQUAL            ; Se uguali, salta
    ; A contiene num1-num2, se negativo B è maggiore
    ; Nota: Qui semplifichiamo assumendo flag N
    LOAD A, [0x70]      ; Ricarica num1
    STORE [0x72], A     ; Assumiamo A sia max
    HALT
    
EQUAL:
    LOAD A, [0x70]      ; Qualunque va bene
    STORE [0x72], A
    HALT

; Nota: Versione semplificata. In assembly reale useremmo
; confronti e salti condizionati più sofisticati (JG, JL, ecc.)
```

**Versione migliorata con confronto:**
```assembly
START:
    LOAD A, [0x70]      ; A = num1
    LOAD B, [0x71]      ; B = num2
    MOV C, A            ; Salva num1 in C
    SUB A, B            ; Confronta: A - B
    JZ EQUAL            ; Se uguali
    ; Se qui, sono diversi
    ; Dobbiamo controllare il segno
    ; (implementazione dipende da architettura)
    
; Soluzione pratica: usa CMP se disponibile
START2:
    LOAD A, [0x70]
    LOAD B, [0x71]
    CMP A, B            ; Confronta A e B
    JG A_MAGGIORE       ; Se A > B
    STORE [0x72], B     ; Altrimenti B è max
    HALT
    
A_MAGGIORE:
    STORE [0x72], A     ; A è max
    HALT
    
EQUAL:
    STORE [0x72], A     ; Qualunque
    HALT
```

**3.2** Programma per sommare N numeri in un array.

```assembly
; Input: 
;   MEMORIA[0x80] = N (quanti numeri)
;   MEMORIA[0x81..0x80+N] = array di numeri
; Output: 
;   MEMORIA[0x90] = somma

START:
    LOAD A, [0x80]      ; A = N (contatore)
    MOV B, 0            ; B = somma = 0
    MOV C, 0x81         ; C = indirizzo corrente
    
LOOP:
    JZ A, FINE          ; Se A = 0, finito
    LOAD D, [C]         ; D = numero corrente
    ADD B, D            ; Somma += numero
    INC C               ; C++ (prossimo indirizzo)
    DEC A               ; A-- (decrementa contatore)
    JMP LOOP            ; Ripeti
    
FINE:
    STORE [0x90], B     ; Salva somma
    HALT

; Nota: INC e DEC sono istruzioni che incrementano/decrementano
; JZ A, addr salta se A è zero
```

**Traccia esempio:**
```
MEMORIA[0x80] = 3        ; N = 3
MEMORIA[0x81] = 10       ; array[0]
MEMORIA[0x82] = 20       ; array[1]
MEMORIA[0x83] = 30       ; array[2]

Esecuzione:
Iterazione 1: A=3, B=0,  C=0x81, D=10 → B=10
Iterazione 2: A=2, B=10, C=0x82, D=20 → B=30
Iterazione 3: A=1, B=30, C=0x83, D=30 → B=60
Fine: A=0, B=60

MEMORIA[0x90] = 60 ✓
```

**3.3** Programma per calcolare il fattoriale (semplificato).

```assembly
; Input: MEMORIA[0xA0] = N
; Output: MEMORIA[0xA1] = N!

START:
    LOAD A, [0xA0]      ; A = N
    MOV B, 1            ; B = risultato = 1
    JZ A, ZERO          ; Se N=0, fattoriale=1
    
LOOP:
    JZ A, FINE          ; Se A=0, finito
    MUL B, A            ; B = B * A
    DEC A               ; A--
    JMP LOOP
    
ZERO:
FINE:
    STORE [0xA1], B     ; Salva fattoriale
    HALT

; Nota: MUL è una moltiplicazione
; Questa è una versione molto semplificata!
```

---

#### Esercizio 4: Analisi Prestazioni (25 min)

**4.1** Calcola quanti cicli di clock servono per eseguire il programma:

```assembly
0x00: LOAD A, [0x50]    ; 3 cicli (fetch + decode + execute con accesso memoria)
0x02: ADD A, 5          ; 2 cicli (fetch + decode + execute senza memoria)
0x04: STORE [0x51], A   ; 3 cicli
0x06: HALT              ; 1 ciclo
```

```
Soluzione:
LOAD: 3 cicli
ADD: 2 cicli
STORE: 3 cicli
HALT: 1 ciclo
Totale: 9 cicli

Se il clock è 1 GHz (1 ciclo = 1 nanosecondo):
Tempo totale = 9 ns
```

**4.2** Confronta due implementazioni dello stesso programma.

**Versione A: Con ciclo**
```assembly
; Somma 100 volte il numero 1
MOV A, 0        ; 1 ciclo
MOV B, 100      ; 1 ciclo
LOOP:
    ADD A, 1    ; 2 cicli
    DEC B       ; 1 ciclo
    JNZ LOOP    ; 2 cicli se salta, 1 se no
HALT            ; 1 ciclo

Calcolo cicli:
Setup: 2 cicli
Loop: 100 iterazioni × (2 + 1 + 2) = 500 cicli
Ultima iter: 2 + 1 + 1 = 4 cicli (JNZ non salta)
Halt: 1 ciclo
Totale: 2 + 499×5 + 4 + 1 = 2502 cicli
```

**Versione B: Senza ciclo (ottimizzata)**
```assembly
MOV A, 100      ; 1 ciclo
HALT            ; 1 ciclo

Totale: 2 cicli
```

**Conclusione:**
- Versione A: 2502 cicli
- Versione B: 2 cicli
- Speedup: 1251× più veloce!

**Lezione:** Il compilatore ottimizza operazioni costanti!

---

#### Esercizio 5: Cache e Località (20 min)

**5.1** Analizza l'impatto della cache sul seguente codice:

```c
// Versione A: Accesso per righe (località spaziale buona)
for (i = 0; i < 1000; i++) {
    for (j = 0; j < 1000; j++) {
        sum += array[i][j];
    }
}

// Versione B: Accesso per colonne (località spaziale cattiva)
for (j = 0; j < 1000; j++) {
    for (i = 0; i < 1000; i++) {
        sum += array[i][j];
    }
}
```

```
Soluzione:

Array in memoria (row-major order in C):
[0][0], [0][1], [0][2], ..., [0][999],
[1][0], [1][1], [1][2], ..., [1][999],
...

Versione A (per righe):
- Accede sequenzialmente: [0][0], [0][1], [0][2]...
- Ottima località spaziale
- Cache miss ratio: ~1% (solo primo elemento di ogni linea cache)
- Tempo: 100 ms (esempio)

Versione B (per colonne):
- Accede con salti: [0][0], [1][0], [2][0]...
- Cattiva località spaziale
- Cache miss ratio: ~100% (ogni accesso "salta" la linea cache)
- Tempo: 800 ms (esempio)

Speedup Versione A: 8× più veloce!

Lezione: L'ordine degli accessi alla memoria è critico!
```

**5.2** Simula accessi cache con una cache a 4 linee:

```
Cache (4 linee, ciascuna contiene 4 byte):
┌─────┬─────────────────┬──────┐
│ Idx │  Indirizzi      │ Stato│
├─────┼─────────────────┼──────┤
│  0  │ vuoto           │      │
│  1  │ vuoto           │      │
│  2  │ vuoto           │      │
│  3  │ vuoto           │      │
└─────┴─────────────────┴──────┘

Sequenza accessi: 0x00, 0x04, 0x08, 0x0C, 0x00, 0x10
```

```
Soluzione (direct-mapped cache, line size = 4 byte):

Accesso 1: 0x00
- Cache MISS (linea 0 vuota)
- Carica linea: [0x00-0x03]
┌─────┬─────────────────┬──────┐
│  0  │ 0x00-0x03       │ VALID│
│  1  │ vuoto           │      │
│  2  │ vuoto           │      │
│  3  │ vuoto           │      │
└─────┴─────────────────┴──────┘

Accesso 2: 0x04
- Cache MISS (linea 1 vuota)
- Carica linea: [0x04-0x07]
┌─────┬─────────────────┬──────┐
│  0  │ 0x00-0x03       │ VALID│
│  1  │ 0x04-0x07       │ VALID│
│  2  │ vuoto           │      │
│  3  │ vuoto           │      │
└─────┴─────────────────┴──────┘

Accesso 3: 0x08
- Cache MISS
┌─────┬─────────────────┬──────┐
│  0  │ 0x00-0x03       │ VALID│
│  1  │ 0x04-0x07       │ VALID│
│  2  │ 0x08-0x0B       │ VALID│
│  3  │ vuoto           │      │
└─────┴─────────────────┴──────┘

Accesso 4: 0x0C
- Cache MISS
┌─────┬─────────────────┬──────┐
│  0  │ 0x00-0x03       │ VALID│
│  1  │ 0x04-0x07       │ VALID│
│  2  │ 0x08-0x0B       │ VALID│
│  3  │ 0x0C-0x0F       │ VALID│
└─────┴─────────────────┴──────┘

Accesso 5: 0x00
- Cache HIT! ✓ (già in linea 0)
- Nessun accesso a memoria

Accesso 6: 0x10
- Cache MISS
- Deve sostituire linea 0 (piena)
┌─────┬─────────────────┬──────┐
│  0  │ 0x10-0x13       │ VALID│ (sostituita)
│  1  │ 0x04-0x07       │ VALID│
│  2  │ 0x08-0x0B       │ VALID│
│  3  │ 0x0C-0x0F       │ VALID│
└─────┴─────────────────┴──────┘

Riepilogo:
- Accessi totali: 6
- Cache miss: 5
- Cache hit: 1
- Hit rate: 16.7%
- Accessi a memoria principale: 5 (costosi!)
```

---

### ESERCIZI AUTONOMI

#### Esercizio 6: Progettazione Completa

**6.1** Progetta un programma assembly che:
- Legge un array di N numeri
- Conta quanti sono pari e quanti dispari
- Memorizza i risultati

**6.2** Scrivi un programma che inverte un array:
```
Input:  [1, 2, 3, 4, 5]
Output: [5, 4, 3, 2, 1]
```

**6.3** Implementa la ricerca lineare in assembly:
- Input: array e valore da cercare
- Output: indice se trovato, -1 altrimenti

---

### ESERCIZI DA SVOLGERE A CASA

**Homework 1:** Disegna un diagramma temporale (timing diagram) per l'esecuzione dell'istruzione `LOAD A, [0x100]`, mostrando:
- Segnali di clock
- Attività del bus indirizzi
- Attività del bus dati
- Stato dei registri (PC, MAR, MDR, IR)

**Homework 2:** Simula manualmente l'esecuzione del seguente programma:
```assembly
0x00: LOAD A, [0x20]
0x02: LOAD B, [0x21]
0x04: ADD A, B
0x06: STORE [0x22], A
0x08: SUB A, B
0x0A: STORE [0x23], A
0x0C: HALT
```
Con dati: MEMORIA[0x20] = 15, MEMORIA[0x21] = 7

**Homework 3:** Ricerca e documenta:
- Cos'è la pipeline della CPU?
- Come funziona l'esecuzione speculativa?
- Cosa sono gli hazard nella pipeline?
- Cos'è il branch prediction?

**Homework 4:** Confronta le architetture:
- Von Neumann vs Harvard
- RISC vs CISC
- Vantaggi e svantaggi di ciascuna

**Homework 5:** Scrivi un programma assembly (pseudocodice) che implementa il bubble sort per ordinare un array di 5 numeri.

**Homework 6:** Calcola quanta energia consuma una CPU durante l'esecuzione di un programma:
- Frequenza: 3 GHz
- Tensione: 1.2V
- Corrente media: 50A
- Durata programma: 1 secondo
Formula: Energia = Potenza × Tempo, Potenza = Tensione × Corrente

---

## APPROFONDIMENTI

### Storia dell'Architettura dei Computer

**Timeline:**
- **1945**: John von Neumann propone l'architettura stored-program
- **1951**: UNIVAC I, primo computer commerciale
- **1971**: Intel 4004, primo microprocessore (2300 transistor, 740 kHz)
- **1978**: Intel 8086, architettura x86 (nasce il PC)
- **1985**: Intel 386, primo x86 a 32 bit
- **2003**: AMD Athlon 64, x86-64 (64 bit)
- **2006**: Intel Core 2 Duo, era multi-core
- **2020**: Apple M1, ARM ad alte prestazioni

### Legge di Moore

**Enunciato (1965):** Il numero di transistor su un chip raddoppia circa ogni 2 anni.

**Evoluzione:**
```
1971 - Intel 4004:        2,300 transistor
1978 - Intel 8086:       29,000 transistor
1993 - Pentium:       3,100,000 transistor
2000 - Pentium 4:    42,000,000 transistor
2010 - Core i7:   1,170,000,000 transistor
2020 - Apple M1: 16,000,000,000 transistor
```

**Stato attuale:** La legge sta rallentando a causa di limiti fisici (dimensioni atomiche, dissipazione termica).

### Architettura Harvard

**Differenza principale:** Bus separati per istruzioni e dati.

```
     ┌───────┐
     │  CPU  │
     └───┬───┘
         │
    ┌────┴────┐
    │         │
    ▼         ▼
┌────────┐ ┌────────┐
│ MEMORIA│ │ MEMORIA│
│ISTRUZIONI│ DATI  │
└────────┘ └────────┘
```

**Vantaggi:**
- Può accedere a istruzioni e dati contemporaneamente
- Più veloce (no collo di bottiglia)
- Più sicura (codice e dati separati)

**Svantaggi:**
- Più complessa
- Più costosa
- Meno flessibile

**Uso:** DSP, microcontrollori embedded, sistemi real-time

### RISC vs CISC

**RISC** (Reduced Instruction Set Computer):
- Poche istruzioni semplici
- Tutte stessa durata (1 ciclo)
- Molti registri
- Pipeline efficiente
- Esempi: ARM, MIPS, RISC-V

**CISC** (Complex Instruction Set Computer):
- Molte istruzioni complesse
- Durata variabile (1-100+ cicli)
- Pochi registri
- Istruzioni potenti
- Esempi: x86, x86-64

**Trend moderno:** Ibrid