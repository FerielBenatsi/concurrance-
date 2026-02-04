# Cours Complet : Architecture des Ordinateurs et Programmation Concurrente
## Guide pour Débutants avec Vocabulaire Expliqué

---

# Table des Matières

1. [Introduction : Pourquoi ce cours ?](#partie-1--introduction)
2. [L'Ordinateur : Vue d'Ensemble](#partie-2--lordinateur-vue-densemble)
3. [Le Processeur (CPU) en Détail](#partie-3--le-processeur-cpu-en-détail)
4. [La Mémoire : Hiérarchie et Fonctionnement](#partie-4--la-mémoire)
5. [Du Mono-cœur au Multi-cœur](#partie-5--du-mono-cœur-au-multi-cœur)
6. [Processus et Threads](#partie-6--processus-et-threads)
7. [Concurrence vs Parallélisme](#partie-7--concurrence-vs-parallélisme)
8. [Les Problèmes de la Concurrence](#partie-8--les-problèmes-de-la-concurrence)
9. [Les Solutions de Synchronisation](#partie-9--les-solutions-de-synchronisation)
10. [Vocabulaire Complet A-Z](#partie-10--glossaire-complet)

---

# Partie 1 : Introduction

## Pourquoi comprendre l'architecture ?

Imaginez que vous êtes chef cuisinier. Pour bien cuisiner, vous devez comprendre :
- Comment fonctionne votre cuisine (l'ordinateur)
- Combien de fourneaux vous avez (les cœurs)
- Comment organiser vos commis (les threads)
- Comment éviter qu'ils se marchent dessus (la synchronisation)

La **programmation concurrente**, c'est l'art de faire plusieurs choses "en même temps" sur un ordinateur. Pour la maîtriser, il faut d'abord comprendre comment l'ordinateur fonctionne.

## Ce que vous allez apprendre

```
┌─────────────────────────────────────────────────────────┐
│                    VOTRE PARCOURS                       │
├─────────────────────────────────────────────────────────┤
│  1. Comment un ordinateur est construit                 │
│  2. Ce qu'est vraiment un processeur                    │
│  3. Comment la mémoire fonctionne                       │
│  4. Pourquoi on a plusieurs cœurs aujourd'hui           │
│  5. La différence entre processus et thread             │
│  6. Ce que "concurrent" et "parallèle" signifient       │
│  7. Les problèmes classiques et leurs solutions         │
│  8. Tout le vocabulaire technique expliqué              │
└─────────────────────────────────────────────────────────┘
```

---

# Partie 2 : L'Ordinateur Vue d'Ensemble

## 2.1 Les Composants Principaux

Un ordinateur est composé de plusieurs parties qui travaillent ensemble :

```
┌────────────────────────────────────────────────────────────────┐
│                        ORDINATEUR                              │
│                                                                │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐     │
│  │     CPU      │◄──►│   MÉMOIRE    │◄──►│   STOCKAGE   │     │
│  │ (Processeur) │    │    (RAM)     │    │ (SSD/Disque) │     │
│  └──────────────┘    └──────────────┘    └──────────────┘     │
│         ▲                   ▲                   ▲              │
│         │                   │                   │              │
│         ▼                   ▼                   ▼              │
│  ┌────────────────────────────────────────────────────────┐   │
│  │                    BUS (Autoroute de données)          │   │
│  └────────────────────────────────────────────────────────┘   │
│         ▲                   ▲                   ▲              │
│         ▼                   ▼                   ▼              │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐     │
│  │ Carte Graph. │    │   Réseau     │    │  Périphér.   │     │
│  │    (GPU)     │    │   (WiFi...)  │    │ (Clavier...) │     │
│  └──────────────┘    └──────────────┘    └──────────────┘     │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### Le CPU (Central Processing Unit) - Le Cerveau

```
┌─────────────────────────────────────────┐
│                  CPU                    │
│  • Exécute les instructions             │
│  • Fait les calculs                     │
│  • Prend les décisions                  │
│  • Coordonne tout le reste              │
│                                         │
│  Analogie : Le chef d'orchestre         │
└─────────────────────────────────────────┘
```

**En français simple** : Le CPU est le "cerveau" de l'ordinateur. C'est lui qui exécute votre programme, instruction par instruction.

### La RAM (Random Access Memory) - La Mémoire Vive

```
┌─────────────────────────────────────────┐
│                  RAM                    │
│  • Stockage temporaire ultra-rapide     │
│  • Contient les programmes en cours     │
│  • Contient les données utilisées       │
│  • S'efface quand on éteint             │
│                                         │
│  Analogie : Le bureau de travail        │
└─────────────────────────────────────────┘
```

**En français simple** : La RAM, c'est comme votre bureau. Vous y posez les documents sur lesquels vous travaillez actuellement. C'est rapide d'accès mais limité en espace.

### Le Stockage (SSD/Disque Dur) - La Mémoire Permanente

```
┌─────────────────────────────────────────┐
│              STOCKAGE                   │
│  • Garde les données même éteint        │
│  • Grande capacité                      │
│  • Plus lent que la RAM                 │
│  • Contient l'OS, fichiers, programmes  │
│                                         │
│  Analogie : L'armoire de rangement      │
└─────────────────────────────────────────┘
```

**En français simple** : Le stockage, c'est votre armoire. Vous y rangez tout, c'est permanent, mais il faut du temps pour aller chercher quelque chose.

## 2.2 Comment ils Communiquent : Le Bus

Le **bus** est l'autoroute qui relie tous les composants.

```
        VITESSE DE COMMUNICATION
        
   CPU ◄────────────────────────────► RAM
              Très rapide
              ~50 Go/s
              
   RAM ◄────────────────────────────► SSD
              Rapide
              ~3-7 Go/s
              
   SSD ◄────────────────────────────► Disque Dur
              Lent
              ~0.1-0.5 Go/s
```

**Point important** : Plus on s'éloigne du CPU, plus c'est lent. C'est pourquoi on a une "hiérarchie" de mémoire.

## 2.3 Analogie Complète : La Cuisine

```
┌─────────────────────────────────────────────────────────────┐
│                    ANALOGIE : LA CUISINE                    │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   COMPOSANT ORDI    │    ÉQUIVALENT CUISINE                │
│   ─────────────────────────────────────────────────────    │
│   CPU               │    Le cuisinier (celui qui travaille)│
│   Cœurs du CPU      │    Nombre de cuisiniers              │
│   RAM               │    Le plan de travail                │
│   Cache             │    Les ingrédients sous la main      │
│   SSD/Disque        │    Le réfrigérateur/garde-manger     │
│   Programme         │    La recette à suivre               │
│   Données           │    Les ingrédients                   │
│   Thread            │    Une tâche (couper, cuire...)      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

# Partie 3 : Le Processeur (CPU) en Détail

## 3.1 Structure Interne du CPU

Le CPU n'est pas un bloc monolithique. Il contient plusieurs parties :

```
┌─────────────────────────────────────────────────────────────────┐
│                           CPU                                   │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                    UNITÉ DE CONTRÔLE                      │ │
│  │         (Chef d'orchestre - dirige tout)                  │ │
│  └───────────────────────────────────────────────────────────┘ │
│                              │                                  │
│         ┌────────────────────┼────────────────────┐            │
│         ▼                    ▼                    ▼            │
│  ┌─────────────┐      ┌─────────────┐      ┌─────────────┐    │
│  │    ALU      │      │    FPU      │      │  REGISTRES  │    │
│  │  (Calculs   │      │  (Calculs   │      │  (Mémoire   │    │
│  │  entiers)   │      │  décimaux)  │      │  ultra-     │    │
│  │             │      │             │      │  rapide)    │    │
│  │ +, -, ×, ÷  │      │ 3.14159...  │      │             │    │
│  └─────────────┘      └─────────────┘      └─────────────┘    │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐ │
│  │                      CACHE L1                              │ │
│  │            (Mémoire très proche et rapide)                 │ │
│  └───────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### L'Unité de Contrôle (Control Unit)

```
┌─────────────────────────────────────────┐
│          UNITÉ DE CONTRÔLE              │
│                                         │
│  Rôle :                                 │
│  • Lit les instructions du programme    │
│  • Décode ce qu'il faut faire           │
│  • Envoie les ordres aux autres unités  │
│  • Synchronise tout                     │
│                                         │
│  C'est le "chef d'orchestre" du CPU     │
└─────────────────────────────────────────┘
```

### L'ALU (Arithmetic Logic Unit) - Unité Arithmétique et Logique

```
┌─────────────────────────────────────────┐
│                 ALU                     │
│                                         │
│  Opérations arithmétiques :             │
│  • Addition      : 5 + 3 = 8            │
│  • Soustraction  : 5 - 3 = 2            │
│  • Multiplication: 5 × 3 = 15           │
│  • Division      : 6 ÷ 3 = 2            │
│                                         │
│  Opérations logiques :                  │
│  • ET  (AND) : vrai ET vrai = vrai      │
│  • OU  (OR)  : vrai OU faux = vrai      │
│  • NON (NOT) : NON vrai = faux          │
│  • Comparaisons : <, >, ==, !=          │
│                                         │
│  Travaille avec des ENTIERS             │
└─────────────────────────────────────────┘
```

### La FPU (Floating Point Unit) - Unité à Virgule Flottante

```
┌─────────────────────────────────────────┐
│                 FPU                     │
│                                         │
│  Calculs avec nombres décimaux :        │
│  • 3.14159 × 2.5 = 7.853975             │
│  • Racines carrées                      │
│  • Fonctions trigonométriques           │
│  • Calculs scientifiques                │
│                                         │
│  Essentiel pour :                       │
│  • Jeux vidéo (graphismes 3D)           │
│  • Simulations scientifiques            │
│  • Intelligence artificielle            │
└─────────────────────────────────────────┘
```

### Les Registres - Mémoire Ultra-Rapide

```
┌─────────────────────────────────────────┐
│              REGISTRES                  │
│                                         │
│  • Petite mémoire DANS le CPU           │
│  • Extrêmement rapide (1 cycle)         │
│  • Capacité très limitée                │
│  • Stocke les données en cours d'usage  │
│                                         │
│  Exemple sur CPU 64 bits :              │
│  ┌────────────────────────────────────┐ │
│  │ RAX : 0x00000000DEADBEEF          │ │
│  │ RBX : 0x0000000012345678          │ │
│  │ RCX : 0x0000000000000042          │ │
│  │ ...                                │ │
│  └────────────────────────────────────┘ │
│                                         │
│  Analogie : Les mains du cuisinier      │
│  (ce qu'il tient à l'instant T)         │
└─────────────────────────────────────────┘
```

## 3.2 Le Cycle d'Instruction

Le CPU répète sans cesse le même cycle :

```
    ┌─────────────────────────────────────────────────────┐
    │              CYCLE D'INSTRUCTION                    │
    │                                                     │
    │   ┌─────────┐   ┌─────────┐   ┌─────────┐         │
    │   │ FETCH   │──►│ DECODE  │──►│ EXECUTE │         │
    │   │(Chercher)│   │(Décoder)│   │(Exécuter)│        │
    │   └─────────┘   └─────────┘   └─────────┘         │
    │        │                            │              │
    │        └────────────────────────────┘              │
    │                 (Recommencer)                      │
    └─────────────────────────────────────────────────────┘

    1. FETCH (Récupérer)
       → Aller chercher l'instruction en mémoire
       
    2. DECODE (Décoder)  
       → Comprendre ce que l'instruction demande
       
    3. EXECUTE (Exécuter)
       → Faire le travail demandé
       
    4. (Optionnel) WRITE BACK (Écrire le résultat)
       → Sauvegarder le résultat
```

### Exemple Concret

```
Programme : calculer a = 5 + 3

Instruction 1 : LOAD 5 dans registre R1
    FETCH   → Lire "LOAD 5, R1" depuis la mémoire
    DECODE  → Comprendre : mettre 5 dans R1
    EXECUTE → R1 = 5

Instruction 2 : LOAD 3 dans registre R2
    FETCH   → Lire "LOAD 3, R2"
    DECODE  → Comprendre : mettre 3 dans R2
    EXECUTE → R2 = 3

Instruction 3 : ADD R1, R2, R3
    FETCH   → Lire "ADD R1, R2, R3"
    DECODE  → Comprendre : additionner R1 et R2, mettre dans R3
    EXECUTE → R3 = R1 + R2 = 5 + 3 = 8
```

## 3.3 La Fréquence d'Horloge (Clock Speed)

```
┌─────────────────────────────────────────────────────────┐
│              FRÉQUENCE D'HORLOGE                        │
│                                                         │
│  Le CPU a une "horloge" interne qui bat régulièrement  │
│  Chaque "battement" = 1 cycle                          │
│                                                         │
│  Fréquence = nombre de cycles par seconde              │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │  1 MHz  = 1 million de cycles/seconde           │   │
│  │  1 GHz  = 1 milliard de cycles/seconde          │   │
│  │  3 GHz  = 3 milliards de cycles/seconde         │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  Un CPU à 3 GHz fait 3 milliards d'opérations/seconde! │
│                                                         │
│  ATTENTION : Plus de GHz ≠ forcément plus rapide       │
│  (Cela dépend aussi de l'architecture)                 │
└─────────────────────────────────────────────────────────┘
```

## 3.4 Le Pipeline - Travail à la Chaîne

Pour aller plus vite, les CPU modernes utilisent le **pipeline** :

```
SANS PIPELINE (une instruction à la fois) :
─────────────────────────────────────────────────────────
Temps    │ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ 7 │ 8 │ 9 │
─────────────────────────────────────────────────────────
Instr 1  │ F │ D │ E │   │   │   │   │   │   │
Instr 2  │   │   │   │ F │ D │ E │   │   │   │
Instr 3  │   │   │   │   │   │   │ F │ D │ E │
─────────────────────────────────────────────────────────
→ 3 instructions en 9 cycles


AVEC PIPELINE (comme une chaîne de montage) :
─────────────────────────────────────────────────────────
Temps    │ 1 │ 2 │ 3 │ 4 │ 5 │
─────────────────────────────────────────────────────────
Instr 1  │ F │ D │ E │   │   │
Instr 2  │   │ F │ D │ E │   │
Instr 3  │   │   │ F │ D │ E │
─────────────────────────────────────────────────────────
→ 3 instructions en 5 cycles (presque 2x plus rapide!)

Légende : F = Fetch, D = Decode, E = Execute
```

**Analogie** : C'est comme une chaîne de montage de voitures. Pendant qu'une voiture est peinte, une autre est assemblée, et une autre reçoit son moteur. On ne fait pas tout sur une seule voiture avant de commencer la suivante.

---

# Partie 4 : La Mémoire

## 4.1 La Hiérarchie de Mémoire

Plus la mémoire est proche du CPU, plus elle est rapide mais petite et chère :

```
                        HIÉRARCHIE DE MÉMOIRE
                        
                              ▲
                              │ Plus rapide
                              │ Plus petit
                              │ Plus cher
                              │
              ┌───────────────┴───────────────┐
              │         REGISTRES             │  ← Dans le CPU
              │   ~1 ns    ~1 Ko    ~$$$$$    │     Quelques octets
              └───────────────┬───────────────┘
                              │
              ┌───────────────┴───────────────┐
              │         CACHE L1              │  ← Dans le CPU
              │   ~2 ns    ~64 Ko   ~$$$$     │     Par cœur
              └───────────────┬───────────────┘
                              │
              ┌───────────────┴───────────────┐
              │         CACHE L2              │  ← Dans le CPU
              │   ~7 ns    ~256 Ko  ~$$$      │     Par cœur
              └───────────────┬───────────────┘
                              │
              ┌───────────────┴───────────────┐
              │         CACHE L3              │  ← Dans le CPU
              │   ~15 ns   ~8-32 Mo ~$$       │     Partagé
              └───────────────┬───────────────┘
                              │
              ┌───────────────┴───────────────┐
              │            RAM                │  ← Sur la carte mère
              │   ~100 ns  ~8-64 Go ~$        │     
              └───────────────┬───────────────┘
                              │
              ┌───────────────┴───────────────┐
              │        SSD / Disque           │  ← Stockage
              │   ~100 µs  ~To     ~¢         │     
              └───────────────────────────────┘
              
                              │
                              │ Plus lent
                              │ Plus grand
                              │ Moins cher
                              ▼
```

### Temps d'Accès en Perspective

```
┌─────────────────────────────────────────────────────────────┐
│     SI 1 CYCLE CPU = 1 SECONDE, ALORS...                   │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Registre     →  1 seconde                                  │
│  Cache L1     →  2 secondes                                 │
│  Cache L2     →  7 secondes                                 │
│  Cache L3     →  15 secondes                                │
│  RAM          →  1.5 minute                                 │
│  SSD          →  2-3 jours                                  │
│  Disque dur   →  1-6 mois                                   │
│  Internet     →  Plusieurs années                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

C'est pourquoi le **cache** est si important !

## 4.2 Le Cache - Mémoire Tampon

Le cache garde une copie des données fréquemment utilisées près du CPU :

```
┌─────────────────────────────────────────────────────────────┐
│                    COMMENT FONCTIONNE LE CACHE              │
│                                                             │
│   1. Le CPU veut la donnée X                                │
│                                                             │
│   2. Est-ce que X est dans le cache ?                       │
│      │                                                      │
│      ├── OUI (Cache Hit) ✓                                  │
│      │   → Récupérer X du cache (très rapide)               │
│      │                                                      │
│      └── NON (Cache Miss) ✗                                 │
│          → Aller chercher X dans la RAM (lent)              │
│          → Copier X dans le cache pour la prochaine fois    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Cache Hit vs Cache Miss

```
CACHE HIT (Succès) - Ce qu'on veut !
────────────────────────────────────
CPU : "Je veux la donnée à l'adresse 0x1234"
Cache L1 : "Je l'ai ! La voici !" (2 ns)
→ Très rapide ✓


CACHE MISS (Échec) - Ce qu'on veut éviter
────────────────────────────────────
CPU : "Je veux la donnée à l'adresse 0x5678"
Cache L1 : "Je ne l'ai pas..."
Cache L2 : "Moi non plus..."
Cache L3 : "Moi non plus..."
RAM : "Je l'ai, la voilà" (100 ns)
→ 50x plus lent ✗
```

### Localité des Données

Les caches fonctionnent bien grâce à deux principes :

```
┌─────────────────────────────────────────────────────────────┐
│               LOCALITÉ TEMPORELLE                           │
│                                                             │
│  "Si j'utilise une donnée maintenant, je vais probablement │
│   la réutiliser bientôt"                                   │
│                                                             │
│  Exemple : Une variable de boucle                           │
│                                                             │
│  for i in 0..1000 {                                         │
│      total += i;  // 'i' et 'total' réutilisés 1000 fois   │
│  }                                                          │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│               LOCALITÉ SPATIALE                             │
│                                                             │
│  "Si j'utilise une donnée, je vais probablement utiliser   │
│   les données à côté"                                       │
│                                                             │
│  Exemple : Parcourir un tableau                             │
│                                                             │
│  Mémoire : [1][2][3][4][5][6][7][8]                        │
│             ↑                                               │
│  Si j'accède à [1], le cache charge aussi [2][3][4]...     │
│  → Les accès suivants seront des cache hits !               │
└─────────────────────────────────────────────────────────────┘
```

## 4.3 RAM - La Mémoire Vive

```
┌─────────────────────────────────────────────────────────────┐
│                         RAM                                 │
│                                                             │
│  Caractéristiques :                                         │
│  • Volatile : s'efface quand on éteint                      │
│  • Accès aléatoire : on peut lire n'importe où              │
│  • Rapide (comparé au disque)                               │
│  • Capacité : 8 Go à 128 Go typiquement                     │
│                                                             │
│  Contient :                                                 │
│  • Le code des programmes en cours                          │
│  • Les données des programmes en cours                      │
│  • Le système d'exploitation                                │
│                                                             │
│  Organisation :                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Adresse 0x0000  │ Données du système               │   │
│  │ Adresse 0x1000  │ Programme 1 - Code               │   │
│  │ Adresse 0x2000  │ Programme 1 - Données            │   │
│  │ Adresse 0x3000  │ Programme 2 - Code               │   │
│  │ ...             │ ...                              │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 4.4 Types de Mémoire dans un Programme

```
┌─────────────────────────────────────────────────────────────┐
│           ORGANISATION MÉMOIRE D'UN PROGRAMME               │
│                                                             │
│   Adresses hautes                                           │
│        │                                                    │
│        ▼                                                    │
│   ┌─────────────┐                                          │
│   │    STACK    │  ← Variables locales, appels fonction    │
│   │    (Pile)   │    Grandit vers le bas ↓                 │
│   │      ↓      │                                          │
│   ├─────────────┤                                          │
│   │             │  ← Espace libre                          │
│   │             │                                          │
│   ├─────────────┤                                          │
│   │      ↑      │                                          │
│   │    HEAP     │  ← Allocation dynamique (malloc, new)    │
│   │    (Tas)    │    Grandit vers le haut ↑                │
│   ├─────────────┤                                          │
│   │    BSS      │  ← Variables globales non initialisées   │
│   ├─────────────┤                                          │
│   │    DATA     │  ← Variables globales initialisées       │
│   ├─────────────┤                                          │
│   │    TEXT     │  ← Code du programme (instructions)      │
│   └─────────────┘                                          │
│        │                                                    │
│        ▼                                                    │
│   Adresses basses                                           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Stack (Pile) vs Heap (Tas)

```
┌─────────────────────────┬─────────────────────────┐
│         STACK           │          HEAP           │
├─────────────────────────┼─────────────────────────┤
│ Allocation automatique  │ Allocation manuelle     │
│ Très rapide             │ Plus lent               │
│ Taille limitée (~1 Mo)  │ Taille quasi-illimitée  │
│ LIFO (dernier entré,    │ Accès dans n'importe    │
│       premier sorti)    │ quel ordre              │
│ Variables locales       │ Données dynamiques      │
│ Paramètres fonctions    │ Objets, tableaux grands │
├─────────────────────────┼─────────────────────────┤
│ En Rust :               │ En Rust :               │
│ let x = 5;              │ let v = vec![1,2,3];    │
│ let s = "hello";        │ let b = Box::new(5);    │
└─────────────────────────┴─────────────────────────┘
```

---

# Partie 5 : Du Mono-cœur au Multi-cœur

## 5.1 L'Évolution des Processeurs

```
HISTORIQUE SIMPLIFIÉ
────────────────────────────────────────────────────────────────

1970s-1990s : Augmentation de la fréquence
┌────────────────────────────────────────────────────────────┐
│  1971 : Intel 4004     →  740 KHz                          │
│  1989 : Intel 486      →  25 MHz                           │
│  1999 : Pentium III    →  500 MHz                          │
│  2004 : Pentium 4      →  3.8 GHz                          │
└────────────────────────────────────────────────────────────┘

2004+ : Le "Mur de la Chaleur" (Power Wall)
┌────────────────────────────────────────────────────────────┐
│  PROBLÈME : Plus de GHz = Plus de chaleur = Surchauffe !   │
│                                                            │
│  On ne pouvait plus augmenter la fréquence indéfiniment    │
│                                                            │
│  SOLUTION : Ajouter plusieurs cœurs au lieu d'accélérer    │
└────────────────────────────────────────────────────────────┘

2005+ : Ère multi-cœur
┌────────────────────────────────────────────────────────────┐
│  2005 : Dual-core (2 cœurs)                                │
│  2006 : Quad-core (4 cœurs)                                │
│  2010 : 6-8 cœurs grand public                             │
│  2020 : 16-64 cœurs grand public                           │
│  Serveurs : jusqu'à 128+ cœurs                             │
└────────────────────────────────────────────────────────────┘
```

## 5.2 Qu'est-ce qu'un Cœur ?

```
┌─────────────────────────────────────────────────────────────┐
│                      QU'EST-CE QU'UN CŒUR ?                │
│                                                             │
│  Un cœur = Une unité de calcul COMPLÈTE et INDÉPENDANTE    │
│                                                             │
│  Chaque cœur possède :                                      │
│  • Sa propre unité de contrôle                              │
│  • Sa propre ALU                                            │
│  • Sa propre FPU                                            │
│  • Ses propres registres                                    │
│  • Son propre cache L1 et L2                                │
│                                                             │
│  Les cœurs PARTAGENT :                                      │
│  • Le cache L3                                              │
│  • L'accès à la RAM                                         │
│  • Les bus de communication                                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### CPU Multi-cœur - Structure

```
┌─────────────────────────────────────────────────────────────────┐
│                    CPU QUAD-CORE (4 cœurs)                      │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│  │   CŒUR 0    │  │   CŒUR 1    │  │   CŒUR 2    │  │   CŒUR 3    │
│  │  ┌───────┐  │  │  ┌───────┐  │  │  ┌───────┐  │  │  ┌───────┐  │
│  │  │Registr│  │  │  │Registr│  │  │  │Registr│  │  │  │Registr│  │
│  │  │  ALU  │  │  │  │  ALU  │  │  │  │  ALU  │  │  │  │  ALU  │  │
│  │  │  FPU  │  │  │  │  FPU  │  │  │  │  FPU  │  │  │  │  FPU  │  │
│  │  └───────┘  │  │  └───────┘  │  │  └───────┘  │  │  └───────┘  │
│  │  Cache L1   │  │  Cache L1   │  │  Cache L1   │  │  Cache L1   │
│  │  Cache L2   │  │  Cache L2   │  │  Cache L2   │  │  Cache L2   │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────┘
│         │                │                │                │
│         └────────────────┴────────────────┴────────────────┘
│                                 │
│                    ┌────────────┴────────────┐
│                    │       CACHE L3          │  ← Partagé
│                    │    (Mémoire commune)    │
│                    └────────────┬────────────┘
│                                 │
│                    ┌────────────┴────────────┐
│                    │   CONTRÔLEUR MÉMOIRE    │  ← Vers la RAM
│                    └─────────────────────────┘
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## 5.3 Hyper-Threading / SMT

```
┌─────────────────────────────────────────────────────────────┐
│           HYPER-THREADING (Intel) / SMT (AMD)               │
│         (Simultaneous Multi-Threading)                      │
│                                                             │
│  IDÉE : Un cœur physique peut exécuter 2 threads           │
│         en simulant 2 cœurs "logiques"                     │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              1 CŒUR PHYSIQUE                        │   │
│  │  ┌───────────────────┬───────────────────┐         │   │
│  │  │  Thread logique 1 │  Thread logique 2 │         │   │
│  │  │  (Cœur logique 0) │  (Cœur logique 1) │         │   │
│  │  └───────────────────┴───────────────────┘         │   │
│  │              ↓               ↓                      │   │
│  │         Partagent les ressources du cœur            │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  EXEMPLE : CPU 4 cœurs avec Hyper-Threading                 │
│  → 4 cœurs physiques                                        │
│  → 8 cœurs logiques (ce que voit l'OS)                      │
│                                                             │
│  GAIN : ~15-30% de performance en plus (pas 100%)           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 5.4 Comparaison Mono-cœur vs Multi-cœur

```
TÂCHE : Préparer 4 plats

MONO-CŒUR (1 cuisinier)
────────────────────────────────────────────────
        Temps →
Cuisinier : [Plat1][Plat2][Plat3][Plat4]
                                          ↑
                                    Fini ici
Temps total : 4 unités


MULTI-CŒUR (4 cuisiniers)
────────────────────────────────────────────────
          Temps →
Cuisinier 1 : [Plat1]
Cuisinier 2 : [Plat2]
Cuisinier 3 : [Plat3]
Cuisinier 4 : [Plat4]
                   ↑
             Fini ici
Temps total : 1 unité (4x plus rapide !)


MAIS ATTENTION : Tout n'est pas parallélisable !
────────────────────────────────────────────────
Si Plat2 nécessite que Plat1 soit fini...
On ne peut pas le faire en parallèle !
```

## 5.5 La Loi d'Amdahl

```
┌─────────────────────────────────────────────────────────────┐
│                    LOI D'AMDAHL                             │
│                                                             │
│  "L'accélération maximale d'un programme est limitée       │
│   par sa partie qui ne peut PAS être parallélisée"         │
│                                                             │
│  FORMULE :                                                  │
│                        1                                    │
│  Accélération = ─────────────────                          │
│                  (1-P) + (P/N)                              │
│                                                             │
│  P = partie parallélisable (entre 0 et 1)                   │
│  N = nombre de processeurs                                  │
│                                                             │
│  EXEMPLE :                                                  │
│  Programme : 80% parallélisable, 20% séquentiel             │
│                                                             │
│  Avec 2 cœurs  : 1/(0.2 + 0.8/2)  = 1.67x plus rapide      │
│  Avec 4 cœurs  : 1/(0.2 + 0.8/4)  = 2.50x plus rapide      │
│  Avec 8 cœurs  : 1/(0.2 + 0.8/8)  = 3.33x plus rapide      │
│  Avec ∞ cœurs  : 1/(0.2 + 0)      = 5.00x MAXIMUM !        │
│                                                             │
│  → Même avec des millions de cœurs, on ne dépasse pas 5x   │
│                                                             │
└─────────────────────────────────────────────────────────────┘

VISUALISATION :
────────────────────────────────────────────────
Programme original :
[███████████████████│████████]
   80% parallèle     20% séquentiel

Avec 4 cœurs :
[█████│████████]
  20%    20%
   ↑       ↑
 4x plus  Pas changé
 rapide

→ Le séquentiel devient le goulot d'étranglement !
```

---

# Partie 6 : Processus et Threads

## 6.1 Qu'est-ce qu'un Processus ?

```
┌─────────────────────────────────────────────────────────────┐
│                      PROCESSUS                              │
│                                                             │
│  Définition : Un programme EN COURS D'EXÉCUTION             │
│                                                             │
│  Un processus possède :                                     │
│  • Son propre espace mémoire (isolé)                        │
│  • Son propre code à exécuter                               │
│  • Ses propres ressources (fichiers ouverts, etc.)          │
│  • Au moins un thread (le thread principal)                 │
│  • Un identifiant unique (PID)                              │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                 PROCESSUS 1234                       │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐          │   │
│  │  │   CODE   │  │  DONNÉES │  │   HEAP   │          │   │
│  │  │          │  │          │  │          │          │   │
│  │  └──────────┘  └──────────┘  └──────────┘          │   │
│  │                                                     │   │
│  │  ┌──────────────────────────────────────────────┐  │   │
│  │  │  THREAD PRINCIPAL (Stack, Registres)         │  │   │
│  │  └──────────────────────────────────────────────┘  │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  Exemples de processus :                                    │
│  • Chrome, Firefox (navigateurs)                            │
│  • Word, Excel (bureautique)                                │
│  • Spotify (musique)                                        │
│  • Votre programme Rust                                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 6.2 Qu'est-ce qu'un Thread ?

```
┌─────────────────────────────────────────────────────────────┐
│                       THREAD                                │
│                                                             │
│  Définition : Un "fil d'exécution" au sein d'un processus   │
│               = La plus petite unité d'exécution            │
│                                                             │
│  Un thread possède :                                        │
│  • Sa propre stack (pile)                                   │
│  • Ses propres registres                                    │
│  • Son propre compteur de programme (où il en est)          │
│                                                             │
│  Un thread PARTAGE avec les autres threads du processus :   │
│  • L'espace mémoire (heap, données globales)                │
│  • Le code du programme                                     │
│  • Les ressources (fichiers, etc.)                          │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                  PROCESSUS                           │   │
│  │                                                      │   │
│  │  Code, Données, Heap  ← PARTAGÉS                    │   │
│  │                                                      │   │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐            │   │
│  │  │ Thread 1 │ │ Thread 2 │ │ Thread 3 │            │   │
│  │  │ ──────── │ │ ──────── │ │ ──────── │            │   │
│  │  │ Stack    │ │ Stack    │ │ Stack    │            │   │
│  │  │ Registr. │ │ Registr. │ │ Registr. │            │   │
│  │  │ PC       │ │ PC       │ │ PC       │            │   │
│  │  └──────────┘ └──────────┘ └──────────┘            │   │
│  │       ↑             ↑             ↑                 │   │
│  │       └─────────────┴─────────────┘                 │   │
│  │         Peuvent tourner en PARALLÈLE                │   │
│  │         sur différents cœurs CPU                    │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 6.3 Processus vs Thread - Comparaison

```
┌────────────────────────────┬────────────────────────────────┐
│         PROCESSUS          │            THREAD              │
├────────────────────────────┼────────────────────────────────┤
│ Mémoire ISOLÉE             │ Mémoire PARTAGÉE               │
│ (chacun son espace)        │ (même espace mémoire)          │
├────────────────────────────┼────────────────────────────────┤
│ Communication difficile    │ Communication facile           │
│ (IPC : pipes, sockets...)  │ (variables partagées)          │
├────────────────────────────┼────────────────────────────────┤
│ Création LENTE             │ Création RAPIDE                │
│ (copier tout l'espace)     │ (juste stack + registres)      │
├────────────────────────────┼────────────────────────────────┤
│ Plus SÉCURISÉ              │ Moins sécurisé                 │
│ (isolation mémoire)        │ (un bug peut tout crasher)     │
├────────────────────────────┼────────────────────────────────┤
│ Un crash n'affecte pas     │ Un crash peut affecter         │
│ les autres processus       │ tous les threads               │
├────────────────────────────┼────────────────────────────────┤
│ Exemple : Chrome           │ Exemple : Les onglets dans     │
│ (1 processus par onglet)   │ Firefox (threads)              │
└────────────────────────────┴────────────────────────────────┘
```

### Analogie

```
PROCESSUS = Appartements différents dans un immeuble
────────────────────────────────────────────────────
• Chacun a sa propre cuisine, salon, salle de bain
• Impossible d'accéder à l'appartement du voisin
• Pour communiquer : passer par le couloir (IPC)


THREADS = Colocataires dans le même appartement
────────────────────────────────────────────────────
• Partagent la cuisine, le salon, la salle de bain
• Chacun a sa propre chambre (stack)
• Communication facile (se parler directement)
• MAIS : Doivent se coordonner (qui utilise la douche ?)
```

## 6.4 Le Scheduler (Ordonnanceur)

Le **scheduler** est le composant de l'OS qui décide quel thread s'exécute et quand.

```
┌─────────────────────────────────────────────────────────────┐
│                      SCHEDULER                              │
│                                                             │
│  RÔLE : Répartir le temps CPU entre tous les threads        │
│                                                             │
│  COMMENT ÇA MARCHE :                                        │
│                                                             │
│  1. Le scheduler a une liste de threads prêts               │
│  2. Il assigne chaque thread à un cœur disponible           │
│  3. Chaque thread reçoit un "quantum" de temps              │
│  4. Quand le quantum expire → changement de thread          │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │    Temps →                                          │   │
│  │                                                     │   │
│  │  Cœur 0 : [T1][T3][T1][T5][T1][T3]...              │   │
│  │  Cœur 1 : [T2][T4][T2][T6][T4][T2]...              │   │
│  │                                                     │   │
│  │  Chaque [ ] = un quantum de temps (~1-10 ms)        │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  CONTEXT SWITCH (Changement de Contexte) :                  │
│  • Sauvegarder l'état du thread actuel (registres, etc.)    │
│  • Charger l'état du nouveau thread                         │
│  • Coût : ~1-10 microsecondes (pas gratuit !)               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### États d'un Thread

```
                    ┌─────────────────────────────────────┐
                    │                                     │
                    ▼                                     │
              ┌──────────┐                               │
    Création  │   NEW    │                               │
       │      │ (Nouveau)│                               │
       │      └────┬─────┘                               │
       │           │ start()                             │
       │           ▼                                     │
       │      ┌──────────┐         ┌──────────┐         │
       │      │  READY   │ ◄────── │ RUNNING  │         │
       └─────►│  (Prêt)  │ ──────► │(En cours)│         │
              └────┬─────┘ sélect. └────┬─────┘         │
                   │      par           │               │
                   │      scheduler     │               │
                   │                    │               │
                   │   ┌────────────────┼───────────────┤
                   │   │                │               │
                   │   │ Attente I/O    │ Terminé       │
                   │   │ sleep()        │               │
                   │   │ wait()         │               │
                   │   ▼                ▼               │
              ┌──────────┐        ┌──────────┐         │
              │ BLOCKED  │        │TERMINATED│         │
              │ (Bloqué) │        │(Terminé) │         │
              └────┬─────┘        └──────────┘         │
                   │                                    │
                   │ I/O terminé                        │
                   └────────────────────────────────────┘
```

---

# Partie 7 : Concurrence vs Parallélisme

## 7.1 Définitions Claires

```
┌─────────────────────────────────────────────────────────────┐
│                     CONCURRENCE                             │
│                                                             │
│  Définition : Gérer plusieurs tâches qui PROGRESSENT        │
│               pendant la même période de temps              │
│                                                             │
│  ≠ Signifie pas "en même temps" !                           │
│  = Signifie "dans la même fenêtre temporelle"               │
│                                                             │
│  EXEMPLE : Jongleur avec 3 balles                           │
│  • Une seule balle en main à la fois                        │
│  • MAIS il gère les 3 balles "en même temps"                │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  1 CPU, 2 tâches, entrelacées :                     │   │
│  │                                                     │   │
│  │  [Tâche A][Tâche B][Tâche A][Tâche B][Tâche A]     │   │
│  │                                                     │   │
│  │  Les deux progressent, mais jamais simultanément    │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                     PARALLÉLISME                            │
│                                                             │
│  Définition : Exécuter plusieurs tâches VRAIMENT            │
│               en même temps (simultanément)                 │
│                                                             │
│  Nécessite : Plusieurs unités d'exécution (cœurs)           │
│                                                             │
│  EXEMPLE : Deux jongleurs avec 3 balles chacun              │
│  • Chacun fait son travail au même moment                   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  2 CPUs, 2 tâches, en parallèle :                   │   │
│  │                                                     │   │
│  │  CPU 1 : [Tâche A][Tâche A][Tâche A][Tâche A]      │   │
│  │  CPU 2 : [Tâche B][Tâche B][Tâche B][Tâche B]      │   │
│  │                                                     │   │
│  │  Les deux s'exécutent vraiment en même temps        │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 7.2 Visualisation Comparée

```
CONCURRENCE (1 serveur, plusieurs clients)
══════════════════════════════════════════

Temps →    1   2   3   4   5   6   7   8   9   10
           ─────────────────────────────────────────
Serveur:   │C1 │C2 │C1 │C3 │C2 │C1 │C3 │C2 │C3 │...│
           ─────────────────────────────────────────

Le serveur alterne entre les clients.
Chaque client a l'IMPRESSION d'être servi continuellement.


PARALLÉLISME (3 serveurs, plusieurs clients)
══════════════════════════════════════════

Temps →    1   2   3   4   5   6   7   8   9   10
           ─────────────────────────────────────────
Serveur 1: │C1 │C1 │C1 │C4 │C4 │C4 │C7 │...│
Serveur 2: │C2 │C2 │C2 │C5 │C5 │C5 │C8 │...│
Serveur 3: │C3 │C3 │C3 │C6 │C6 │C6 │C9 │...│
           ─────────────────────────────────────────

3 clients sont servis VRAIMENT en même temps.
```

## 7.3 Combinaison : Concurrence + Parallélisme

En pratique, on combine souvent les deux :

```
┌─────────────────────────────────────────────────────────────┐
│           CONCURRENCE + PARALLÉLISME                        │
│                                                             │
│  Situation réelle : 4 cœurs, 100 threads                    │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │  Cœur 0 : [T1][T5][T9 ][T1][T13][T5 ][T9 ]...     │    │
│  │  Cœur 1 : [T2][T6][T10][T2][T14][T6 ][T10]...     │    │
│  │  Cœur 2 : [T3][T7][T11][T3][T15][T7 ][T11]...     │    │
│  │  Cœur 3 : [T4][T8][T12][T4][T16][T8 ][T12]...     │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  • PARALLÉLISME : 4 threads s'exécutent en même temps       │
│  • CONCURRENCE  : 100 threads progressent en alternant      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 7.4 Quand Utiliser Quoi ?

```
┌────────────────────────────────────────────────────────────┐
│            CONCURRENCE (sans parallélisme)                 │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  UTILE QUAND :                                             │
│  • Beaucoup d'attente I/O (réseau, disque)                 │
│  • Serveur web (attendre les clients)                      │
│  • Interface utilisateur (attendre les clics)              │
│                                                            │
│  POURQUOI :                                                │
│  Pendant qu'une tâche attend, une autre peut travailler    │
│                                                            │
│  EXEMPLE : async/await en Rust                             │
│                                                            │
└────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────┐
│                      PARALLÉLISME                          │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  UTILE QUAND :                                             │
│  • Calculs intensifs (CPU-bound)                           │
│  • Traitement de données massives                          │
│  • Rendu graphique                                         │
│  • Simulations scientifiques                               │
│                                                            │
│  POURQUOI :                                                │
│  Plusieurs calculs en même temps = plus rapide             │
│                                                            │
│  EXEMPLE : rayon en Rust (parallélisme de données)         │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### Résumé en Une Phrase

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│  CONCURRENCE = Structure du programme                       │
│                (comment on ORGANISE les tâches)             │
│                                                             │
│  PARALLÉLISME = Exécution du programme                      │
│                 (comment on EXÉCUTE les tâches)             │
│                                                             │
│  "La concurrence concerne la STRUCTURE,                     │
│   le parallélisme concerne l'EXÉCUTION"                     │
│                    — Rob Pike (créateur de Go)              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

# Partie 8 : Les Problèmes de la Concurrence

## 8.1 Vue d'Ensemble des Problèmes

```
┌─────────────────────────────────────────────────────────────┐
│         PROBLÈMES CLASSIQUES DE LA CONCURRENCE              │
│                                                             │
│  1. RACE CONDITION (Condition de Course)                    │
│     → Résultat dépend de l'ordre d'exécution               │
│                                                             │
│  2. DATA RACE (Course aux Données)                          │
│     → Accès concurrent non synchronisé à la mémoire         │
│                                                             │
│  3. DEADLOCK (Interblocage)                                 │
│     → Threads bloqués mutuellement pour toujours            │
│                                                             │
│  4. LIVELOCK (Blocage Actif)                                │
│     → Threads qui travaillent mais ne progressent pas       │
│                                                             │
│  5. STARVATION (Famine)                                     │
│     → Un thread n'obtient jamais les ressources             │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 8.2 Race Condition (Condition de Course)

```
┌─────────────────────────────────────────────────────────────┐
│                    RACE CONDITION                           │
│                                                             │
│  DÉFINITION : Le résultat du programme dépend de l'ordre    │
│               (aléatoire) dans lequel les threads           │
│               s'exécutent                                   │
│                                                             │
│  EXEMPLE : Deux personnes retirent de l'argent              │
│                                                             │
│  Compte bancaire : 100€                                     │
│  Personne A : retirer 70€                                   │
│  Personne B : retirer 50€                                   │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │  SCÉNARIO PROBLÉMATIQUE :                          │    │
│  │                                                    │    │
│  │  1. A lit le solde : 100€                          │    │
│  │  2. B lit le solde : 100€                          │    │
│  │  3. A vérifie : 100 >= 70 ? OUI                    │    │
│  │  4. B vérifie : 100 >= 50 ? OUI                    │    │
│  │  5. A retire : 100 - 70 = 30€                      │    │
│  │  6. B retire : 100 - 50 = 50€                      │    │
│  │                                                    │    │
│  │  RÉSULTAT : Le solde est soit 30€ soit 50€         │    │
│  │             selon qui écrit en dernier !           │    │
│  │             Et on a retiré 120€ d'un compte à 100€!│    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 8.3 Data Race (Course aux Données)

```
┌─────────────────────────────────────────────────────────────┐
│                      DATA RACE                              │
│                                                             │
│  DÉFINITION : Deux threads accèdent à la même mémoire       │
│               ET au moins un fait une écriture              │
│               ET pas de synchronisation                     │
│                                                             │
│  C'est un BUG. Comportement INDÉFINI !                      │
│                                                             │
│  EXEMPLE : Compteur partagé                                 │
│                                                             │
│  compteur = 0                                               │
│  Thread 1 : compteur = compteur + 1                         │
│  Thread 2 : compteur = compteur + 1                         │
│  Résultat attendu : 2                                       │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │  CE QUI SE PASSE EN RÉALITÉ :                      │    │
│  │                                                    │    │
│  │  "compteur = compteur + 1" n'est PAS atomique !    │    │
│  │  C'est en fait 3 opérations :                      │    │
│  │                                                    │    │
│  │  1. LIRE compteur                                  │    │
│  │  2. AJOUTER 1                                      │    │
│  │  3. ÉCRIRE compteur                                │    │
│  │                                                    │    │
│  │  Thread 1          Thread 2                        │    │
│  │  ─────────         ─────────                       │    │
│  │  1. Lire (0)                                       │    │
│  │                    1. Lire (0)                     │    │
│  │  2. Ajouter (1)                                    │    │
│  │                    2. Ajouter (1)                  │    │
│  │  3. Écrire (1)                                     │    │
│  │                    3. Écrire (1)                   │    │
│  │                                                    │    │
│  │  RÉSULTAT : compteur = 1 (au lieu de 2 !)          │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  NOTE : Rust EMPÊCHE les data races à la compilation !      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 8.4 Deadlock (Interblocage)

```
┌─────────────────────────────────────────────────────────────┐
│                      DEADLOCK                               │
│                                                             │
│  DÉFINITION : Deux threads (ou plus) s'attendent           │
│               mutuellement pour toujours                    │
│                                                             │
│  CONDITIONS (toutes les 4 nécessaires) :                    │
│  1. Exclusion mutuelle (ressource non partageable)          │
│  2. Possession et attente (tenir + attendre)                │
│  3. Pas de préemption (on ne peut pas forcer à lâcher)      │
│  4. Attente circulaire (A attend B qui attend A)            │
│                                                             │
│  EXEMPLE : Le dîner des philosophes                         │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │         🍴 Fourchette 1 🍴                         │    │
│  │              │                                     │    │
│  │     👤 ──────┼────── 👤                            │    │
│  │   Phil. 1    │    Phil. 2                          │    │
│  │       │      │      │                              │    │
│  │       │      │      │                              │    │
│  │  🍴───┘      │      └───🍴                         │    │
│  │  Fourch.5    │      Fourch.2                       │    │
│  │              │                                     │    │
│  │                                                    │    │
│  │  Chaque philosophe a besoin de 2 fourchettes       │    │
│  │  Si chacun prend sa fourchette gauche et attend    │    │
│  │  la droite... DEADLOCK !                           │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  EXEMPLE CODE :                                             │
│                                                             │
│  Thread 1:                    Thread 2:                     │
│  ─────────                    ─────────                     │
│  lock(A)                      lock(B)                       │
│  lock(B) ← attend B           lock(A) ← attend A            │
│           qui ne sera                  qui ne sera          │
│           jamais libéré                jamais libéré        │
│                                                             │
│  → BLOQUÉ POUR TOUJOURS                                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Visualisation du Deadlock

```
        SITUATION NORMALE           DEADLOCK
        ────────────────           ────────
        
        Thread 1                   Thread 1
           │                          │
           ▼                          ▼
        [Lock A] ─────►           [Lock A]
           │                          │
           ▼                          │ attend B
        [Lock B]                      │
           │                          ▼
           ▼                      × BLOQUÉ
        [Travail]                     
           │                      Thread 2
           ▼                          │
        [Unlock B]                    ▼
           │                      [Lock B]
           ▼                          │
        [Unlock A]                    │ attend A
                                      │
                                      ▼
                                  × BLOQUÉ
```

## 8.5 Livelock (Blocage Actif)

```
┌─────────────────────────────────────────────────────────────┐
│                      LIVELOCK                               │
│                                                             │
│  DÉFINITION : Les threads ne sont pas bloqués mais          │
│               ne font que se répondre sans progresser       │
│                                                             │
│  ANALOGIE : Deux personnes dans un couloir                  │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │   →  👤        👤  ←                               │    │
│  │      Alice    Bob                                  │    │
│  │                                                    │    │
│  │   Étape 1: Alice va à droite, Bob va à gauche     │    │
│  │                                                    │    │
│  │        👤  →  ←  👤                                │    │
│  │      Alice      Bob                                │    │
│  │                                                    │    │
│  │   Étape 2: Ils se bloquent, Alice va à gauche,    │    │
│  │            Bob va à droite                         │    │
│  │                                                    │    │
│  │   ←  👤        👤  →                               │    │
│  │      Alice    Bob                                  │    │
│  │                                                    │    │
│  │   Et ça recommence... indéfiniment !               │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  DIFFÉRENCE AVEC DEADLOCK :                                 │
│  • Deadlock : threads bloqués, ne font RIEN                 │
│  • Livelock : threads actifs, mais ne PROGRESSENT pas       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 8.6 Starvation (Famine)

```
┌─────────────────────────────────────────────────────────────┐
│                     STARVATION                              │
│                                                             │
│  DÉFINITION : Un thread ne peut jamais accéder à une        │
│               ressource car d'autres passent toujours       │
│               avant lui                                     │
│                                                             │
│  ANALOGIE : File d'attente où les VIP passent toujours     │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │  File pour la ressource :                          │    │
│  │                                                    │    │
│  │  [VIP][VIP][VIP][Normal][VIP][VIP][Normal]         │    │
│  │                    ↑                 ↑              │    │
│  │                    │                 │              │    │
│  │              Attend depuis      Vient d'arriver    │    │
│  │              très longtemps                        │    │
│  │                                                    │    │
│  │  Si les VIP arrivent constamment, le "Normal"     │    │
│  │  ne sera JAMAIS servi !                            │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  CAUSES :                                                   │
│  • Priorités mal gérées                                     │
│  • Scheduler injuste                                        │
│  • Ressource monopolisée                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 8.7 Résumé des Problèmes

```
┌──────────────────┬─────────────────────────────────────────┐
│    PROBLÈME      │           CARACTÉRISTIQUE               │
├──────────────────┼─────────────────────────────────────────┤
│ Race Condition   │ Résultat imprévisible selon l'ordre     │
├──────────────────┼─────────────────────────────────────────┤
│ Data Race        │ Accès mémoire concurrent non protégé    │
├──────────────────┼─────────────────────────────────────────┤
│ Deadlock         │ Threads bloqués mutuellement            │
├──────────────────┼─────────────────────────────────────────┤
│ Livelock         │ Threads actifs mais sans progrès        │
├──────────────────┼─────────────────────────────────────────┤
│ Starvation       │ Thread jamais servi                     │
└──────────────────┴─────────────────────────────────────────┘

POINT IMPORTANT POUR RUST :
═══════════════════════════
Rust GARANTIT à la compilation l'absence de DATA RACES !
C'est son avantage majeur pour la programmation concurrente.
Les autres problèmes peuvent toujours arriver, mais les
data races (les plus courants et dangereux) sont éliminés.
```

---

# Partie 9 : Les Solutions de Synchronisation

## 9.1 Vue d'Ensemble

```
┌─────────────────────────────────────────────────────────────┐
│           OUTILS DE SYNCHRONISATION                         │
│                                                             │
│  1. MUTEX (Mutual Exclusion)                                │
│     → Un seul thread à la fois                              │
│                                                             │
│  2. SEMAPHORE                                               │
│     → N threads à la fois                                   │
│                                                             │
│  3. RWLOCK (Read-Write Lock)                                │
│     → Plusieurs lecteurs OU un écrivain                     │
│                                                             │
│  4. ATOMIC OPERATIONS                                       │
│     → Opérations indivisibles sans verrou                   │
│                                                             │
│  5. CONDITION VARIABLES                                     │
│     → Attendre qu'une condition soit vraie                  │
│                                                             │
│  6. CHANNELS (Message Passing)                              │
│     → Communication par messages                            │
│                                                             │
│  7. BARRIER                                                 │
│     → Point de synchronisation                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 9.2 Mutex (Mutual Exclusion)

```
┌─────────────────────────────────────────────────────────────┐
│                        MUTEX                                │
│                                                             │
│  PRINCIPE : Verrou qui garantit qu'UN SEUL thread          │
│             peut accéder à une ressource à la fois          │
│                                                             │
│  ANALOGIE : Cabine téléphonique avec verrou                │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │      Sans Mutex           Avec Mutex              │    │
│  │      ───────────          ──────────              │    │
│  │                                                    │    │
│  │      [Donnée]             [🔒 Donnée]             │    │
│  │       ↑   ↑                  ↑                    │    │
│  │       │   │                  │ (un seul)          │    │
│  │      T1  T2                 T1                    │    │
│  │      CHAOS !               T2 attend...           │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  OPÉRATIONS :                                               │
│  • lock()   : Acquérir le verrou (attend si occupé)         │
│  • unlock() : Libérer le verrou                             │
│  • try_lock(): Essayer sans attendre                        │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │  Thread 1              Thread 2                    │    │
│  │  ─────────             ─────────                   │    │
│  │  lock()                                            │    │
│  │  [accès données]       lock() ← BLOQUÉ            │    │
│  │  [modification]              ← attend...          │    │
│  │  unlock()                    ← débloqué !         │    │
│  │                        [accès données]            │    │
│  │                        unlock()                   │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  EN RUST :                                                  │
│  let mutex = Mutex::new(donnee);                            │
│  let guard = mutex.lock().unwrap();  // Verrouille          │
│  // utiliser guard                                          │
│  // Déverrouillé automatiquement quand guard est détruit    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 9.3 Sémaphore

```
┌─────────────────────────────────────────────────────────────┐
│                      SEMAPHORE                              │
│                                                             │
│  PRINCIPE : Compteur qui limite le nombre de threads        │
│             pouvant accéder simultanément                   │
│                                                             │
│  ANALOGIE : Parking avec N places                           │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │  Sémaphore(3) = 3 threads max en même temps       │    │
│  │                                                    │    │
│  │  [T1][T2][T3]  ← À l'intérieur (3 places prises)  │    │
│  │   T4 T5 T6     ← En attente                       │    │
│  │                                                    │    │
│  │  Quand T1 sort → T4 peut entrer                   │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  OPÉRATIONS :                                               │
│  • acquire() / wait() / P() : Décrémenter (entrer)          │
│  • release() / signal() / V() : Incrémenter (sortir)        │
│                                                             │
│  CAS SPÉCIAUX :                                             │
│  • Sémaphore(1) = équivalent à un Mutex                     │
│  • Sémaphore(0) = pour la signalisation                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 9.4 RwLock (Read-Write Lock)

```
┌─────────────────────────────────────────────────────────────┐
│                      RWLOCK                                 │
│                                                             │
│  PRINCIPE : Distingue lecteurs et écrivains                 │
│             • Plusieurs lecteurs simultanés OK              │
│             • Un seul écrivain, exclusif                    │
│                                                             │
│  ANALOGIE : Bibliothèque                                    │
│             • Plusieurs personnes peuvent lire              │
│             • Pour modifier, il faut être seul              │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │  Mode LECTURE :                                    │    │
│  │  [📖 Lecteur 1]                                    │    │
│  │  [📖 Lecteur 2]  ← Tous en même temps OK          │    │
│  │  [📖 Lecteur 3]                                    │    │
│  │                                                    │    │
│  │  Mode ÉCRITURE :                                   │    │
│  │  [✏️ Écrivain]   ← SEUL, lecteurs attendent       │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  QUAND L'UTILISER :                                         │
│  • Beaucoup plus de lectures que d'écritures                │
│  • Exemple : cache, configuration                           │
│                                                             │
│  EN RUST :                                                  │
│  let lock = RwLock::new(donnee);                            │
│  let read_guard = lock.read().unwrap();   // Lecture        │
│  let write_guard = lock.write().unwrap(); // Écriture       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 9.5 Opérations Atomiques

```
┌─────────────────────────────────────────────────────────────┐
│                 OPÉRATIONS ATOMIQUES                        │
│                                                             │
│  PRINCIPE : Opérations indivisibles garanties par le        │
│             matériel, sans besoin de verrou                 │
│                                                             │
│  ATOMIQUE signifie : "Ne peut pas être interrompu"          │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │  OPÉRATION NORMALE (3 étapes, interruptible) :     │    │
│  │  1. Lire valeur                                    │    │
│  │  2. Modifier    ← Un autre thread peut s'insérer   │    │
│  │  3. Écrire                                         │    │
│  │                                                    │    │
│  │  OPÉRATION ATOMIQUE (1 étape, indivisible) :       │    │
│  │  [Lire + Modifier + Écrire]  ← Tout en un coup    │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  TYPES ATOMIQUES EN RUST :                                  │
│  • AtomicBool                                               │
│  • AtomicI32, AtomicU64, etc.                               │
│  • AtomicPtr<T>                                             │
│                                                             │
│  OPÉRATIONS :                                               │
│  • load() : Lire                                            │
│  • store() : Écrire                                         │
│  • fetch_add() : Ajouter et retourner l'ancienne valeur     │
│  • compare_exchange() : Comparer et échanger                │
│                                                             │
│  AVANTAGE : Plus rapide que Mutex pour opérations simples   │
│  LIMITE : Seulement pour types simples (pas de struct)      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 9.6 Channels (Canaux de Communication)

```
┌─────────────────────────────────────────────────────────────┐
│                      CHANNELS                               │
│                                                             │
│  PRINCIPE : Communication par MESSAGES au lieu de           │
│             mémoire partagée                                │
│                                                             │
│  PHILOSOPHIE : "Ne communiquez pas en partageant la         │
│                mémoire, partagez la mémoire en              │
│                communiquant"                                │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │  MÉMOIRE PARTAGÉE :        CHANNELS :              │    │
│  │                                                    │    │
│  │      [Donnée]              [Expéditeur]            │    │
│  │       ↑   ↑                     │                  │    │
│  │      T1  T2                     ▼                  │    │
│  │    (DANGEREUX)             [═══════════]           │    │
│  │                             (le canal)             │    │
│  │                                 │                  │    │
│  │                                 ▼                  │    │
│  │                            [Récepteur]             │    │
│  │                             (SÉCURISÉ)             │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  TYPES DE CHANNELS :                                        │
│                                                             │
│  • MPSC : Multiple Producers, Single Consumer               │
│    (Plusieurs expéditeurs, un récepteur)                    │
│                                                             │
│  • MPMC : Multiple Producers, Multiple Consumers            │
│    (Plusieurs expéditeurs, plusieurs récepteurs)            │
│                                                             │
│  • SPSC : Single Producer, Single Consumer                  │
│    (Un expéditeur, un récepteur)                            │
│                                                             │
│  EN RUST (standard) :                                       │
│  let (tx, rx) = mpsc::channel();                            │
│  tx.send(message).unwrap();  // Envoyer                     │
│  let msg = rx.recv().unwrap(); // Recevoir                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 9.7 Barrier (Barrière)

```
┌─────────────────────────────────────────────────────────────┐
│                       BARRIER                               │
│                                                             │
│  PRINCIPE : Point de rendez-vous où tous les threads        │
│             doivent arriver avant de continuer              │
│                                                             │
│  ANALOGIE : Groupe qui attend que tout le monde soit là     │
│             avant de partir                                 │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │  Thread 1: ═════════►│                             │    │
│  │  Thread 2: ══►       │  BARRIER                    │    │
│  │  Thread 3: ════════► │    (attend)                 │    │
│  │  Thread 4: ═══►      │                             │    │
│  │                      │                             │    │
│  │  Quand TOUS arrivent :                             │    │
│  │                      │                             │    │
│  │  Thread 1: ══════════│═══════════►                 │    │
│  │  Thread 2: ══════════│═══════════►                 │    │
│  │  Thread 3: ══════════│═══════════►                 │    │
│  │  Thread 4: ══════════│═══════════►                 │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  UTILISATION : Synchroniser des phases de calcul            │
│                                                             │
│  EN RUST :                                                  │
│  let barrier = Arc::new(Barrier::new(4)); // 4 threads      │
│  barrier.wait(); // Attend les autres                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 9.8 Condition Variable

```
┌─────────────────────────────────────────────────────────────┐
│                  CONDITION VARIABLE                         │
│                                                             │
│  PRINCIPE : Permet à un thread d'ATTENDRE qu'une            │
│             condition soit remplie                          │
│                                                             │
│  ANALOGIE : Attendre que le repas soit prêt                 │
│                                                             │
│  ┌────────────────────────────────────────────────────┐    │
│  │                                                    │    │
│  │  Thread PRODUCTEUR :                               │    │
│  │  1. Produire donnée                                │    │
│  │  2. notify() → "C'est prêt !"                      │    │
│  │                                                    │    │
│  │  Thread CONSOMMATEUR :                             │    │
│  │  1. wait() → Dort jusqu'à notification             │    │
│  │  2. Se réveille                                    │    │
│  │  3. Consommer donnée                               │    │
│  │                                                    │    │
│  └────────────────────────────────────────────────────┘    │
│                                                             │
│  OPÉRATIONS :                                               │
│  • wait()       : Attendre la notification                  │
│  • notify_one() : Réveiller UN thread en attente            │
│  • notify_all() : Réveiller TOUS les threads en attente     │
│                                                             │
│  ATTENTION : Toujours utilisé AVEC un Mutex                 │
│                                                             │
│  EN RUST :                                                  │
│  let pair = (Mutex::new(false), Condvar::new());            │
│  // Attendre                                                │
│  let (lock, cvar) = &pair;                                  │
│  let guard = cvar.wait(lock.lock().unwrap()).unwrap();      │
│  // Notifier                                                │
│  cvar.notify_one();                                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## 9.9 Tableau Récapitulatif

```
┌───────────────┬──────────────────────┬─────────────────────────┐
│    OUTIL      │      CAS D'USAGE     │      PERFORMANCE        │
├───────────────┼──────────────────────┼─────────────────────────┤
│ Mutex         │ Protection simple    │ Moyen                   │
│               │ 1 thread à la fois   │                         │
├───────────────┼──────────────────────┼─────────────────────────┤
│ RwLock        │ Beaucoup de lectures │ Bon pour lectures       │
│               │ peu d'écritures      │ Moyen pour écritures    │
├───────────────┼──────────────────────┼─────────────────────────┤
│ Atomic        │ Compteurs, flags     │ Excellent               │
│               │ Types simples        │ (pas de verrou)         │
├───────────────┼──────────────────────┼─────────────────────────┤
│ Channel       │ Communication        │ Bon                     │
│               │ Pipeline de données  │ (dépend du type)        │
├───────────────┼──────────────────────┼─────────────────────────┤
│ Semaphore     │ Limiter accès        │ Moyen                   │
│               │ Pool de ressources   │                         │
├───────────────┼──────────────────────┼─────────────────────────┤
│ Barrier       │ Synchronisation      │ Bon                     │
│               │ de phases            │                         │
├───────────────┼──────────────────────┼─────────────────────────┤
│ CondVar       │ Producteur/Consom.   │ Moyen                   │
│               │ Attente événement    │                         │
└───────────────┴──────────────────────┴─────────────────────────┘
```

---

# Partie 10 : Glossaire Complet

## A

```
┌─────────────────────────────────────────────────────────────┐
│ ADRESSE MÉMOIRE                                             │
│ Numéro unique identifiant un emplacement en mémoire.        │
│ Exemple : 0x7FFE1234                                        │
├─────────────────────────────────────────────────────────────┤
│ ALU (Arithmetic Logic Unit)                                 │
│ Partie du CPU qui fait les calculs sur les entiers          │
│ et les opérations logiques (ET, OU, NON).                   │
├─────────────────────────────────────────────────────────────┤
│ AMDAHL (Loi de)                                             │
│ Limite théorique de l'accélération possible avec            │
│ plus de processeurs, dépend de la partie séquentielle.      │
├─────────────────────────────────────────────────────────────┤
│ ARC (Atomic Reference Counting)                             │
│ Compteur de références thread-safe pour partager            │
│ des données entre threads en Rust.                          │
├─────────────────────────────────────────────────────────────┤
│ ASYNCHRONE                                                  │
│ Exécution qui ne bloque pas en attendant une réponse.       │
│ Permet de faire autre chose pendant l'attente.              │
├─────────────────────────────────────────────────────────────┤
│ ATOMIC (Atomique)                                           │
│ Opération indivisible, ne peut pas être interrompue.        │
│ S'exécute entièrement ou pas du tout.                       │
└─────────────────────────────────────────────────────────────┘
```

## B

```
┌─────────────────────────────────────────────────────────────┐
│ BARRIER (Barrière)                                          │
│ Point de synchronisation où tous les threads doivent        │
│ arriver avant que quiconque puisse continuer.               │
├─────────────────────────────────────────────────────────────┤
│ BIT                                                         │
│ Plus petite unité d'information (0 ou 1).                   │
├─────────────────────────────────────────────────────────────┤
│ BLOCKING (Bloquant)                                         │
│ Opération qui suspend le thread jusqu'à completion.         │
│ Opposé de non-bloquant (async).                             │
├─────────────────────────────────────────────────────────────┤
│ BORROWING (Emprunt)                                         │
│ En Rust : utiliser une référence sans prendre ownership.    │
│ Peut être immuable (&T) ou mutable (&mut T).                │
├─────────────────────────────────────────────────────────────┤
│ BUS                                                         │
│ Canal de communication entre composants de l'ordinateur.    │
│ Transporte données, adresses et signaux de contrôle.        │
├─────────────────────────────────────────────────────────────┤
│ BYTE (Octet)                                                │
│ 8 bits. Unité de base pour mesurer la mémoire.              │
│ 1 KB = 1024 bytes, 1 MB = 1024 KB, etc.                     │
└─────────────────────────────────────────────────────────────┘
```

## C

```
┌─────────────────────────────────────────────────────────────┐
│ CACHE                                                       │
│ Mémoire très rapide proche du CPU qui garde une copie       │
│ des données fréquemment utilisées.                          │
├─────────────────────────────────────────────────────────────┤
│ CACHE HIT                                                   │
│ La donnée demandée est dans le cache. Rapide !              │
├─────────────────────────────────────────────────────────────┤
│ CACHE MISS                                                  │
│ La donnée n'est pas dans le cache. Doit aller en RAM.       │
├─────────────────────────────────────────────────────────────┤
│ CHANNEL (Canal)                                             │
│ Mécanisme de communication entre threads par messages.      │
│ Types : MPSC, MPMC, SPSC.                                   │
├─────────────────────────────────────────────────────────────┤
│ CLOCK CYCLE (Cycle d'horloge)                               │
│ Un "battement" de l'horloge du CPU. Unité de temps          │
│ de base pour le processeur.                                 │
├─────────────────────────────────────────────────────────────┤
│ CONCURRENCE                                                 │
│ Gérer plusieurs tâches qui progressent dans la même         │
│ période. Pas forcément en même temps.                       │
├─────────────────────────────────────────────────────────────┤
│ CONDITION VARIABLE                                          │
│ Mécanisme pour qu'un thread attende qu'une condition        │
│ soit satisfaite, signalée par un autre thread.              │
├─────────────────────────────────────────────────────────────┤
│ CONTEXT SWITCH                                              │
│ Changement de thread par le scheduler. Sauvegarde           │
│ l'état d'un thread et charge celui d'un autre.              │
├─────────────────────────────────────────────────────────────┤
│ CORE (Cœur)                                                 │
│ Unité de calcul indépendante dans un CPU.                   │
│ Un CPU peut avoir plusieurs cœurs.                          │
├─────────────────────────────────────────────────────────────┤
│ CPU (Central Processing Unit)                               │
│ Processeur central. Le "cerveau" de l'ordinateur            │
│ qui exécute les instructions.                               │
├─────────────────────────────────────────────────────────────┤
│ CRITICAL SECTION (Section Critique)                         │
│ Portion de code qui accède à des ressources partagées       │
│ et doit être exécutée par un seul thread à la fois.         │
└─────────────────────────────────────────────────────────────┘
```

## D

```
┌─────────────────────────────────────────────────────────────┐
│ DATA RACE (Course aux données)                              │
│ Bug : deux threads accèdent à la même mémoire,              │
│ au moins un écrit, sans synchronisation.                    │
├─────────────────────────────────────────────────────────────┤
│ DEADLOCK (Interblocage)                                     │
│ Situation où des threads s'attendent mutuellement           │
│ indéfiniment, causant un blocage total.                     │
├─────────────────────────────────────────────────────────────┤
│ DECODE                                                      │
│ Étape du CPU où l'instruction est interprétée               │
│ pour savoir quelle opération effectuer.                     │
├─────────────────────────────────────────────────────────────┤
│ DMA (Direct Memory Access)                                  │
│ Transfert de données entre mémoire et périphériques         │
│ sans passer par le CPU.                                     │
└─────────────────────────────────────────────────────────────┘
```

## E-F

```
┌─────────────────────────────────────────────────────────────┐
│ EXECUTE                                                     │
│ Étape du CPU où l'opération est effectivement réalisée.     │
├─────────────────────────────────────────────────────────────┤
│ FETCH                                                       │
│ Étape du CPU où l'instruction est récupérée de la mémoire.  │
├─────────────────────────────────────────────────────────────┤
│ FIFO (First In, First Out)                                  │
│ File d'attente : premier arrivé, premier servi.             │
├─────────────────────────────────────────────────────────────┤
│ FPU (Floating Point Unit)                                   │
│ Partie du CPU spécialisée dans les calculs à virgule        │
│ flottante (nombres décimaux).                               │
├─────────────────────────────────────────────────────────────┤
│ FUTURE                                                      │
│ Représente une valeur qui sera disponible plus tard.        │
│ Base de la programmation asynchrone.                        │
└─────────────────────────────────────────────────────────────┘
```

## G-H

```
┌─────────────────────────────────────────────────────────────┐
│ GHz (Gigahertz)                                             │
│ 1 milliard de cycles par seconde. Mesure la fréquence       │
│ du processeur.                                              │
├─────────────────────────────────────────────────────────────┤
│ GPU (Graphics Processing Unit)                              │
│ Processeur graphique. Optimisé pour les calculs             │
│ parallèles massifs.                                         │
├─────────────────────────────────────────────────────────────┤
│ HEAP (Tas)                                                  │
│ Zone mémoire pour allocation dynamique. Plus flexible       │
│ mais plus lente que la stack.                               │
├─────────────────────────────────────────────────────────────┤
│ HYPER-THREADING                                             │
│ Technologie Intel permettant à un cœur physique             │
│ d'exécuter deux threads simultanément.                      │
└─────────────────────────────────────────────────────────────┘
```

## I-L

```
┌─────────────────────────────────────────────────────────────┐
│ I/O (Input/Output)                                          │
│ Opérations d'entrée/sortie : disque, réseau, clavier...     │
│ Généralement plus lentes que les calculs CPU.               │
├─────────────────────────────────────────────────────────────┤
│ IPC (Inter-Process Communication)                           │
│ Mécanismes de communication entre processus différents.     │
│ Exemples : pipes, sockets, mémoire partagée.                │
├─────────────────────────────────────────────────────────────┤
│ LATENCY (Latence)                                           │
│ Temps d'attente avant qu'une opération ne commence.         │
│ Exemple : latence RAM = ~100 ns.                            │
├─────────────────────────────────────────────────────────────┤
│ LIFETIME                                                    │
│ En Rust : durée pendant laquelle une référence est valide.  │
│ Le compilateur vérifie qu'on ne garde pas de références     │
│ vers des données détruites.                                 │
├─────────────────────────────────────────────────────────────┤
│ LIFO (Last In, First Out)                                   │
│ Pile : dernier arrivé, premier sorti.                       │
├─────────────────────────────────────────────────────────────┤
│ LIVELOCK                                                    │
│ Threads actifs mais qui ne progressent pas car ils          │
│ se répondent mutuellement sans avancer.                     │
├─────────────────────────────────────────────────────────────┤
│ LOCK (Verrou)                                               │
│ Mécanisme pour garantir l'accès exclusif à une ressource.   │
│ Un thread acquiert le lock, les autres attendent.           │
├─────────────────────────────────────────────────────────────┤
│ LOGICAL CORE (Cœur logique)                                 │
│ Cœur virtuel visible par l'OS. Avec Hyper-Threading,        │
│ 1 cœur physique = 2 cœurs logiques.                         │
└─────────────────────────────────────────────────────────────┘
```

## M

```
┌─────────────────────────────────────────────────────────────┐
│ MEMORY BARRIER / FENCE                                      │
│ Instruction qui garantit l'ordre des opérations mémoire.    │
│ Empêche le CPU de réordonner les lectures/écritures.        │
├─────────────────────────────────────────────────────────────┤
│ MPMC (Multiple Producer Multiple Consumer)                  │
│ Canal avec plusieurs expéditeurs et plusieurs récepteurs.   │
├─────────────────────────────────────────────────────────────┤
│ MPSC (Multiple Producer Single Consumer)                    │
│ Canal avec plusieurs expéditeurs mais un seul récepteur.    │
│ Type standard en Rust.                                      │
├─────────────────────────────────────────────────────────────┤
│ MUTEX (Mutual Exclusion)                                    │
│ Verrou garantissant qu'un seul thread accède à              │
│ une ressource à la fois.                                    │
└─────────────────────────────────────────────────────────────┘
```

## N-O

```
┌─────────────────────────────────────────────────────────────┐
│ NON-BLOCKING (Non-bloquant)                                 │
│ Opération qui retourne immédiatement, même si pas finie.    │
│ Permet de faire autre chose en attendant.                   │
├─────────────────────────────────────────────────────────────┤
│ OS (Operating System)                                       │
│ Système d'exploitation. Gère les ressources matérielles     │
│ et fournit des services aux programmes.                     │
├─────────────────────────────────────────────────────────────┤
│ OWNERSHIP (Propriété)                                       │
│ En Rust : chaque valeur a un propriétaire unique.           │
│ Quand le propriétaire sort du scope, la valeur est libérée. │
└─────────────────────────────────────────────────────────────┘
```

## P

```
┌─────────────────────────────────────────────────────────────┐
│ PARALLÉLISME                                                │
│ Exécution simultanée de plusieurs tâches sur plusieurs      │
│ unités de calcul. Vrai "en même temps".                     │
├─────────────────────────────────────────────────────────────┤
│ PHYSICAL CORE (Cœur physique)                               │
│ Véritable unité de calcul matérielle dans le CPU.           │
├─────────────────────────────────────────────────────────────┤
│ PID (Process ID)                                            │
│ Identifiant unique d'un processus attribué par l'OS.        │
├─────────────────────────────────────────────────────────────┤
│ PIPELINE                                                    │
│ Technique où le CPU travaille sur plusieurs instructions    │
│ en même temps, chacune à une étape différente.              │
├─────────────────────────────────────────────────────────────┤
│ PREEMPTION (Préemption)                                     │
│ Capacité de l'OS à interrompre un thread pour en            │
│ exécuter un autre.                                          │
├─────────────────────────────────────────────────────────────┤
│ PROCESS (Processus)                                         │
│ Programme en cours d'exécution avec son propre espace       │
│ mémoire isolé.                                              │
├─────────────────────────────────────────────────────────────┤
│ PRODUCER-CONSUMER                                           │
│ Pattern où des threads produisent des données et            │
│ d'autres les consomment via une file.                       │
└─────────────────────────────────────────────────────────────┘
```

## Q-R

```
┌─────────────────────────────────────────────────────────────┐
│ QUANTUM (Time slice)                                        │
│ Durée pendant laquelle un thread s'exécute avant            │
│ que le scheduler ne passe à un autre.                       │
├─────────────────────────────────────────────────────────────┤
│ RACE CONDITION                                              │
│ Bug où le résultat dépend de l'ordre (aléatoire)            │
│ d'exécution des threads.                                    │
├─────────────────────────────────────────────────────────────┤
│ RAM (Random Access Memory)                                  │
│ Mémoire vive. Stockage temporaire rapide pour les           │
│ programmes en cours d'exécution.                            │
├─────────────────────────────────────────────────────────────┤
│ REGISTER (Registre)                                         │
│ Mémoire ultra-rapide à l'intérieur du CPU.                  │
│ Stocke les données en cours de traitement.                  │
├─────────────────────────────────────────────────────────────┤
│ RWLOCK (Read-Write Lock)                                    │
│ Verrou permettant plusieurs lecteurs simultanés             │
│ OU un seul écrivain exclusif.                               │
└─────────────────────────────────────────────────────────────┘
```

## S

```
┌─────────────────────────────────────────────────────────────┐
│ SCHEDULER (Ordonnanceur)                                    │
│ Composant de l'OS qui décide quel thread s'exécute,         │
│ sur quel cœur, et pendant combien de temps.                 │
├─────────────────────────────────────────────────────────────┤
│ SEMAPHORE (Sémaphore)                                       │
│ Compteur limitant le nombre de threads pouvant              │
│ accéder simultanément à une ressource.                      │
├─────────────────────────────────────────────────────────────┤
│ SMT (Simultaneous Multi-Threading)                          │
│ Nom générique pour Hyper-Threading. Permet à un cœur        │
│ d'exécuter plusieurs threads.                               │
├─────────────────────────────────────────────────────────────┤
│ SPAWN                                                       │
│ Créer un nouveau thread.                                    │
├─────────────────────────────────────────────────────────────┤
│ SPINLOCK                                                    │
│ Verrou où le thread attend activement (boucle)              │
│ au lieu de dormir. Utile pour attentes très courtes.        │
├─────────────────────────────────────────────────────────────┤
│ STACK (Pile)                                                │
│ Zone mémoire pour variables locales et appels fonction.     │
│ Allocation/désallocation très rapide (LIFO).                │
├─────────────────────────────────────────────────────────────┤
│ STARVATION (Famine)                                         │
│ Un thread ne peut jamais accéder à une ressource car        │
│ d'autres passent toujours avant.                            │
├─────────────────────────────────────────────────────────────┤
│ SYNCHRONE                                                   │
│ Exécution qui bloque en attendant la fin de l'opération.    │
│ Opposé d'asynchrone.                                        │
└─────────────────────────────────────────────────────────────┘
```

## T

```
┌─────────────────────────────────────────────────────────────┐
│ THREAD                                                      │
│ Fil d'exécution au sein d'un processus. Plus léger          │
│ qu'un processus, partage la mémoire avec les autres threads.│
├─────────────────────────────────────────────────────────────┤
│ THREAD POOL                                                 │
│ Groupe de threads pré-créés qui attendent des tâches.       │
│ Évite le coût de création/destruction de threads.           │
├─────────────────────────────────────────────────────────────┤
│ THREAD-SAFE                                                 │
│ Code qui peut être appelé par plusieurs threads             │
│ simultanément sans causer de bugs.                          │
├─────────────────────────────────────────────────────────────┤
│ THROUGHPUT (Débit)                                          │
│ Quantité de travail effectuée par unité de temps.           │
│ Exemple : requêtes par seconde.                             │
└─────────────────────────────────────────────────────────────┘
```

## U-Z

```
┌─────────────────────────────────────────────────────────────┐
│ VOLATILE                                                    │
│ Indique au compilateur de ne pas optimiser les accès        │
│ à cette variable car elle peut changer de l'extérieur.      │
├─────────────────────────────────────────────────────────────┤
│ WORK STEALING                                               │
│ Technique où un thread inactif "vole" du travail            │
│ à un thread occupé pour équilibrer la charge.               │
├─────────────────────────────────────────────────────────────┤
│ YIELD                                                       │
│ Un thread cède volontairement son temps CPU                 │
│ pour laisser d'autres threads s'exécuter.                   │
└─────────────────────────────────────────────────────────────┘
```

---

# Conclusion

Vous avez maintenant toutes les bases pour comprendre la programmation concurrente :

```
┌─────────────────────────────────────────────────────────────┐
│                    CE QUE VOUS SAVEZ                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ✓ Comment fonctionne un ordinateur                         │
│  ✓ Ce qu'est un CPU, ses composants, son fonctionnement     │
│  ✓ La hiérarchie de mémoire et pourquoi elle existe         │
│  ✓ La différence entre cœurs physiques et logiques          │
│  ✓ Processus vs Threads                                     │
│  ✓ Concurrence vs Parallélisme                              │
│  ✓ Les problèmes classiques (race, deadlock, etc.)          │
│  ✓ Les solutions de synchronisation                         │
│  ✓ Tout le vocabulaire technique                            │
│                                                             │
│  PROCHAINE ÉTAPE :                                          │
│  Pratiquer avec Rust ! Le cours précédent vous montre       │
│  comment utiliser ces concepts en code.                     │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

*Ce cours pose les fondations théoriques. La pratique avec du vrai code est essentielle pour maîtriser ces concepts !*
