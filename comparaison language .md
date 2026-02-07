# Comparaison : C vs Rust vs Go vs OCaml
## Programmation Concurrente

---

# Table des Matières

1. [Vue d'Ensemble](#1-vue-densemble)
2. [Philosophie et Approche](#2-philosophie-et-approche)
3. [Sécurité Mémoire](#3-sécurité-mémoire)
4. [Primitives de Concurrence](#4-primitives-de-concurrence)
5. [Exemples de Code Comparés](#5-exemples-de-code-comparés)
6. [Performance](#6-performance)
7. [Facilité d'Utilisation](#7-facilité-dutilisation)
8. [Écosystème et Outils](#8-écosystème-et-outils)
9. [Cas d'Usage Recommandés](#9-cas-dusage-recommandés)
10. [Tableau Récapitulatif](#10-tableau-récapitulatif)

---

# 1. Vue d'Ensemble

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        RÉSUMÉ EN UNE PHRASE                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  C      →  "Tu as le pouvoir total, mais tu es seul responsable"            │
│            Contrôle absolu, aucune protection                               │
│                                                                             │
│  RUST   →  "Le compilateur est ton garde du corps strict"                   │
│            Sécurité garantie à la compilation, zero-cost                    │
│                                                                             │
│  GO     →  "La concurrence facile pour tout le monde"                       │
│            Goroutines légères, channels intégrés, simple                    │
│                                                                             │
│  OCAML  →  "Fonctionnel et élégant, mais threads limités"                   │
│            Immutabilité naturelle, GIL historique (changé en OCaml 5)       │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Tableau Comparatif Rapide

```
┌──────────────────┬─────────┬─────────┬─────────┬─────────┐
│                  │    C    │  RUST   │   GO    │  OCAML  │
├──────────────────┼─────────┼─────────┼─────────┼─────────┤
│ Année création   │  1972   │  2010   │  2009   │  1996   │
├──────────────────┼─────────┼─────────┼─────────┼─────────┤
│ Paradigme        │ Impérat.│ Multi   │ Impérat.│ Fonct.  │
├──────────────────┼─────────┼─────────┼─────────┼─────────┤
│ Gestion mémoire  │ Manuel  │ Owner.  │ GC      │ GC      │
├──────────────────┼─────────┼─────────┼─────────┼─────────┤
│ Sécurité threads │ Aucune  │ Compile │ Runtime │ Partiel │
├──────────────────┼─────────┼─────────┼─────────┼─────────┤
│ Modèle principal │ Threads │ Threads │ CSP     │ Threads │
│                  │ + mutex │ + async │ Gorout. │ + async │
├──────────────────┼─────────┼─────────┼─────────┼─────────┤
│ Courbe apprent.  │ Moyenne │ Raide   │ Douce   │ Raide   │
├──────────────────┼─────────┼─────────┼─────────┼─────────┤
│ Performance      │ ★★★★★  │ ★★★★★  │ ★★★★☆  │ ★★★★☆  │
├──────────────────┼─────────┼─────────┼─────────┼─────────┤
│ Sécurité         │ ★☆☆☆☆  │ ★★★★★  │ ★★★★☆  │ ★★★★☆  │
├──────────────────┼─────────┼─────────┼─────────┼─────────┤
│ Simplicité       │ ★★★☆☆  │ ★★☆☆☆  │ ★★★★★  │ ★★★☆☆  │
└──────────────────┴─────────┴─────────┴─────────┴─────────┘
```

---

# 2. Philosophie et Approche

## C - "Fais ce que tu veux, à tes risques et périls"

```
┌─────────────────────────────────────────────────────────────┐
│                         C                                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  PHILOSOPHIE :                                              │
│  • Accès direct au matériel                                 │
│  • Aucune abstraction cachée                                │
│  • Le programmeur sait ce qu'il fait                        │
│  • Performance maximale                                     │
│                                                             │
│  APPROCHE CONCURRENCE :                                     │
│  • Threads POSIX (pthreads)                                 │
│  • Mutex, semaphores, condition variables                   │
│  • Mémoire partagée par défaut                              │
│  • AUCUNE vérification automatique                          │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  "C te donne une corde. Tu peux t'en servir pour    │   │
│  │   grimper... ou te pendre."                         │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  AVANTAGES :                                                │
│  ✓ Performance maximale                                     │
│  ✓ Contrôle total                                           │
│  ✓ Portable (partout)                                       │
│  ✓ Bibliothèques matures                                    │
│                                                             │
│  INCONVÉNIENTS :                                            │
│  ✗ Data races faciles                                       │
│  ✗ Deadlocks fréquents                                      │
│  ✗ Bugs difficiles à trouver                                │
│  ✗ Pas de protection du compilateur                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Rust - "Sécurité garantie par le compilateur"

```
┌─────────────────────────────────────────────────────────────┐
│                        RUST                                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  PHILOSOPHIE :                                              │
│  • "Fearless Concurrency" (concurrence sans peur)           │
│  • Sécurité mémoire SANS garbage collector                  │
│  • Zero-cost abstractions                                   │
│  • Le compilateur attrape les bugs AVANT l'exécution        │
│                                                             │
│  APPROCHE CONCURRENCE :                                     │
│  • Ownership + Borrowing = pas de data races                │
│  • Traits Send et Sync pour la sécurité threads             │
│  • Threads + async/await                                    │
│  • Channels pour message passing                            │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  "Si ça compile, il n'y a pas de data race"         │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  AVANTAGES :                                                │
│  ✓ GARANTIE : pas de data races                             │
│  ✓ Performance = C/C++                                      │
│  ✓ Abstractions sans coût runtime                           │
│  ✓ Erreurs détectées à la compilation                       │
│                                                             │
│  INCONVÉNIENTS :                                            │
│  ✗ Courbe d'apprentissage raide                             │
│  ✗ Temps de compilation longs                               │
│  ✗ "Combat avec le borrow checker"                          │
│  ✗ Syntaxe parfois verbeuse                                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Go - "Simplicité et concurrence pour tous"

```
┌─────────────────────────────────────────────────────────────┐
│                         GO                                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  PHILOSOPHIE :                                              │
│  • Simplicité avant tout                                    │
│  • Concurrence intégrée au langage                          │
│  • "Don't communicate by sharing memory;                    │
│     share memory by communicating"                          │
│  • Compilation rapide                                       │
│                                                             │
│  APPROCHE CONCURRENCE :                                     │
│  • Goroutines (threads ultra-légers)                        │
│  • Channels (communication par messages)                    │
│  • Modèle CSP (Communicating Sequential Processes)          │
│  • select pour multiplexer les channels                     │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  "Lancer 100 000 goroutines ? Pas de problème !"    │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  AVANTAGES :                                                │
│  ✓ Très facile à apprendre                                  │
│  ✓ Goroutines ultra-légères (2 Ko vs 1 Mo pour thread)      │
│  ✓ Channels intégrés au langage                             │
│  ✓ Runtime gère le scheduling                               │
│  ✓ Race detector intégré                                    │
│                                                             │
│  INCONVÉNIENTS :                                            │
│  ✗ Data races POSSIBLES (détectées au runtime, pas compile) │
│  ✗ Garbage collector (latence parfois)                      │
│  ✗ Pas de génériques (ajoutés en Go 1.18, limités)          │
│  ✗ Gestion d'erreurs verbeuse                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## OCaml - "Fonctionnel, élégant, en évolution"

```
┌─────────────────────────────────────────────────────────────┐
│                        OCAML                                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  PHILOSOPHIE :                                              │
│  • Programmation fonctionnelle avec effets contrôlés        │
│  • Immutabilité par défaut = moins de bugs                  │
│  • Système de types puissant                                │
│  • Expressivité et sécurité                                 │
│                                                             │
│  APPROCHE CONCURRENCE :                                     │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  AVANT OCaml 5 (< 2022) :                           │   │
│  │  • GIL (Global Interpreter Lock)                    │   │
│  │  • Un seul thread OCaml à la fois                   │   │
│  │  • Parallelisme via multi-processus                 │   │
│  │                                                     │   │
│  │  DEPUIS OCaml 5 (2022+) :                           │   │
│  │  • Multicore natif !                                │   │
│  │  • Domains (vrais threads parallèles)               │   │
│  │  • Effect handlers (async élégant)                  │   │
│  │  • Révolution pour OCaml                            │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  AVANTAGES :                                                │
│  ✓ Immutabilité = naturellement thread-safe                 │
│  ✓ Système de types évite beaucoup de bugs                  │
│  ✓ Pattern matching puissant                                │
│  ✓ OCaml 5 : vrai parallélisme moderne                      │
│                                                             │
│  INCONVÉNIENTS :                                            │
│  ✗ Écosystème plus petit                                    │
│  ✗ Moins de ressources/tutoriels                            │
│  ✗ OCaml 5 encore récent (écosystème s'adapte)              │
│  ✗ Courbe d'apprentissage (paradigme fonctionnel)           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

# 3. Sécurité Mémoire

## Comparaison des Garanties

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    SÉCURITÉ MÉMOIRE ET THREADS                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│                    DATA RACES    USE-AFTER-FREE   NULL POINTER   DEADLOCK   │
│                    ───────────   ─────────────    ────────────   ────────   │
│  C                 Possible ✗    Possible ✗       Possible ✗     Possible ✗ │
│                    (aucune       (aucune          (aucune        (aucune    │
│                     protect.)     protect.)        protect.)      protect.) │
│                                                                             │
│  RUST              Impossible ✓  Impossible ✓     Impossible ✓   Possible ✗ │
│                    (compile      (ownership)      (Option<T>)    (logique,  │
│                     time)                                         pas mem.) │
│                                                                             │
│  GO                Possible ✗    Impossible ✓     Possible ~     Possible ✗ │
│                    (race         (GC)             (nil existe)   (channels  │
│                     detector)                                     peuvent)  │
│                                                                             │
│  OCAML             Rare ~        Impossible ✓     Impossible ✓   Possible ✗ │
│                    (immutable    (GC)             (Option)       (mutex     │
│                     par défaut)                                   peuvent)  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Détail par Langage

### C - Aucune Protection

```c
// DATA RACE - C ne dit rien !
int counter = 0;

void* increment(void* arg) {
    for (int i = 0; i < 1000000; i++) {
        counter++;  // BUG : non-atomique, data race !
    }
    return NULL;
}

// USE-AFTER-FREE - C ne dit rien !
int* ptr = malloc(sizeof(int));
free(ptr);
*ptr = 42;  // BUG : utilisation après libération !

// Le compilateur C ne signale RIEN.
// Ces bugs apparaissent au runtime (ou pire, jamais clairement).
```

### Rust - Protection à la Compilation

```rust
// DATA RACE - Rust REFUSE de compiler !
let mut counter = 0;

// ERREUR : cannot borrow `counter` as mutable more than once
std::thread::spawn(|| {
    counter += 1;  // Erreur de compilation !
});

// Version correcte avec Arc<Mutex<T>>
let counter = Arc::new(Mutex::new(0));
let counter_clone = Arc::clone(&counter);

std::thread::spawn(move || {
    let mut num = counter_clone.lock().unwrap();
    *num += 1;
});

// USE-AFTER-FREE - Impossible en Rust !
let ptr = Box::new(42);
drop(ptr);
// println!("{}", *ptr);  // ERREUR : use of moved value
```

### Go - Détection au Runtime

```go
// DATA RACE - Go compile, mais race detector le trouve
var counter int

go func() {
    counter++  // Race condition !
}()
counter++  // Race condition !

// go run -race main.go  → détecte la race AU RUNTIME

// Version correcte avec mutex
var mu sync.Mutex
mu.Lock()
counter++
mu.Unlock()

// Ou avec channel (idiomatique Go)
ch := make(chan int, 1)
ch <- 0
go func() {
    val := <-ch
    ch <- val + 1
}()
```

### OCaml - Immutabilité Naturelle

```ocaml
(* Immutable par défaut = pas de data race sur cette donnée *)
let counter = 0  (* Impossible à modifier *)

(* Version mutable explicite *)
let counter = ref 0

(* OCaml 5 avec Domain (threads parallèles) *)
let counter = Atomic.make 0

let _ = Domain.spawn (fun () ->
    Atomic.incr counter
)

(* L'immutabilité par défaut réduit naturellement les bugs *)
```

---

# 4. Primitives de Concurrence

## Vue Comparative

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      PRIMITIVES DE CONCURRENCE                              │
├─────────────────┬─────────────┬─────────────┬─────────────┬─────────────────┤
│   PRIMITIVE     │      C      │    RUST     │     GO      │     OCAML       │
├─────────────────┼─────────────┼─────────────┼─────────────┼─────────────────┤
│ Threads         │ pthread     │ std::thread │ goroutines  │ Domain (v5)     │
│                 │             │             │             │ Thread (limité) │
├─────────────────┼─────────────┼─────────────┼─────────────┼─────────────────┤
│ Mutex           │ pthread_    │ Mutex<T>    │ sync.Mutex  │ Mutex.t         │
│                 │ mutex_t     │             │             │                 │
├─────────────────┼─────────────┼─────────────┼─────────────┼─────────────────┤
│ RW Lock         │ pthread_    │ RwLock<T>   │ sync.RWMutex│ (bibliothèque)  │
│                 │ rwlock_t    │             │             │                 │
├─────────────────┼─────────────┼─────────────┼─────────────┼─────────────────┤
│ Channels        │ (manuel)    │ mpsc        │ chan (natif)│ (bibliothèque)  │
│                 │             │ crossbeam   │             │ Domainslib      │
├─────────────────┼─────────────┼─────────────┼─────────────┼─────────────────┤
│ Atomics         │ stdatomic.h │ std::atomic │ sync/atomic │ Atomic (v5)     │
├─────────────────┼─────────────┼─────────────┼─────────────┼─────────────────┤
│ Condition Var   │ pthread_    │ Condvar     │ sync.Cond   │ Condition.t     │
│                 │ cond_t      │             │             │                 │
├─────────────────┼─────────────┼─────────────┼─────────────┼─────────────────┤
│ Async/Await     │ (non natif) │ async/await │ goroutines  │ Effect handlers │
│                 │             │ + tokio     │ (implicite) │ (v5)            │
├─────────────────┼─────────────┼─────────────┼─────────────┼─────────────────┤
│ Thread Pool     │ (manuel)    │ rayon       │ runtime Go  │ Domainslib      │
│                 │             │ tokio       │ (intégré)   │                 │
├─────────────────┼─────────────┼─────────────┼─────────────┼─────────────────┤
│ Barrier         │ pthread_    │ Barrier     │ sync.       │ (bibliothèque)  │
│                 │ barrier_t   │             │ WaitGroup   │                 │
└─────────────────┴─────────────┴─────────────┴─────────────┴─────────────────┘
```

## Poids des Threads

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    COÛT DE CRÉATION D'UN "THREAD"                           │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  C (pthread)      │████████████████████████████████│  ~1-8 Mo stack         │
│                   │ Thread OS lourd                 │  ~50-100 µs création   │
│                   │                                 │  Limité à ~10 000      │
│                                                                             │
│  RUST (thread)    │████████████████████████████████│  ~2 Mo stack (défaut)  │
│                   │ Thread OS (comme C)             │  ~50-100 µs création   │
│                   │                                 │  Limité à ~10 000      │
│                                                                             │
│  RUST (tokio)     │███│                             │  ~quelques Ko          │
│                   │ Task async légère               │  ~1 µs création        │
│                   │                                 │  Millions possibles    │
│                                                                             │
│  GO (goroutine)   │██│                              │  ~2-8 Ko stack initial │
│                   │ Goroutine ultra-légère          │  ~1 µs création        │
│                   │                                 │  Millions possibles    │
│                                                                             │
│  OCAML (Domain)   │████████████████████████████████│  Thread OS             │
│                   │ Limité au nombre de cœurs       │  Lourd                 │
│                   │                                 │  Quelques dizaines     │
│                                                                             │
│  OCAML (Effect)   │███│                             │  Très léger            │
│                   │ Fiber/coroutine                 │  Beaucoup possible     │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

RÉSUMÉ :
• C, Rust (std) : Threads OS lourds (~10 000 max)
• Rust (tokio), Go : Très légers (millions possibles)
• OCaml 5 : Domains lourds + Effects légers
```

---

# 5. Exemples de Code Comparés

## Exemple 1 : Compteur Partagé (Problème Classique)

### C

```c
#include <stdio.h>
#include <pthread.h>

int counter = 0;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

void* increment(void* arg) {
    for (int i = 0; i < 100000; i++) {
        pthread_mutex_lock(&mutex);
        counter++;
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() {
    pthread_t t1, t2;
    
    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);
    
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    
    printf("Counter: %d\n", counter);  // 200000
    
    pthread_mutex_destroy(&mutex);
    return 0;
}

// ⚠️  Si on oublie le mutex → data race silencieuse
// ⚠️  Si on oublie unlock → deadlock
// ⚠️  Pas de vérification du compilateur
```

### Rust

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let counter = Arc::new(Mutex::new(0));
    let mut handles = vec![];

    for _ in 0..2 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            for _ in 0..100000 {
                let mut num = counter.lock().unwrap();
                *num += 1;
            }  // unlock automatique ici (RAII)
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Counter: {}", *counter.lock().unwrap());  // 200000
}

// ✓ Sans Arc<Mutex<T>> → erreur de compilation
// ✓ Unlock automatique (impossible d'oublier)
// ✓ Le compilateur GARANTIT l'absence de data race
```

### Go

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var counter int
    var mu sync.Mutex
    var wg sync.WaitGroup

    for i := 0; i < 2; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            for j := 0; j < 100000; j++ {
                mu.Lock()
                counter++
                mu.Unlock()
            }
        }()
    }

    wg.Wait()
    fmt.Println("Counter:", counter)  // 200000
}

// ⚠️  Si on oublie le mutex → data race (détectée avec -race)
// ✓ Simple et lisible
// ✓ go run -race main.go pour détecter les races
```

### Go (Idiomatique avec Channel)

```go
package main

import "fmt"

func main() {
    counter := make(chan int, 1)
    counter <- 0
    done := make(chan bool)

    increment := func() {
        for i := 0; i < 100000; i++ {
            val := <-counter
            counter <- val + 1
        }
        done <- true
    }

    go increment()
    go increment()

    <-done
    <-done

    fmt.Println("Counter:", <-counter)  // 200000
}

// ✓ Pas de mutex explicite
// ✓ Communication par messages
// ✓ Plus "Go idiomatique"
```

### OCaml 5

```ocaml
let counter = Atomic.make 0

let increment () =
  for _ = 1 to 100000 do
    Atomic.incr counter
  done

let () =
  let d1 = Domain.spawn increment in
  let d2 = Domain.spawn increment in
  Domain.join d1;
  Domain.join d2;
  Printf.printf "Counter: %d\n" (Atomic.get counter)
  (* 200000 *)

(* ✓ Atomic évite le besoin de mutex pour un compteur
   ✓ Domain = vrai parallélisme sur multi-cœurs
   ✓ Syntaxe concise *)
```

---

## Exemple 2 : Producteur-Consommateur avec Channels

### C (Manuel, Complexe)

```c
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>

#define BUFFER_SIZE 10

int buffer[BUFFER_SIZE];
int count = 0;
int in = 0, out = 0;

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t not_full = PTHREAD_COND_INITIALIZER;
pthread_cond_t not_empty = PTHREAD_COND_INITIALIZER;

void* producer(void* arg) {
    for (int i = 0; i < 20; i++) {
        pthread_mutex_lock(&mutex);
        
        while (count == BUFFER_SIZE) {
            pthread_cond_wait(&not_full, &mutex);
        }
        
        buffer[in] = i;
        in = (in + 1) % BUFFER_SIZE;
        count++;
        printf("Produced: %d\n", i);
        
        pthread_cond_signal(&not_empty);
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

void* consumer(void* arg) {
    for (int i = 0; i < 20; i++) {
        pthread_mutex_lock(&mutex);
        
        while (count == 0) {
            pthread_cond_wait(&not_empty, &mutex);
        }
        
        int item = buffer[out];
        out = (out + 1) % BUFFER_SIZE;
        count--;
        printf("Consumed: %d\n", item);
        
        pthread_cond_signal(&not_full);
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() {
    pthread_t prod, cons;
    pthread_create(&prod, NULL, producer, NULL);
    pthread_create(&cons, NULL, consumer, NULL);
    pthread_join(prod, NULL);
    pthread_join(cons, NULL);
    return 0;
}

// ⚠️  ~50 lignes pour un simple producteur-consommateur
// ⚠️  Facile de se tromper avec les conditions
```

### Rust

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    // Producteur
    let producer = thread::spawn(move || {
        for i in 0..20 {
            tx.send(i).unwrap();
            println!("Produced: {}", i);
        }
    });

    // Consommateur
    let consumer = thread::spawn(move || {
        for received in rx {
            println!("Consumed: {}", received);
        }
    });

    producer.join().unwrap();
    consumer.join().unwrap();
}

// ✓ ~20 lignes
// ✓ Channel typé et sûr
// ✓ Fermeture automatique du channel
```

### Go

```go
package main

import "fmt"

func main() {
    ch := make(chan int, 10)

    // Producteur
    go func() {
        for i := 0; i < 20; i++ {
            ch <- i
            fmt.Println("Produced:", i)
        }
        close(ch)
    }()

    // Consommateur
    for item := range ch {
        fmt.Println("Consumed:", item)
    }
}

// ✓ ~15 lignes !
// ✓ Syntaxe native très simple
// ✓ range sur channel = idiomatique
```

### OCaml 5

```ocaml
module Chan = Domainslib.Chan

let () =
  let ch = Chan.make_bounded 10 in
  
  (* Producteur *)
  let producer = Domain.spawn (fun () ->
    for i = 0 to 19 do
      Chan.send ch i;
      Printf.printf "Produced: %d\n%!" i
    done
  ) in
  
  (* Consommateur *)
  for _ = 0 to 19 do
    let item = Chan.recv ch in
    Printf.printf "Consumed: %d\n%!" item
  done;
  
  Domain.join producer

(* ✓ Bibliothèque Domainslib pour channels
   ✓ Syntaxe fonctionnelle *)
```

---

## Exemple 3 : Parallélisme de Données (Map Parallèle)

### C (avec OpenMP)

```c
#include <stdio.h>
#include <omp.h>

int main() {
    int data[1000000];
    int results[1000000];
    
    // Initialisation
    for (int i = 0; i < 1000000; i++) {
        data[i] = i;
    }
    
    // Map parallèle avec OpenMP
    #pragma omp parallel for
    for (int i = 0; i < 1000000; i++) {
        results[i] = data[i] * data[i];
    }
    
    printf("results[999999] = %d\n", results[999999]);
    return 0;
}

// gcc -fopenmp main.c
// ✓ Simple avec OpenMP
// ⚠️  Nécessite directive de compilation
```

### Rust (avec Rayon)

```rust
use rayon::prelude::*;

fn main() {
    let data: Vec<i64> = (0..1_000_000).collect();
    
    // Map parallèle - une seule ligne !
    let results: Vec<i64> = data.par_iter()
        .map(|x| x * x)
        .collect();
    
    println!("results[999999] = {}", results[999_999]);
}

// ✓ Extrêmement simple
// ✓ Juste changer .iter() en .par_iter()
// ✓ Rayon gère tout automatiquement
```

### Go

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    data := make([]int, 1000000)
    results := make([]int, 1000000)
    
    for i := range data {
        data[i] = i
    }
    
    var wg sync.WaitGroup
    numWorkers := 8
    chunkSize := len(data) / numWorkers
    
    for w := 0; w < numWorkers; w++ {
        wg.Add(1)
        start := w * chunkSize
        end := start + chunkSize
        if w == numWorkers-1 {
            end = len(data)
        }
        
        go func(start, end int) {
            defer wg.Done()
            for i := start; i < end; i++ {
                results[i] = data[i] * data[i]
            }
        }(start, end)
    }
    
    wg.Wait()
    fmt.Println("results[999999] =", results[999999])
}

// ⚠️  Plus verbeux que Rust
// ⚠️  Division manuelle du travail
// ✓ Mais reste raisonnable
```

### OCaml 5

```ocaml
module T = Domainslib.Task

let () =
  let pool = T.setup_pool ~num_domains:4 () in
  
  let data = Array.init 1_000_000 (fun i -> i) in
  let results = Array.make 1_000_000 0 in
  
  T.run pool (fun () ->
    T.parallel_for pool ~start:0 ~finish:(Array.length data - 1)
      ~body:(fun i -> results.(i) <- data.(i) * data.(i))
  );
  
  Printf.printf "results[999999] = %d\n" results.(999_999);
  T.teardown_pool pool

(* ✓ Domainslib fournit parallel_for
   ✓ Pool de domains réutilisable *)
```

---

# 6. Performance

## Benchmarks Typiques

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    PERFORMANCE RELATIVE (indicatif)                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  CALCUL INTENSIF (CPU-bound) :                                              │
│  ────────────────────────────────────────────────────────────────────────── │
│  C      │████████████████████████████████████████│ 100%  (référence)        │
│  Rust   │███████████████████████████████████████ │ 98-102% (équivalent)     │
│  Go     │██████████████████████████████████      │ 80-90%                   │
│  OCaml  │█████████████████████████████████       │ 75-95%                   │
│                                                                             │
│  CRÉATION DE THREADS/TÂCHES :                                               │
│  ────────────────────────────────────────────────────────────────────────── │
│  C (pthread)    │████████████████████████████████│ ~50-100 µs               │
│  Rust (thread)  │████████████████████████████████│ ~50-100 µs               │
│  Rust (tokio)   │██                              │ ~1 µs                    │
│  Go (goroutine) │█                               │ ~0.3-1 µs                │
│  OCaml (Domain) │████████████████████████████████│ ~100 µs                  │
│                                                                             │
│  I/O CONCURRENTES (ex: 10K connexions) :                                    │
│  ────────────────────────────────────────────────────────────────────────── │
│  C (epoll)      │████████████████████████████████│ Excellent (manuel)       │
│  Rust (tokio)   │███████████████████████████████ │ Excellent                │
│  Go             │██████████████████████████████  │ Très bon                 │
│  OCaml (Lwt)    │████████████████████████        │ Bon                      │
│                                                                             │
│  UTILISATION MÉMOIRE PAR TÂCHE :                                            │
│  ────────────────────────────────────────────────────────────────────────── │
│  C/Rust (thread)│████████████████████████████████│ ~1-8 Mo                  │
│  Rust (tokio)   │███                             │ ~quelques Ko             │
│  Go (goroutine) │██                              │ ~2-8 Ko                  │
│  OCaml (Effect) │███                             │ ~quelques Ko             │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Analyse

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│  C :                                                                        │
│  • Performance maximale si bien codé                                        │
│  • Aucun overhead d'abstraction                                             │
│  • Mais : temps de développement long, bugs probables                       │
│                                                                             │
│  RUST :                                                                     │
│  • Équivalent à C en performance                                            │
│  • Abstractions "zero-cost"                                                 │
│  • async/await très efficace avec tokio                                     │
│  • Le meilleur des deux mondes (perf + sécurité)                            │
│                                                                             │
│  GO :                                                                       │
│  • ~10-20% plus lent que C/Rust en calcul pur                               │
│  • MAIS : développement 2-3x plus rapide                                    │
│  • Goroutines extrêmement efficaces                                         │
│  • Excellent pour serveurs I/O-bound                                        │
│                                                                             │
│  OCAML :                                                                    │
│  • Performance correcte (souvent sous-estimée)                              │
│  • GC optimisé, allocation très rapide                                      │
│  • OCaml 5 améliore significativement le parallélisme                       │
│  • Idéal pour calcul symbolique, compilateurs                               │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# 7. Facilité d'Utilisation

## Courbe d'Apprentissage

```
DIFFICULTÉ D'APPRENTISSAGE DE LA CONCURRENCE

Facile                                                              Difficile
   │                                                                     │
   ▼                                                                     ▼
   ├──────────┬──────────┬──────────┬──────────┬──────────┬──────────┤
   │    GO    │  OCAML   │    C     │   RUST   │  RUST    │    C     │
   │ (basics) │ (basics) │ (basics) │ (basics) │ (avancé) │ (avancé) │
   └──────────┴──────────┴──────────┴──────────┴──────────┴──────────┘
   
   GO      : Goroutines + channels s'apprennent en 1 jour
   OCAML   : Concepts de base simples, paradigme fonctionnel à acquérir
   C       : Concepts simples, MAIS très facile de faire des bugs
   RUST    : Borrow checker = obstacle initial, puis ça devient naturel
```

## Comparaison de la Complexité du Code

```
┌─────────────────────────────────────────────────────────────────────────────┐
│              LIGNES DE CODE POUR TÂCHES ÉQUIVALENTES                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  TÂCHE                           │   C   │ RUST  │  GO   │ OCAML           │
│  ────────────────────────────────┼───────┼───────┼───────┼─────────────────│
│  Compteur thread-safe            │  30   │  15   │  12   │  10             │
│  Producteur-consommateur         │  50   │  20   │  15   │  20             │
│  Web server basique concurrent   │ 200+  │  50   │  30   │  40             │
│  Pool de workers                 │ 150   │  40   │  25   │  30             │
│                                                                             │
│  REMARQUE : Moins de lignes ≠ toujours mieux                                │
│             Mais indique la "friction" du langage                           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Messages d'Erreur

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    QUALITÉ DES ERREURS DE CONCURRENCE                       │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  C :                                                                        │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  Segmentation fault (core dumped)                                   │   │
│  │  // Bonne chance pour trouver le bug...                             │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
│  RUST :                                                                     │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  error[E0382]: borrow of moved value: `data`                        │   │
│  │    --> src/main.rs:15:20                                            │   │
│  │     |                                                               │   │
│  │  14 |     let handle = thread::spawn(move || {                      │   │
│  │     |                                ------- value moved here       │   │
│  │  15 |     println!("{}", data);                                     │   │
│  │     |                    ^^^^ value borrowed here after move        │   │
│  │     |                                                               │   │
│  │  help: consider cloning the value...                                │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│  // Message clair + suggestion de correction !                             │
│                                                                             │
│  GO :                                                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  WARNING: DATA RACE                                                 │   │
│  │  Write at 0x00c0000a4000 by goroutine 7:                            │   │
│  │    main.main.func1()                                                │   │
│  │        /tmp/main.go:12 +0x38                                        │   │
│  │  Previous write at 0x00c0000a4000 by goroutine 6:                   │   │
│  │    main.main.func1()                                                │   │
│  │        /tmp/main.go:12 +0x38                                        │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│  // Détecté au runtime avec -race, assez informatif                        │
│                                                                             │
│  OCAML :                                                                    │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  Error: This expression has type int but an expression was expected │   │
│  │         of type int ref                                             │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│  // Système de types attrape beaucoup d'erreurs                            │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# 8. Écosystème et Outils

## Bibliothèques de Concurrence

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         ÉCOSYSTÈME                                          │
├────────────────┬────────────────────────────────────────────────────────────┤
│                │                                                            │
│  C             │  • pthreads (standard POSIX)                               │
│                │  • OpenMP (parallélisme données)                           │
│                │  • libuv (async I/O, utilisé par Node.js)                  │
│                │  • libev, libevent                                         │
│                │  → Mature mais fragmenté, beaucoup de choix                │
│                │                                                            │
├────────────────┼────────────────────────────────────────────────────────────┤
│                │                                                            │
│  RUST          │  • std::thread, std::sync (standard)                       │
│                │  • tokio (async runtime #1)                                │
│                │  • async-std (alternative à tokio)                         │
│                │  • rayon (parallélisme données)                            │
│                │  • crossbeam (structures lock-free)                        │
│                │  • parking_lot (mutex optimisés)                           │
│                │  → Écosystème riche et moderne                             │
│                │                                                            │
├────────────────┼────────────────────────────────────────────────────────────┤
│                │                                                            │
│  GO            │  • Tout est intégré au langage !                           │
│                │  • sync (Mutex, WaitGroup, etc.)                           │
│                │  • context (annulation, timeouts)                          │
│                │  • golang.org/x/sync (errgroup, semaphore)                 │
│                │  → Simple : peu de choix à faire                           │
│                │                                                            │
├────────────────┼────────────────────────────────────────────────────────────┤
│                │                                                            │
│  OCAML         │  • Domain, Atomic (OCaml 5 stdlib)                         │
│                │  • Domainslib (parallel_for, channels)                     │
│                │  • Lwt (async I/O, monadic)                                │
│                │  • Async (Jane Street, alternatif à Lwt)                   │
│                │  • Eio (nouveau, basé sur effects)                         │
│                │  → En transition vers OCaml 5                              │
│                │                                                            │
└────────────────┴────────────────────────────────────────────────────────────┘
```

## Outils de Débogage

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      OUTILS DE DÉBOGAGE CONCURRENCE                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  C :                                                                        │
│  ├── Valgrind (Helgrind, DRD) : détection data races                        │
│  ├── ThreadSanitizer (TSan) : détection races (compile-time)                │
│  ├── GDB : débogage threads                                                 │
│  └── Intel Inspector : analyse commerciale                                  │
│                                                                             │
│  RUST :                                                                     │
│  ├── Le compilateur ! (attrape data races avant exécution)                  │
│  ├── cargo miri : interpréteur qui détecte UB                               │
│  ├── ThreadSanitizer (moins utile car compile empêche races)                │
│  └── tokio-console : visualisation tâches async                             │
│                                                                             │
│  GO :                                                                       │
│  ├── go run -race : Race Detector intégré (excellent !)                     │
│  ├── pprof : profiling goroutines                                           │
│  ├── trace : visualisation exécution                                        │
│  └── delve : débogueur avec support goroutines                              │
│                                                                             │
│  OCAML :                                                                    │
│  ├── Le système de types (prévient beaucoup de bugs)                        │
│  ├── ocamldebug : débogueur                                                 │
│  ├── Memtrace : profiling mémoire                                           │
│  └── Outils en développement pour OCaml 5                                   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

# 9. Cas d'Usage Recommandés

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    QUEL LANGAGE POUR QUEL USAGE ?                           │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ╔═══════════════════════════════════════════════════════════════════════╗ │
│  ║  CHOISIR C QUAND :                                                    ║ │
│  ╠═══════════════════════════════════════════════════════════════════════╣ │
│  ║  • Systèmes embarqués avec contraintes extrêmes                       ║ │
│  ║  • Noyaux de systèmes d'exploitation                                  ║ │
│  ║  • Interfaçage bas niveau (drivers)                                   ║ │
│  ║  • Code legacy à maintenir                                            ║ │
│  ║  • Équipe très expérimentée en C                                      ║ │
│  ║                                                                       ║ │
│  ║  Exemples : Linux kernel, SQLite, Redis                               ║ │
│  ╚═══════════════════════════════════════════════════════════════════════╝ │
│                                                                             │
│  ╔═══════════════════════════════════════════════════════════════════════╗ │
│  ║  CHOISIR RUST QUAND :                                                 ║ │
│  ╠═══════════════════════════════════════════════════════════════════════╣ │
│  ║  • Performance critique ET sécurité importante                        ║ │
│  ║  • Systèmes où les bugs coûtent cher                                  ║ │
│  ║  • Concurrence intensive avec état partagé                            ║ │
│  ║  • Remplacement de C/C++ pour nouveau projet                          ║ │
│  ║  • WebAssembly, CLI tools performants                                 ║ │
│  ║                                                                       ║ │
│  ║  Exemples : Servo, Ripgrep, Discord, Cloudflare, Dropbox              ║ │
│  ╚═══════════════════════════════════════════════════════════════════════╝ │
│                                                                             │
│  ╔═══════════════════════════════════════════════════════════════════════╗ │
│  ║  CHOISIR GO QUAND :                                                   ║ │
│  ╠═══════════════════════════════════════════════════════════════════════╣ │
│  ║  • Microservices et APIs                                              ║ │
│  ║  • Outils DevOps / Infrastructure                                     ║ │
│  ║  • Serveurs réseau (beaucoup de connexions)                           ║ │
│  ║  • Équipe avec niveaux variés                                         ║ │
│  ║  • Time-to-market important                                           ║ │
│  ║  • Cloud-native applications                                          ║ │
│  ║                                                                       ║ │
│  ║  Exemples : Docker, Kubernetes, Terraform, Hugo, Prometheus           ║ │
│  ╚═══════════════════════════════════════════════════════════════════════╝ │
│                                                                             │
│  ╔═══════════════════════════════════════════════════════════════════════╗ │
│  ║  CHOISIR OCAML QUAND :                                                ║ │
│  ╠═══════════════════════════════════════════════════════════════════════╣ │
│  ║  • Compilateurs et langages                                           ║ │
│  ║  • Analyse statique, vérification formelle                            ║ │
│  ║  • Finance (trading haute fréquence - Jane Street)                    ║ │
│  ║  • Systèmes avec logique complexe                                     ║ │
│  ║  • Correctness > tout le reste                                        ║ │
│  ║                                                                       ║ │
│  ║  Exemples : Flow (Facebook), Hack, Coq, MirageOS, Jane Street         ║ │
│  ╚═══════════════════════════════════════════════════════════════════════╝ │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Arbre de Décision

```
                    Besoin de concurrence ?
                            │
                            ▼
            ┌───────────────┴───────────────┐
            │                               │
       Performance                    Productivité
       critique ?                     prioritaire ?
            │                               │
            ▼                               ▼
    ┌───────┴───────┐               ┌───────┴───────┐
    │               │               │               │
Contrôle         Sécurité      Simple &        Fonctionnel
total ?          mémoire ?     Rapide ?        & Sûr ?
    │               │               │               │
    ▼               ▼               ▼               ▼
┌───────┐      ┌───────┐       ┌───────┐      ┌───────┐
│   C   │      │ RUST  │       │  GO   │      │ OCAML │
└───────┘      └───────┘       └───────┘      └───────┘

Notes :
• C si : équipe experte, contraintes extrêmes, legacy
• Rust si : perf + sécurité, systèmes critiques
• Go si : serveurs, microservices, équipe mixte
• OCaml si : compilateurs, finance, correctness
```

---

# 10. Tableau Récapitulatif

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                           TABLEAU RÉCAPITULATIF COMPLET                                │
├──────────────────────┬──────────────┬──────────────┬──────────────┬──────────────────┤
│                      │      C       │     RUST     │      GO      │      OCAML       │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ MODÈLE PRINCIPAL     │ Threads OS   │ Threads OS   │ Goroutines   │ Domains +        │
│                      │ + mutex      │ + async      │ + channels   │ Effects          │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ SÉCURITÉ DATA RACE   │ Aucune       │ Compile-time │ Runtime      │ Type system +    │
│                      │              │ (garanti)    │ (-race flag) │ immutabilité     │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ GESTION MÉMOIRE      │ Manuel       │ Ownership    │ GC           │ GC               │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ PERFORMANCE CPU      │ ★★★★★        │ ★★★★★        │ ★★★★☆        │ ★★★★☆            │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ PERFORMANCE I/O      │ ★★★★★        │ ★★★★★        │ ★★★★★        │ ★★★★☆            │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ FACILITÉ APPRENTISS. │ ★★★☆☆        │ ★★☆☆☆        │ ★★★★★        │ ★★★☆☆            │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ FACILITÉ CONCURRENCE │ ★★☆☆☆        │ ★★★☆☆        │ ★★★★★        │ ★★★☆☆            │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ TAILLE ÉCOSYSTÈME    │ ★★★★★        │ ★★★★☆        │ ★★★★☆        │ ★★★☆☆            │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ DEBUGGING CONCUR.    │ ★★☆☆☆        │ ★★★★★        │ ★★★★☆        │ ★★★☆☆            │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ SCALABILITÉ THREADS  │ ~10K         │ ~10K (std)   │ ~1M          │ ~Cœurs (Domain)  │
│                      │              │ ~1M (tokio)  │              │ ~1M (Effects)    │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ ASYNC NATIF          │ Non          │ Oui (1.39+)  │ Oui          │ Oui (v5)         │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ CAS D'USAGE TYPIQUE  │ Embedded,    │ Systems,     │ Servers,     │ Compilers,       │
│                      │ Kernels,     │ WebAssembly, │ DevOps,      │ Finance,         │
│                      │ Drivers      │ CLI, Perf    │ Cloud        │ Formal verif.    │
├──────────────────────┼──────────────┼──────────────┼──────────────┼──────────────────┤
│ ENTREPRISES CONNUES  │ (partout)    │ Mozilla,     │ Google,      │ Jane Street,     │
│                      │              │ Discord,     │ Uber,        │ Facebook,        │
│                      │              │ Cloudflare   │ Twitch       │ Docker           │
└──────────────────────┴──────────────┴──────────────┴──────────────┴──────────────────┘
```

---

# Conclusion

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           EN RÉSUMÉ                                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  🔧 C = Le vétéran                                                          │
│     Puissance maximale, responsabilité maximale.                            │
│     Pour experts qui savent ce qu'ils font.                                 │
│                                                                             │
│  🦀 RUST = Le gardien                                                       │
│     Performance de C + sécurité garantie par le compilateur.                │
│     Investissement initial, puis développement serein.                      │
│                                                                             │
│  🐹 GO = Le pragmatique                                                     │
│     Concurrence facile, productive, pour tous.                              │
│     Idéal pour équipes et projets nécessitant rapidité de développement.    │
│                                                                             │
│  🐫 OCAML = L'élégant                                                       │
│     Fonctionnel, expressif, correctness-oriented.                           │
│     Pour problèmes complexes où la logique prime.                           │
│                                                                             │
│  ═══════════════════════════════════════════════════════════════════════   │
│                                                                             │
│  MON CONSEIL POUR DÉBUTER :                                                 │
│                                                                             │
│  1. Commencer avec GO pour comprendre les concepts                          │
│     (goroutines, channels, race detector)                                   │
│                                                                             │
│  2. Passer à RUST pour comprendre la sécurité en profondeur                 │
│     (ownership, borrow checker, Send/Sync)                                  │
│                                                                             │
│  3. Explorer C si besoin de comprendre le bas niveau                        │
│     (pthreads, ce que les abstractions cachent)                             │
│                                                                             │
│  4. Découvrir OCAML pour une perspective fonctionnelle                      │
│     (immutabilité, effects, autre façon de penser)                          │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

*Cette comparaison reflète l'état des langages en 2024-2025. Les écosystèmes évoluent constamment !*