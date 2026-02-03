# Cours Complet Rust : De A à Z
## Avec Focus sur la Programmation Concurrente et les Threads

---

# Table des Matières

1. [Introduction à Rust](#partie-1--introduction-à-rust)
2. [Les Fondamentaux](#partie-2--les-fondamentaux-de-rust)
3. [Ownership, Borrowing et Lifetimes](#partie-3--ownership-borrowing-et-lifetimes)
4. [Structures de Données Avancées](#partie-4--structures-de-données-avancées)
5. [Gestion des Erreurs](#partie-5--gestion-des-erreurs)
6. [Traits et Généricité](#partie-6--traits-et-généricité)
7. [Programmation Concurrente - Introduction](#partie-7--programmation-concurrente---introduction)
8. [Les Threads en Rust](#partie-8--les-threads-en-rust)
9. [Synchronisation et Mutex](#partie-9--synchronisation-et-mutex)
10. [Canaux de Communication](#partie-10--canaux-de-communication-channels)
11. [Programmation Asynchrone](#partie-11--programmation-asynchrone)
12. [Patterns Avancés de Concurrence](#partie-12--patterns-avancés-de-concurrence)
13. [Projets Pratiques](#partie-13--projets-pratiques)

---

# Partie 1 : Introduction à Rust

## 1.1 Qu'est-ce que Rust ?

Rust est un langage de programmation système créé par Mozilla, conçu pour être sûr, concurrent et pratique. Il garantit la sécurité mémoire sans garbage collector grâce à son système unique d'ownership.

### Pourquoi Rust ?

- **Sécurité mémoire** : Pas de null pointers, pas de data races
- **Performance** : Comparable à C/C++
- **Concurrence sans peur** : Le compilateur empêche les bugs de concurrence
- **Zero-cost abstractions** : Les abstractions n'ont pas de coût runtime

## 1.2 Installation

```bash
# Installation via rustup (recommandé)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Vérifier l'installation
rustc --version
cargo --version
```

## 1.3 Premier Programme

```rust
fn main() {
    println!("Bonjour, Rust !");
}
```

Compilation et exécution :
```bash
rustc main.rs
./main

# Ou avec Cargo (gestionnaire de projet)
cargo new mon_projet
cd mon_projet
cargo run
```

## 1.4 Structure d'un Projet Cargo

```
mon_projet/
├── Cargo.toml      # Configuration et dépendances
├── Cargo.lock      # Versions exactes des dépendances
└── src/
    └── main.rs     # Point d'entrée
```

**Cargo.toml** :
```toml
[package]
name = "mon_projet"
version = "0.1.0"
edition = "2021"

[dependencies]
serde = "1.0"
tokio = { version = "1", features = ["full"] }
```

---

# Partie 2 : Les Fondamentaux de Rust

## 2.1 Variables et Mutabilité

```rust
fn main() {
    // Variables immuables par défaut
    let x = 5;
    // x = 6; // ERREUR ! x est immuable
    
    // Variables mutables
    let mut y = 5;
    y = 6; // OK
    
    // Shadowing (redéfinition)
    let z = 5;
    let z = z + 1;  // Nouveau z qui "masque" l'ancien
    let z = "texte"; // Peut même changer de type
    
    // Constantes (toujours immuables, type obligatoire)
    const MAX_POINTS: u32 = 100_000;
}
```

## 2.2 Types de Données

### Types Scalaires

```rust
fn main() {
    // Entiers signés : i8, i16, i32, i64, i128, isize
    let a: i32 = -42;
    
    // Entiers non signés : u8, u16, u32, u64, u128, usize
    let b: u64 = 42;
    
    // Flottants : f32, f64
    let c: f64 = 3.14159;
    
    // Booléens
    let d: bool = true;
    
    // Caractères (Unicode, 4 octets)
    let e: char = '🦀';
    
    // Littéraux numériques
    let decimal = 98_222;      // Séparateur visuel
    let hex = 0xff;
    let octal = 0o77;
    let binary = 0b1111_0000;
    let byte = b'A';           // u8 seulement
}
```

### Types Composés

```rust
fn main() {
    // Tuples (taille fixe, types hétérogènes)
    let tuple: (i32, f64, char) = (500, 6.4, 'a');
    let (x, y, z) = tuple;  // Destructuration
    let premier = tuple.0;   // Accès par index
    
    // Arrays (taille fixe, type homogène)
    let array: [i32; 5] = [1, 2, 3, 4, 5];
    let zeros = [0; 5];      // [0, 0, 0, 0, 0]
    let element = array[0];
    
    // Slices (vue sur une portion)
    let slice: &[i32] = &array[1..3]; // [2, 3]
}
```

## 2.3 Fonctions

```rust
// Fonction basique
fn dire_bonjour() {
    println!("Bonjour !");
}

// Avec paramètres
fn saluer(nom: &str) {
    println!("Bonjour, {} !", nom);
}

// Avec retour
fn additionner(a: i32, b: i32) -> i32 {
    a + b  // Pas de ; = expression retournée
}

// Retour anticipé
fn valeur_absolue(n: i32) -> i32 {
    if n < 0 {
        return -n;
    }
    n
}

// Fonctions génériques
fn premier<T>(liste: &[T]) -> Option<&T> {
    liste.first()
}

fn main() {
    dire_bonjour();
    saluer("Alice");
    let somme = additionner(5, 3);
    println!("Somme: {}", somme);
}
```

## 2.4 Structures de Contrôle

### Conditions

```rust
fn main() {
    let nombre = 7;
    
    // if/else classique
    if nombre < 5 {
        println!("Petit");
    } else if nombre < 10 {
        println!("Moyen");
    } else {
        println!("Grand");
    }
    
    // if comme expression
    let taille = if nombre < 5 { "petit" } else { "grand" };
    
    // match (pattern matching puissant)
    let resultat = match nombre {
        1 => "un",
        2 | 3 => "deux ou trois",
        4..=6 => "entre quatre et six",
        n if n > 10 => "grand nombre",
        _ => "autre",
    };
    
    // if let (pattern matching simplifié)
    let option = Some(5);
    if let Some(valeur) = option {
        println!("Valeur: {}", valeur);
    }
}
```

### Boucles

```rust
fn main() {
    // loop (boucle infinie)
    let mut compteur = 0;
    let resultat = loop {
        compteur += 1;
        if compteur == 10 {
            break compteur * 2;  // Retourne une valeur
        }
    };
    
    // while
    let mut n = 3;
    while n != 0 {
        println!("{}!", n);
        n -= 1;
    }
    
    // for (itération)
    let tableau = [10, 20, 30, 40, 50];
    for element in tableau.iter() {
        println!("Valeur: {}", element);
    }
    
    // for avec range
    for i in 0..5 {
        println!("Index: {}", i);
    }
    
    // for avec enumerate
    for (index, valeur) in tableau.iter().enumerate() {
        println!("{}: {}", index, valeur);
    }
    
    // Labels pour boucles imbriquées
    'externe: for i in 0..3 {
        for j in 0..3 {
            if i == 1 && j == 1 {
                break 'externe;
            }
            println!("({}, {})", i, j);
        }
    }
}
```

## 2.5 Strings

```rust
fn main() {
    // String (heap, growable)
    let mut s = String::new();
    s.push_str("Bonjour");
    s.push(' ');
    s.push_str("monde");
    
    let s2 = String::from("Création depuis &str");
    let s3 = "Littéral".to_string();
    
    // &str (string slice, référence)
    let slice: &str = "Je suis un slice";
    let partie: &str = &s[0..7];
    
    // Concaténation
    let hello = String::from("Hello, ");
    let world = String::from("World!");
    let phrase = hello + &world; // hello est consommé
    
    // format! (ne consomme pas)
    let s1 = String::from("tic");
    let s2 = String::from("tac");
    let s3 = String::from("toe");
    let s = format!("{}-{}-{}", s1, s2, s3);
    
    // Itération sur caractères
    for c in "नमस्ते".chars() {
        println!("{}", c);
    }
    
    // Itération sur octets
    for b in "नमस्ते".bytes() {
        println!("{}", b);
    }
}
```

## 2.6 Collections

### Vec<T>

```rust
fn main() {
    // Création
    let mut v: Vec<i32> = Vec::new();
    let v2 = vec![1, 2, 3];
    
    // Ajout
    v.push(5);
    v.push(6);
    v.push(7);
    
    // Accès
    let troisieme: &i32 = &v2[2];
    let troisieme: Option<&i32> = v2.get(2);
    
    // Accès sécurisé
    match v2.get(100) {
        Some(valeur) => println!("Valeur: {}", valeur),
        None => println!("Index hors limites"),
    }
    
    // Modification
    for i in &mut v {
        *i += 10;
    }
    
    // Suppression
    let dernier = v.pop();  // Option<T>
    v.remove(0);            // Supprime à l'index
    
    // Enum pour types hétérogènes
    enum CelluleTableur {
        Int(i32),
        Float(f64),
        Texte(String),
    }
    
    let ligne: Vec<CelluleTableur> = vec![
        CelluleTableur::Int(3),
        CelluleTableur::Texte(String::from("bleu")),
        CelluleTableur::Float(10.12),
    ];
}
```

### HashMap<K, V>

```rust
use std::collections::HashMap;

fn main() {
    // Création
    let mut scores = HashMap::new();
    
    // Insertion
    scores.insert(String::from("Bleu"), 10);
    scores.insert(String::from("Rouge"), 50);
    
    // Depuis vecteurs
    let equipes = vec![String::from("Bleu"), String::from("Rouge")];
    let scores_init = vec![10, 50];
    let scores: HashMap<_, _> = equipes.into_iter()
        .zip(scores_init.into_iter())
        .collect();
    
    // Accès
    let nom_equipe = String::from("Bleu");
    let score = scores.get(&nom_equipe);  // Option<&V>
    
    // Itération
    for (cle, valeur) in &scores {
        println!("{}: {}", cle, valeur);
    }
    
    // Mise à jour
    let mut scores = HashMap::new();
    scores.insert(String::from("Bleu"), 10);
    scores.insert(String::from("Bleu"), 25);  // Écrase
    
    // Insérer si absent
    scores.entry(String::from("Jaune")).or_insert(50);
    scores.entry(String::from("Bleu")).or_insert(50);  // Non inséré
    
    // Modifier selon valeur existante
    let texte = "hello world wonderful world";
    let mut compteur = HashMap::new();
    for mot in texte.split_whitespace() {
        let count = compteur.entry(mot).or_insert(0);
        *count += 1;
    }
}
```

### HashSet<T>

```rust
use std::collections::HashSet;

fn main() {
    let mut livres = HashSet::new();
    
    livres.insert("Le Seigneur des Anneaux");
    livres.insert("Harry Potter");
    livres.insert("Le Seigneur des Anneaux");  // Ignoré (doublon)
    
    // Vérification
    if livres.contains("Harry Potter") {
        println!("Livre trouvé !");
    }
    
    // Opérations ensemblistes
    let a: HashSet<_> = [1, 2, 3].iter().cloned().collect();
    let b: HashSet<_> = [2, 3, 4].iter().cloned().collect();
    
    let union: HashSet<_> = a.union(&b).collect();
    let intersection: HashSet<_> = a.intersection(&b).collect();
    let difference: HashSet<_> = a.difference(&b).collect();
    let sym_diff: HashSet<_> = a.symmetric_difference(&b).collect();
}
```

---

# Partie 3 : Ownership, Borrowing et Lifetimes

## 3.1 Le Système d'Ownership

C'est LE concept fondamental de Rust qui garantit la sécurité mémoire.

### Les Trois Règles

1. Chaque valeur a un propriétaire (owner)
2. Il ne peut y avoir qu'un seul propriétaire à la fois
3. Quand le propriétaire sort du scope, la valeur est supprimée (drop)

```rust
fn main() {
    // s entre en scope
    let s = String::from("hello");
    
    // s est déplacé (move) vers s2
    let s2 = s;
    
    // println!("{}", s);  // ERREUR ! s n'est plus valide
    println!("{}", s2);    // OK
    
}   // s2 sort du scope et est libéré

// Pour les types simples (Copy trait)
fn main() {
    let x = 5;
    let y = x;  // Copie, pas déplacement
    
    println!("x = {}, y = {}", x, y);  // Les deux sont valides
}
```

### Move et Clone

```rust
fn main() {
    let s1 = String::from("hello");
    
    // Move (transfert de propriété)
    let s2 = s1;
    // s1 n'est plus utilisable
    
    // Clone (copie profonde)
    let s3 = s2.clone();
    println!("s2 = {}, s3 = {}", s2, s3);  // Les deux sont valides
}

// Avec les fonctions
fn prend_ownership(s: String) {
    println!("{}", s);
}   // s est libéré ici

fn fait_copie(x: i32) {
    println!("{}", x);
}   // x est copié, l'original reste valide

fn main() {
    let s = String::from("hello");
    prend_ownership(s);
    // println!("{}", s);  // ERREUR ! s a été déplacé
    
    let x = 5;
    fait_copie(x);
    println!("{}", x);  // OK, x a été copié
}
```

## 3.2 Références et Borrowing

Le borrowing permet d'utiliser une valeur sans en prendre la propriété.

### Références Immuables

```rust
fn calculer_longueur(s: &String) -> usize {
    s.len()
}   // s sort du scope mais n'est pas libéré (c'est une référence)

fn main() {
    let s1 = String::from("hello");
    
    let len = calculer_longueur(&s1);
    
    println!("La longueur de '{}' est {}.", s1, len);  // s1 est toujours valide
    
    // Plusieurs références immuables simultanées : OK
    let r1 = &s1;
    let r2 = &s1;
    println!("{} et {}", r1, r2);
}
```

### Références Mutables

```rust
fn modifier(s: &mut String) {
    s.push_str(", monde");
}

fn main() {
    let mut s = String::from("hello");
    
    modifier(&mut s);
    println!("{}", s);  // "hello, monde"
    
    // UNE SEULE référence mutable à la fois
    let r1 = &mut s;
    // let r2 = &mut s;  // ERREUR !
    println!("{}", r1);
    
    // Après que r1 n'est plus utilisé, on peut créer r2
    let r2 = &mut s;
    println!("{}", r2);
}
```

### Règles du Borrowing

```rust
fn main() {
    let mut s = String::from("hello");
    
    // INTERDIT : référence mutable + référence immuable simultanées
    // let r1 = &s;
    // let r2 = &mut s;  // ERREUR !
    
    // OK : les références immuables ne sont plus utilisées
    let r1 = &s;
    let r2 = &s;
    println!("{} et {}", r1, r2);
    // r1 et r2 ne sont plus utilisés après ce point
    
    let r3 = &mut s;  // OK
    println!("{}", r3);
}
```

### Références Pendantes (Dangling)

```rust
// ERREUR DE COMPILATION - Rust empêche les références pendantes
// fn dangle() -> &String {
//     let s = String::from("hello");
//     &s  // Référence vers s qui sera libéré !
// }

// Solution : retourner la valeur directement
fn pas_de_dangle() -> String {
    let s = String::from("hello");
    s  // Ownership transféré
}
```

## 3.3 Lifetimes (Durées de Vie)

Les lifetimes indiquent au compilateur combien de temps les références restent valides.

### Lifetime Implicite

```rust
// Ces fonctions ont des lifetimes implicites
fn premiere_mot(s: &str) -> &str {
    let octets = s.as_bytes();
    
    for (i, &item) in octets.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    
    &s[..]
}
```

### Annotations de Lifetime

```rust
// 'a est un paramètre de lifetime
fn plus_long<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let string1 = String::from("long string is long");
    
    {
        let string2 = String::from("xyz");
        let resultat = plus_long(string1.as_str(), string2.as_str());
        println!("La plus longue chaîne est {}", resultat);
    }
}
```

### Lifetime dans les Structs

```rust
// La struct ne peut pas vivre plus longtemps que la référence qu'elle contient
struct Extrait<'a> {
    partie: &'a str,
}

impl<'a> Extrait<'a> {
    fn niveau(&self) -> i32 {
        3
    }
    
    fn annoncer_et_retourner_partie(&self, annonce: &str) -> &str {
        println!("Attention: {}", annonce);
        self.partie
    }
}

fn main() {
    let roman = String::from("Appelez-moi Ishmael. Il y a quelques années...");
    let premiere_phrase = roman.split('.').next().expect("Pas de '.'");
    let i = Extrait {
        partie: premiere_phrase,
    };
    println!("Extrait: {}", i.partie);
}
```

### Lifetime Statique

```rust
// 'static : la référence peut vivre pour toute la durée du programme
let s: &'static str = "J'ai une durée de vie statique.";

// Souvent utilisé avec les messages d'erreur
fn message_erreur() -> &'static str {
    "Une erreur s'est produite"
}
```

### Élision des Lifetimes

Le compilateur applique trois règles pour inférer les lifetimes :

```rust
// Règle 1 : Chaque référence en entrée a son propre lifetime
fn foo<'a>(x: &'a i32) {}
fn foo<'a, 'b>(x: &'a i32, y: &'b i32) {}

// Règle 2 : S'il y a exactement un lifetime en entrée, il est assigné à toutes les sorties
fn foo<'a>(x: &'a i32) -> &'a i32 { x }

// Règle 3 : S'il y a &self ou &mut self, son lifetime est assigné aux sorties
impl<'a> Extrait<'a> {
    fn niveau(&self) -> i32 { 3 }
}
```

---

# Partie 4 : Structures de Données Avancées

## 4.1 Structs

```rust
// Définition
struct Utilisateur {
    actif: bool,
    nom_utilisateur: String,
    email: String,
    connexions: u64,
}

// Instanciation
fn main() {
    let mut user1 = Utilisateur {
        email: String::from("quelquun@exemple.com"),
        nom_utilisateur: String::from("pseudonyme123"),
        actif: true,
        connexions: 1,
    };
    
    // Modification (si mut)
    user1.email = String::from("autre@exemple.com");
    
    // Shorthand init
    let email = String::from("test@test.com");
    let user2 = Utilisateur {
        email,  // Même nom de variable
        nom_utilisateur: String::from("test"),
        actif: true,
        connexions: 1,
    };
    
    // Struct update syntax
    let user3 = Utilisateur {
        email: String::from("nouveau@exemple.com"),
        ..user2  // Copie les autres champs
    };
}
```

### Tuple Structs et Unit Structs

```rust
// Tuple structs
struct Couleur(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let noir = Couleur(0, 0, 0);
    let origine = Point(0, 0, 0);
    
    println!("R: {}", noir.0);
}

// Unit struct (sans champs)
struct ToujoursEgal;

fn main() {
    let sujet = ToujoursEgal;
}
```

## 4.2 Méthodes et Fonctions Associées

```rust
#[derive(Debug)]
struct Rectangle {
    largeur: u32,
    hauteur: u32,
}

impl Rectangle {
    // Fonction associée (constructeur)
    fn new(largeur: u32, hauteur: u32) -> Self {
        Self { largeur, hauteur }
    }
    
    // Fonction associée (pas de self)
    fn carre(taille: u32) -> Self {
        Self {
            largeur: taille,
            hauteur: taille,
        }
    }
    
    // Méthode (prend &self)
    fn aire(&self) -> u32 {
        self.largeur * self.hauteur
    }
    
    // Méthode mutable
    fn agrandir(&mut self, facteur: u32) {
        self.largeur *= facteur;
        self.hauteur *= facteur;
    }
    
    // Méthode avec paramètres
    fn peut_contenir(&self, autre: &Rectangle) -> bool {
        self.largeur > autre.largeur && self.hauteur > autre.hauteur
    }
}

fn main() {
    let rect1 = Rectangle::new(30, 50);
    let rect2 = Rectangle::carre(10);
    
    println!("Aire: {}", rect1.aire());
    println!("rect1 peut contenir rect2: {}", rect1.peut_contenir(&rect2));
    
    let mut rect3 = Rectangle::new(5, 5);
    rect3.agrandir(2);
    println!("{:?}", rect3);
}
```

## 4.3 Enums

```rust
// Enum basique
enum Direction {
    Nord,
    Sud,
    Est,
    Ouest,
}

// Enum avec données
enum Message {
    Quitter,
    Deplacer { x: i32, y: i32 },
    Ecrire(String),
    ChangerCouleur(i32, i32, i32),
}

impl Message {
    fn appeler(&self) {
        match self {
            Message::Quitter => println!("Quitter"),
            Message::Deplacer { x, y } => println!("Déplacer à ({}, {})", x, y),
            Message::Ecrire(texte) => println!("Message: {}", texte),
            Message::ChangerCouleur(r, g, b) => println!("RGB({}, {}, {})", r, g, b),
        }
    }
}

fn main() {
    let m1 = Message::Quitter;
    let m2 = Message::Deplacer { x: 10, y: 20 };
    let m3 = Message::Ecrire(String::from("bonjour"));
    let m4 = Message::ChangerCouleur(255, 0, 0);
    
    m1.appeler();
    m2.appeler();
}
```

### Option<T>

```rust
fn main() {
    let quelque_nombre: Option<i32> = Some(5);
    let aucun_nombre: Option<i32> = None;
    
    // Pattern matching
    match quelque_nombre {
        Some(n) => println!("Le nombre est {}", n),
        None => println!("Pas de nombre"),
    }
    
    // Méthodes utiles
    let x = quelque_nombre.unwrap();           // Panic si None
    let y = quelque_nombre.unwrap_or(0);       // Valeur par défaut
    let z = quelque_nombre.unwrap_or_else(|| {
        // Calcul coûteux seulement si None
        calcul_complexe()
    });
    
    // map
    let double = quelque_nombre.map(|n| n * 2);
    
    // and_then (flatmap)
    let resultat = quelque_nombre.and_then(|n| {
        if n > 0 { Some(n * 2) } else { None }
    });
    
    // is_some, is_none
    if quelque_nombre.is_some() {
        println!("Contient une valeur");
    }
    
    // if let
    if let Some(n) = quelque_nombre {
        println!("La valeur est {}", n);
    }
    
    // let-else (Rust 1.65+)
    let Some(n) = quelque_nombre else {
        println!("Pas de valeur");
        return;
    };
    println!("n = {}", n);
}

fn calcul_complexe() -> i32 {
    42
}
```

## 4.4 Pattern Matching Avancé

```rust
fn main() {
    let x = 5;
    
    // Patterns multiples
    match x {
        1 | 2 => println!("un ou deux"),
        3..=5 => println!("trois à cinq"),
        _ => println!("autre"),
    }
    
    // Destructuration de structs
    struct Point { x: i32, y: i32 }
    let p = Point { x: 0, y: 7 };
    
    match p {
        Point { x, y: 0 } => println!("Sur l'axe x à {}", x),
        Point { x: 0, y } => println!("Sur l'axe y à {}", y),
        Point { x, y } => println!("Point({}, {})", x, y),
    }
    
    // Destructuration d'enums
    enum Couleur {
        Rgb(i32, i32, i32),
        Hsv(i32, i32, i32),
    }
    
    let couleur = Couleur::Rgb(0, 160, 255);
    
    match couleur {
        Couleur::Rgb(r, g, b) => println!("RGB: {}, {}, {}", r, g, b),
        Couleur::Hsv(h, s, v) => println!("HSV: {}, {}, {}", h, s, v),
    }
    
    // Guards
    let num = Some(4);
    
    match num {
        Some(x) if x < 5 => println!("moins de cinq: {}", x),
        Some(x) => println!("{}", x),
        None => (),
    }
    
    // @ binding
    enum Message {
        Hello { id: i32 },
    }
    
    let msg = Message::Hello { id: 5 };
    
    match msg {
        Message::Hello { id: id_var @ 3..=7 } => {
            println!("ID dans la plage: {}", id_var)
        }
        Message::Hello { id: 10..=12 } => {
            println!("ID dans une autre plage")
        }
        Message::Hello { id } => println!("Autre ID: {}", id),
    }
    
    // Ignorer des valeurs
    let nombres = (2, 4, 8, 16, 32);
    match nombres {
        (premier, _, troisieme, _, cinquieme) => {
            println!("{}, {}, {}", premier, troisieme, cinquieme)
        }
    }
    
    // .. pour ignorer le reste
    let Point { x, .. } = p;
    println!("x = {}", x);
}
```

---

# Partie 5 : Gestion des Erreurs

## 5.1 Panic! et Erreurs Irrécupérables

```rust
fn main() {
    // Panic explicite
    // panic!("crash and burn");
    
    // Panic implicite (accès hors limites)
    let v = vec![1, 2, 3];
    // v[99];  // Panic!
    
    // RUST_BACKTRACE=1 pour voir la trace
}
```

## 5.2 Result<T, E> et Erreurs Récupérables

```rust
use std::fs::File;
use std::io::{self, Read, ErrorKind};

fn main() {
    // Ouverture de fichier avec Result
    let greeting_file_result = File::open("hello.txt");
    
    // Gestion avec match
    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Impossible de créer le fichier: {:?}", e),
            },
            other_error => {
                panic!("Impossible d'ouvrir le fichier: {:?}", other_error);
            }
        },
    };
    
    // Plus élégant avec unwrap_or_else
    let greeting_file = File::open("hello.txt").unwrap_or_else(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Impossible de créer le fichier: {:?}", error);
            })
        } else {
            panic!("Impossible d'ouvrir le fichier: {:?}", error);
        }
    });
}

// Méthodes raccourcies
fn main() {
    // unwrap : panic si Err
    let f = File::open("hello.txt").unwrap();
    
    // expect : panic avec message personnalisé
    let f = File::open("hello.txt")
        .expect("hello.txt devrait exister");
}
```

## 5.3 Propagation des Erreurs

```rust
use std::fs::File;
use std::io::{self, Read};

// Propagation manuelle
fn lire_username_du_fichier() -> Result<String, io::Error> {
    let username_file_result = File::open("hello.txt");
    
    let mut username_file = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e),
    };
    
    let mut username = String::new();
    
    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}

// Avec l'opérateur ?
fn lire_username_du_fichier_court() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}

// Encore plus court avec chaînage
fn lire_username_du_fichier_chaine() -> Result<String, io::Error> {
    let mut username = String::new();
    File::open("hello.txt")?.read_to_string(&mut username)?;
    Ok(username)
}

// Le plus court avec fs::read_to_string
use std::fs;

fn lire_username_du_fichier_ultra_court() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```

## 5.4 Erreurs Personnalisées

```rust
use std::fmt;
use std::error::Error;

// Définition d'une erreur personnalisée
#[derive(Debug)]
struct MonErreur {
    details: String,
}

impl MonErreur {
    fn new(msg: &str) -> MonErreur {
        MonErreur { details: msg.to_string() }
    }
}

impl fmt::Display for MonErreur {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "{}", self.details)
    }
}

impl Error for MonErreur {}

fn faire_quelque_chose(valeur: i32) -> Result<i32, MonErreur> {
    if valeur < 0 {
        Err(MonErreur::new("La valeur ne peut pas être négative"))
    } else {
        Ok(valeur * 2)
    }
}

// Avec thiserror (crate populaire)
use thiserror::Error;

#[derive(Error, Debug)]
enum DataStoreError {
    #[error("Erreur de données: {0}")]
    DataCorrupted(String),
    
    #[error("Lecture impossible")]
    ReadError(#[from] std::io::Error),
    
    #[error("Valeur invalide: attendu {expected:?}, obtenu {found:?}")]
    InvalidValue {
        expected: String,
        found: String,
    },
}
```

## 5.5 anyhow pour la Simplicité

```rust
use anyhow::{Context, Result, anyhow};

fn get_cluster_info() -> Result<String> {
    let config = std::fs::read_to_string("cluster.json")
        .context("Impossible de lire le fichier de configuration")?;
    
    // Erreur personnalisée avec anyhow!
    if config.is_empty() {
        return Err(anyhow!("Le fichier de configuration est vide"));
    }
    
    Ok(config)
}

fn main() -> Result<()> {
    let info = get_cluster_info()?;
    println!("{}", info);
    Ok(())
}
```

---

# Partie 6 : Traits et Généricité

## 6.1 Traits Basiques

```rust
// Définition d'un trait
trait Resume {
    fn resumer(&self) -> String;
    
    // Méthode par défaut
    fn resumer_auteur(&self) -> String {
        String::from("(Lisez plus ...)")
    }
}

// Implémentation pour un type
struct ArticleDeJournal {
    titre: String,
    auteur: String,
    contenu: String,
}

impl Resume for ArticleDeJournal {
    fn resumer(&self) -> String {
        format!("{}, par {}", self.titre, self.auteur)
    }
}

struct Tweet {
    username: String,
    contenu: String,
}

impl Resume for Tweet {
    fn resumer(&self) -> String {
        format!("{}: {}", self.username, self.contenu)
    }
    
    // Override de la méthode par défaut
    fn resumer_auteur(&self) -> String {
        format!("@{}", self.username)
    }
}

fn main() {
    let article = ArticleDeJournal {
        titre: String::from("Rust est génial"),
        auteur: String::from("Alice"),
        contenu: String::from("Contenu..."),
    };
    
    println!("{}", article.resumer());
}
```

## 6.2 Traits comme Paramètres

```rust
// Syntaxe impl Trait
fn notifier(item: &impl Resume) {
    println!("Actualité ! {}", item.resumer());
}

// Syntaxe trait bound
fn notifier_bound<T: Resume>(item: &T) {
    println!("Actualité ! {}", item.resumer());
}

// Plusieurs traits
fn notifier_et_afficher(item: &(impl Resume + std::fmt::Display)) {
    println!("{}", item);
    println!("{}", item.resumer());
}

// Avec where clause (plus lisible)
fn une_fonction<T, U>(t: &T, u: &U) -> i32
where
    T: std::fmt::Display + Clone,
    U: Clone + std::fmt::Debug,
{
    42
}

// Retourner un type implémentant un trait
fn retourne_resumable() -> impl Resume {
    Tweet {
        username: String::from("horse_ebooks"),
        contenu: String::from("contenu"),
    }
}
```

## 6.3 Génériques

```rust
// Fonction générique
fn plus_grand<T: PartialOrd>(liste: &[T]) -> &T {
    let mut plus_grand = &liste[0];
    
    for item in liste {
        if item > plus_grand {
            plus_grand = item;
        }
    }
    
    plus_grand
}

// Struct générique
struct Point<T> {
    x: T,
    y: T,
}

// Struct avec plusieurs types génériques
struct PointMixte<T, U> {
    x: T,
    y: U,
}

// Implémentation générique
impl<T> Point<T> {
    fn x(&self) -> &T {
        &self.x
    }
}

// Implémentation spécifique
impl Point<f32> {
    fn distance_depuis_origine(&self) -> f32 {
        (self.x.powi(2) + self.y.powi(2)).sqrt()
    }
}

// Méthode avec génériques différents
impl<T, U> PointMixte<T, U> {
    fn mixup<V, W>(self, autre: PointMixte<V, W>) -> PointMixte<T, W> {
        PointMixte {
            x: self.x,
            y: autre.y,
        }
    }
}

fn main() {
    let entier = Point { x: 5, y: 10 };
    let flottant = Point { x: 1.0, y: 4.0 };
    let mixte = PointMixte { x: 5, y: 4.0 };
    
    println!("entier.x = {}", entier.x());
    println!("distance = {}", flottant.distance_depuis_origine());
}
```

## 6.4 Traits Dérivables

```rust
// Derive automatique de traits courants
#[derive(Debug, Clone, Copy, PartialEq, Eq, PartialOrd, Ord, Hash, Default)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p1 = Point { x: 1, y: 2 };
    let p2 = p1;  // Copy
    
    println!("{:?}", p1);  // Debug
    println!("{}", p1 == p2);  // PartialEq
    
    let p3 = Point::default();  // Default
}
```

## 6.5 Traits Standards Importants

```rust
use std::ops::{Add, Deref};
use std::fmt;

// From et Into
struct Nombre(i32);

impl From<i32> for Nombre {
    fn from(item: i32) -> Self {
        Nombre(item)
    }
}

fn main() {
    let num = Nombre::from(30);
    let num: Nombre = 30.into();  // Into est automatique si From est implémenté
}

// Add pour surcharger +
#[derive(Debug, Copy, Clone)]
struct Point {
    x: i32,
    y: i32,
}

impl Add for Point {
    type Output = Point;
    
    fn add(self, other: Point) -> Point {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 1, y: 0 };
    let p2 = Point { x: 2, y: 3 };
    let p3 = p1 + p2;
    println!("{:?}", p3);
}

// Display
struct Wrapper(Vec<String>);

impl fmt::Display for Wrapper {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "[{}]", self.0.join(", "))
    }
}

// Iterator
struct Compteur {
    count: u32,
}

impl Compteur {
    fn new() -> Compteur {
        Compteur { count: 0 }
    }
}

impl Iterator for Compteur {
    type Item = u32;
    
    fn next(&mut self) -> Option<Self::Item> {
        if self.count < 5 {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}

fn main() {
    let sum: u32 = Compteur::new().sum();
    println!("Somme: {}", sum);
}
```

## 6.6 Objets Trait (Trait Objects)

```rust
// dyn Trait pour polymorphisme dynamique
trait Draw {
    fn draw(&self);
}

struct Button {
    label: String,
}

impl Draw for Button {
    fn draw(&self) {
        println!("Dessiner bouton: {}", self.label);
    }
}

struct TextField {
    placeholder: String,
}

impl Draw for TextField {
    fn draw(&self) {
        println!("Dessiner champ texte: {}", self.placeholder);
    }
}

// Collection de types différents avec trait commun
struct Screen {
    components: Vec<Box<dyn Draw>>,
}

impl Screen {
    fn run(&self) {
        for component in self.components.iter() {
            component.draw();
        }
    }
}

fn main() {
    let screen = Screen {
        components: vec![
            Box::new(Button { label: String::from("OK") }),
            Box::new(TextField { placeholder: String::from("Entrez texte...") }),
        ],
    };
    
    screen.run();
}
```

---

# Partie 7 : Programmation Concurrente - Introduction

## 7.1 Concepts Fondamentaux

La concurrence permet d'exécuter plusieurs parties d'un programme de façon indépendante. Le parallélisme permet d'exécuter ces parties simultanément.

### Problèmes Classiques de Concurrence

1. **Race conditions** : Accès non-synchronisé aux données
2. **Deadlocks** : Threads bloqués mutuellement
3. **Data races** : Accès concurrent à la mémoire partagée

### La Garantie de Rust

Rust garantit au moment de la compilation l'absence de data races grâce à son système d'ownership. Le slogan : "Fearless Concurrency" (Concurrence sans peur).

## 7.2 Modèles de Concurrence en Rust

1. **Threads** : Exécution parallèle avec mémoire partagée
2. **Message passing** : Communication via canaux
3. **Shared state** : État partagé protégé par Mutex/RwLock
4. **Async/Await** : Concurrence légère sans threads OS

---

# Partie 8 : Les Threads en Rust

## 8.1 Création de Threads

```rust
use std::thread;
use std::time::Duration;

fn main() {
    // Créer un nouveau thread
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("hi numéro {} depuis le thread créé!", i);
            thread::sleep(Duration::from_millis(1));
        }
    });
    
    // Code du thread principal
    for i in 1..5 {
        println!("hi numéro {} depuis le thread principal!", i);
        thread::sleep(Duration::from_millis(1));
    }
    
    // Attendre que le thread se termine
    handle.join().unwrap();
}
```

## 8.2 Move et Capture de Variables

```rust
use std::thread;

fn main() {
    let v = vec![1, 2, 3];
    
    // ERREUR sans move : le thread pourrait vivre plus longtemps que v
    // let handle = thread::spawn(|| {
    //     println!("Voici un vecteur: {:?}", v);
    // });
    
    // Avec move : transfert de propriété au thread
    let handle = thread::spawn(move || {
        println!("Voici un vecteur: {:?}", v);
    });
    
    // v n'est plus utilisable ici
    // drop(v);  // ERREUR !
    
    handle.join().unwrap();
}
```

## 8.3 Thread Builder

```rust
use std::thread;

fn main() {
    // Configuration avancée du thread
    let builder = thread::Builder::new()
        .name("mon-thread".to_string())
        .stack_size(32 * 1024);  // 32 KB
    
    let handle = builder.spawn(|| {
        println!("Thread: {:?}", thread::current().name());
    }).unwrap();
    
    handle.join().unwrap();
}
```

## 8.4 Thread-Local Storage

```rust
use std::cell::RefCell;
use std::thread;

// Variable thread-local
thread_local! {
    static COMPTEUR: RefCell<u32> = RefCell::new(0);
}

fn main() {
    COMPTEUR.with(|c| {
        *c.borrow_mut() += 1;
    });
    
    let handle = thread::spawn(|| {
        // Chaque thread a sa propre copie
        COMPTEUR.with(|c| {
            *c.borrow_mut() += 1;
            println!("Thread spawné: {}", c.borrow());
        });
    });
    
    handle.join().unwrap();
    
    COMPTEUR.with(|c| {
        println!("Thread principal: {}", c.borrow());
    });
}
```

## 8.5 Scoped Threads

```rust
use std::thread;

fn main() {
    let mut data = vec![1, 2, 3];
    
    // Scoped threads permettent d'emprunter des données locales
    thread::scope(|s| {
        s.spawn(|| {
            // Emprunt immuable
            println!("Longueur: {}", data.len());
        });
        
        s.spawn(|| {
            // Autre emprunt immuable
            for x in &data {
                println!("{x}");
            }
        });
    });
    
    // Les threads sont garantis de se terminer avant de sortir du scope
    data.push(4);  // OK, les threads sont terminés
    println!("{:?}", data);
}
```

## 8.6 Parallélisme avec rayon

```rust
use rayon::prelude::*;

fn main() {
    // Itération parallèle simple
    let v: Vec<i64> = (0..100_000).collect();
    
    let somme: i64 = v.par_iter().sum();
    println!("Somme: {}", somme);
    
    // Map parallèle
    let carres: Vec<i64> = v.par_iter()
        .map(|x| x * x)
        .collect();
    
    // Filter parallèle
    let pairs: Vec<&i64> = v.par_iter()
        .filter(|x| *x % 2 == 0)
        .collect();
    
    // Reduce parallèle
    let produit: i64 = (1..=10_i64)
        .into_par_iter()
        .reduce(|| 1, |a, b| a * b);
    
    println!("Produit: {}", produit);
}
```

---

# Partie 9 : Synchronisation et Mutex

## 9.1 Mutex<T>

Un Mutex (Mutual Exclusion) protège les données partagées en n'autorisant qu'un seul thread à y accéder.

```rust
use std::sync::Mutex;

fn main() {
    let m = Mutex::new(5);
    
    {
        // lock() bloque jusqu'à obtention du verrou
        // Retourne un MutexGuard (smart pointer)
        let mut num = m.lock().unwrap();
        *num = 6;
    }   // Le verrou est automatiquement libéré ici (Drop)
    
    println!("m = {:?}", m);
}
```

## 9.2 Arc<T> - Atomic Reference Counting

Pour partager un Mutex entre threads, on utilise Arc (Atomic Reference Counted).

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    // Arc permet le partage entre threads
    let compteur = Arc::new(Mutex::new(0));
    let mut handles = vec![];
    
    for _ in 0..10 {
        // Clone l'Arc (pas les données)
        let compteur = Arc::clone(&compteur);
        
        let handle = thread::spawn(move || {
            let mut num = compteur.lock().unwrap();
            *num += 1;
        });
        
        handles.push(handle);
    }
    
    for handle in handles {
        handle.join().unwrap();
    }
    
    println!("Résultat: {}", *compteur.lock().unwrap());
}
```

## 9.3 RwLock<T>

RwLock permet plusieurs lecteurs OU un seul écrivain.

```rust
use std::sync::{Arc, RwLock};
use std::thread;

fn main() {
    let donnees = Arc::new(RwLock::new(vec![1, 2, 3]));
    let mut handles = vec![];
    
    // Plusieurs lecteurs simultanés
    for i in 0..3 {
        let donnees = Arc::clone(&donnees);
        handles.push(thread::spawn(move || {
            let lecture = donnees.read().unwrap();
            println!("Lecteur {}: {:?}", i, *lecture);
        }));
    }
    
    // Un écrivain (bloque jusqu'à ce que tous les lecteurs aient fini)
    {
        let donnees = Arc::clone(&donnees);
        handles.push(thread::spawn(move || {
            let mut ecriture = donnees.write().unwrap();
            ecriture.push(4);
            println!("Écrivain: ajouté 4");
        }));
    }
    
    for handle in handles {
        handle.join().unwrap();
    }
    
    println!("Final: {:?}", *donnees.read().unwrap());
}
```

## 9.4 Éviter les Deadlocks

```rust
use std::sync::{Arc, Mutex};
use std::thread;

fn main() {
    let resource_a = Arc::new(Mutex::new(0));
    let resource_b = Arc::new(Mutex::new(0));
    
    // DANGER : Deadlock possible si l'ordre d'acquisition diffère
    
    // Solution 1: Toujours acquérir dans le même ordre
    let ra = Arc::clone(&resource_a);
    let rb = Arc::clone(&resource_b);
    
    let handle1 = thread::spawn(move || {
        let _a = ra.lock().unwrap();  // Toujours A d'abord
        let _b = rb.lock().unwrap();
        println!("Thread 1 terminé");
    });
    
    let ra = Arc::clone(&resource_a);
    let rb = Arc::clone(&resource_b);
    
    let handle2 = thread::spawn(move || {
        let _a = ra.lock().unwrap();  // Même ordre : A puis B
        let _b = rb.lock().unwrap();
        println!("Thread 2 terminé");
    });
    
    handle1.join().unwrap();
    handle2.join().unwrap();
}

// Solution 2: try_lock avec timeout
use std::time::Duration;

fn avec_try_lock() {
    let m = Mutex::new(5);
    
    match m.try_lock() {
        Ok(guard) => println!("Verrouillé: {}", *guard),
        Err(_) => println!("N'a pas pu obtenir le verrou"),
    }
}
```

## 9.5 Atomic Types

Pour les opérations simples, les types atomiques sont plus efficaces que Mutex.

```rust
use std::sync::atomic::{AtomicUsize, AtomicBool, Ordering};
use std::sync::Arc;
use std::thread;

fn main() {
    let compteur = Arc::new(AtomicUsize::new(0));
    let mut handles = vec![];
    
    for _ in 0..10 {
        let compteur = Arc::clone(&compteur);
        handles.push(thread::spawn(move || {
            for _ in 0..1000 {
                // Incrémentation atomique
                compteur.fetch_add(1, Ordering::SeqCst);
            }
        }));
    }
    
    for handle in handles {
        handle.join().unwrap();
    }
    
    println!("Compteur: {}", compteur.load(Ordering::SeqCst));
}

// Orderings (du plus permissif au plus strict):
// - Relaxed : Pas de garantie d'ordre
// - Acquire : Garanties pour les lectures
// - Release : Garanties pour les écritures
// - AcqRel  : Acquire + Release
// - SeqCst  : Séquentiellement cohérent (le plus sûr)

fn exemple_orderings() {
    let flag = AtomicBool::new(false);
    let data = AtomicUsize::new(0);
    
    // Thread producteur
    // data.store(42, Ordering::Relaxed);
    // flag.store(true, Ordering::Release);  // Garantit que data est visible
    
    // Thread consommateur
    // while !flag.load(Ordering::Acquire) {}  // Synchronise avec Release
    // let value = data.load(Ordering::Relaxed);  // Voit 42
}
```

## 9.6 Condition Variables

```rust
use std::sync::{Arc, Mutex, Condvar};
use std::thread;

fn main() {
    let pair = Arc::new((Mutex::new(false), Condvar::new()));
    let pair2 = Arc::clone(&pair);
    
    // Thread qui attend
    thread::spawn(move || {
        let (lock, cvar) = &*pair2;
        let mut started = lock.lock().unwrap();
        
        while !*started {
            // Attend la notification
            started = cvar.wait(started).unwrap();
        }
        
        println!("Notification reçue!");
    });
    
    // Thread principal qui notifie
    thread::sleep(std::time::Duration::from_secs(1));
    
    let (lock, cvar) = &*pair;
    let mut started = lock.lock().unwrap();
    *started = true;
    cvar.notify_one();
    
    println!("Notification envoyée");
}
```

## 9.7 Barrier

```rust
use std::sync::{Arc, Barrier};
use std::thread;

fn main() {
    let n = 5;
    let barrier = Arc::new(Barrier::new(n));
    let mut handles = vec![];
    
    for i in 0..n {
        let barrier = Arc::clone(&barrier);
        handles.push(thread::spawn(move || {
            println!("Thread {} avant la barrière", i);
            
            // Attend que tous les threads atteignent ce point
            barrier.wait();
            
            println!("Thread {} après la barrière", i);
        }));
    }
    
    for handle in handles {
        handle.join().unwrap();
    }
}
```

## 9.8 Once

```rust
use std::sync::Once;

static INIT: Once = Once::new();
static mut CONFIG: Option<String> = None;

fn get_config() -> &'static str {
    unsafe {
        INIT.call_once(|| {
            println!("Initialisation unique");
            CONFIG = Some(String::from("configuration chargée"));
        });
        CONFIG.as_ref().unwrap()
    }
}

fn main() {
    println!("{}", get_config());  // Initialise
    println!("{}", get_config());  // Réutilise
}

// Rust 1.70+: OnceLock (version safe)
use std::sync::OnceLock;

static CONFIG_SAFE: OnceLock<String> = OnceLock::new();

fn get_config_safe() -> &'static str {
    CONFIG_SAFE.get_or_init(|| {
        println!("Initialisation unique et safe");
        String::from("configuration")
    })
}
```

---

# Partie 10 : Canaux de Communication (Channels)

## 10.1 MPSC - Multiple Producer, Single Consumer

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    // Créer un canal
    let (tx, rx) = mpsc::channel();
    
    thread::spawn(move || {
        let val = String::from("salut");
        tx.send(val).unwrap();
        // val n'est plus utilisable (moved)
    });
    
    // recv() bloque jusqu'à réception
    let recu = rx.recv().unwrap();
    println!("Reçu: {}", recu);
}
```

## 10.2 Envoi de Plusieurs Valeurs

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();
    
    thread::spawn(move || {
        let vals = vec![
            String::from("salut"),
            String::from("depuis"),
            String::from("le"),
            String::from("thread"),
        ];
        
        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(Duration::from_millis(200));
        }
    });
    
    // Utiliser rx comme itérateur
    for recu in rx {
        println!("Reçu: {}", recu);
    }
}
```

## 10.3 Multiple Producers

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();
    
    // Clone du transmitter pour plusieurs producteurs
    let tx1 = tx.clone();
    thread::spawn(move || {
        for i in 0..5 {
            tx1.send(format!("Thread 1: {}", i)).unwrap();
            thread::sleep(Duration::from_millis(100));
        }
    });
    
    thread::spawn(move || {
        for i in 0..5 {
            tx.send(format!("Thread 2: {}", i)).unwrap();
            thread::sleep(Duration::from_millis(150));
        }
    });
    
    for recu in rx {
        println!("{}", recu);
    }
}
```

## 10.4 Canaux Synchrones

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    // sync_channel avec buffer de taille 0 (rendez-vous)
    let (tx, rx) = mpsc::sync_channel(0);
    
    thread::spawn(move || {
        println!("Avant envoi");
        tx.send(1).unwrap();  // Bloque jusqu'à réception
        println!("Après envoi");
    });
    
    thread::sleep(std::time::Duration::from_secs(1));
    println!("Réception: {}", rx.recv().unwrap());
    
    // sync_channel avec buffer
    let (tx, rx) = mpsc::sync_channel(3);
    
    tx.send(1).unwrap();  // OK
    tx.send(2).unwrap();  // OK
    tx.send(3).unwrap();  // OK
    // tx.send(4).unwrap();  // Bloquerait (buffer plein)
}
```

## 10.5 try_recv et Timeout

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();
    
    thread::spawn(move || {
        thread::sleep(Duration::from_secs(1));
        tx.send("message").unwrap();
    });
    
    // try_recv : non-bloquant
    loop {
        match rx.try_recv() {
            Ok(msg) => {
                println!("Reçu: {}", msg);
                break;
            }
            Err(mpsc::TryRecvError::Empty) => {
                println!("Pas encore de message...");
                thread::sleep(Duration::from_millis(200));
            }
            Err(mpsc::TryRecvError::Disconnected) => {
                println!("Channel fermé");
                break;
            }
        }
    }
    
    // recv_timeout
    let (tx, rx) = mpsc::channel::<i32>();
    
    match rx.recv_timeout(Duration::from_secs(1)) {
        Ok(msg) => println!("Reçu: {}", msg),
        Err(mpsc::RecvTimeoutError::Timeout) => println!("Timeout!"),
        Err(mpsc::RecvTimeoutError::Disconnected) => println!("Déconnecté"),
    }
}
```

## 10.6 Crossbeam Channels (Plus Puissants)

```rust
use crossbeam_channel::{bounded, unbounded, select};
use std::thread;
use std::time::Duration;

fn main() {
    // Canal non-borné
    let (s, r) = unbounded();
    s.send("hello").unwrap();
    println!("{}", r.recv().unwrap());
    
    // Canal borné
    let (s, r) = bounded(5);
    s.send(1).unwrap();
    
    // Multiple Producer Multiple Consumer (MPMC)
    let (s, r) = unbounded();
    
    // Plusieurs consommateurs
    for i in 0..3 {
        let r = r.clone();
        thread::spawn(move || {
            for msg in r {
                println!("Consommateur {}: {}", i, msg);
            }
        });
    }
    
    // Plusieurs producteurs
    for i in 0..10 {
        s.send(i).unwrap();
    }
    drop(s);  // Ferme le canal
}

// select! pour attendre sur plusieurs canaux
fn avec_select() {
    let (s1, r1) = bounded(1);
    let (s2, r2) = bounded(1);
    
    thread::spawn(move || {
        thread::sleep(Duration::from_millis(100));
        s1.send("from channel 1").unwrap();
    });
    
    thread::spawn(move || {
        thread::sleep(Duration::from_millis(50));
        s2.send("from channel 2").unwrap();
    });
    
    // Attend le premier message disponible
    select! {
        recv(r1) -> msg => println!("r1: {:?}", msg),
        recv(r2) -> msg => println!("r2: {:?}", msg),
    }
}
```

---

# Partie 11 : Programmation Asynchrone

## 11.1 Introduction à Async/Await

La programmation asynchrone permet la concurrence sans créer de threads OS.

```rust
// Fonction async retourne un Future
async fn hello() -> String {
    "Hello, async world!".to_string()
}

// await suspend l'exécution jusqu'à completion
async fn main_async() {
    let message = hello().await;
    println!("{}", message);
}
```

## 11.2 Runtime Tokio

Tokio est le runtime async le plus populaire.

```toml
# Cargo.toml
[dependencies]
tokio = { version = "1", features = ["full"] }
```

```rust
use tokio::time::{sleep, Duration};

#[tokio::main]
async fn main() {
    println!("Début");
    
    sleep(Duration::from_secs(1)).await;
    
    println!("Après 1 seconde");
}

// Plusieurs tâches concurrentes
#[tokio::main]
async fn main() {
    let task1 = tokio::spawn(async {
        sleep(Duration::from_secs(1)).await;
        println!("Tâche 1 terminée");
        "résultat 1"
    });
    
    let task2 = tokio::spawn(async {
        sleep(Duration::from_millis(500)).await;
        println!("Tâche 2 terminée");
        "résultat 2"
    });
    
    // Attend les deux tâches
    let (r1, r2) = tokio::join!(task1, task2);
    println!("Résultats: {:?}, {:?}", r1, r2);
}
```

## 11.3 Futures et Polling

```rust
use std::future::Future;
use std::pin::Pin;
use std::task::{Context, Poll};

// Implémentation manuelle d'un Future
struct MonFuture {
    compteur: u32,
}

impl Future for MonFuture {
    type Output = u32;
    
    fn poll(mut self: Pin<&mut Self>, cx: &mut Context<'_>) -> Poll<Self::Output> {
        self.compteur += 1;
        
        if self.compteur >= 3 {
            Poll::Ready(self.compteur)
        } else {
            // Re-schedule pour être pollé à nouveau
            cx.waker().wake_by_ref();
            Poll::Pending
        }
    }
}

#[tokio::main]
async fn main() {
    let future = MonFuture { compteur: 0 };
    let resultat = future.await;
    println!("Résultat: {}", resultat);
}
```

## 11.4 Async I/O avec Tokio

```rust
use tokio::fs::File;
use tokio::io::{self, AsyncReadExt, AsyncWriteExt};

#[tokio::main]
async fn main() -> io::Result<()> {
    // Écriture async
    let mut file = File::create("test.txt").await?;
    file.write_all(b"Hello, async file!").await?;
    
    // Lecture async
    let mut file = File::open("test.txt").await?;
    let mut contents = String::new();
    file.read_to_string(&mut contents).await?;
    println!("Contenu: {}", contents);
    
    Ok(())
}
```

## 11.5 Channels Async

```rust
use tokio::sync::mpsc;

#[tokio::main]
async fn main() {
    // Canal async borné
    let (tx, mut rx) = mpsc::channel(32);
    
    tokio::spawn(async move {
        for i in 0..10 {
            tx.send(i).await.unwrap();
        }
    });
    
    while let Some(value) = rx.recv().await {
        println!("Reçu: {}", value);
    }
}

// oneshot : une seule valeur
use tokio::sync::oneshot;

#[tokio::main]
async fn main() {
    let (tx, rx) = oneshot::channel();
    
    tokio::spawn(async move {
        tx.send("résultat").unwrap();
    });
    
    match rx.await {
        Ok(value) => println!("Reçu: {}", value),
        Err(_) => println!("Expéditeur abandonné"),
    }
}

// broadcast : plusieurs récepteurs
use tokio::sync::broadcast;

#[tokio::main]
async fn main() {
    let (tx, mut rx1) = broadcast::channel(16);
    let mut rx2 = tx.subscribe();
    
    tokio::spawn(async move {
        while let Ok(value) = rx1.recv().await {
            println!("rx1: {}", value);
        }
    });
    
    tokio::spawn(async move {
        while let Ok(value) = rx2.recv().await {
            println!("rx2: {}", value);
        }
    });
    
    tx.send(1).unwrap();
    tx.send(2).unwrap();
}

// watch : dernière valeur seulement
use tokio::sync::watch;

#[tokio::main]
async fn main() {
    let (tx, mut rx) = watch::channel("initial");
    
    tokio::spawn(async move {
        loop {
            rx.changed().await.unwrap();
            println!("Valeur changée: {}", *rx.borrow());
        }
    });
    
    tx.send("mise à jour 1").unwrap();
    tx.send("mise à jour 2").unwrap();
}
```

## 11.6 Mutex Async

```rust
use tokio::sync::Mutex;
use std::sync::Arc;

#[tokio::main]
async fn main() {
    let data = Arc::new(Mutex::new(0));
    
    let mut handles = vec![];
    
    for _ in 0..10 {
        let data = Arc::clone(&data);
        handles.push(tokio::spawn(async move {
            let mut lock = data.lock().await;
            *lock += 1;
        }));
    }
    
    for handle in handles {
        handle.await.unwrap();
    }
    
    println!("Résultat: {}", *data.lock().await);
}
```

## 11.7 Select et Race

```rust
use tokio::time::{sleep, Duration};
use tokio::select;

#[tokio::main]
async fn main() {
    let task1 = async {
        sleep(Duration::from_secs(1)).await;
        "task1"
    };
    
    let task2 = async {
        sleep(Duration::from_millis(500)).await;
        "task2"
    };
    
    // Attend le premier terminé
    tokio::select! {
        result = task1 => println!("Task1 first: {}", result),
        result = task2 => println!("Task2 first: {}", result),
    }
}

// Avec timeout
use tokio::time::timeout;

#[tokio::main]
async fn main() {
    let slow_operation = async {
        sleep(Duration::from_secs(10)).await;
        "terminé"
    };
    
    match timeout(Duration::from_secs(1), slow_operation).await {
        Ok(result) => println!("Résultat: {}", result),
        Err(_) => println!("Timeout!"),
    }
}
```

## 11.8 Streams

```rust
use tokio_stream::{self as stream, StreamExt};

#[tokio::main]
async fn main() {
    // Créer un stream
    let mut stream = stream::iter(vec![1, 2, 3, 4, 5]);
    
    // Consommer le stream
    while let Some(value) = stream.next().await {
        println!("Valeur: {}", value);
    }
    
    // Transformation
    let stream = stream::iter(vec![1, 2, 3, 4, 5]);
    let doubled: Vec<i32> = stream.map(|x| x * 2).collect().await;
    println!("{:?}", doubled);
    
    // Filter
    let stream = stream::iter(vec![1, 2, 3, 4, 5, 6]);
    let evens: Vec<i32> = stream.filter(|x| x % 2 == 0).collect().await;
    println!("Pairs: {:?}", evens);
}
```

## 11.9 Async avec différents runtimes

```rust
// async-std
use async_std::task;
use async_std::prelude::*;

#[async_std::main]
async fn main() {
    task::sleep(std::time::Duration::from_secs(1)).await;
    println!("Hello from async-std!");
}

// smol (léger)
fn main() {
    smol::block_on(async {
        println!("Hello from smol!");
    });
}
```

---

# Partie 12 : Patterns Avancés de Concurrence

## 12.1 Actor Pattern

```rust
use tokio::sync::mpsc;

// Message enum
enum Message {
    Increment,
    Decrement,
    GetValue(tokio::sync::oneshot::Sender<i32>),
}

// Actor
struct CounterActor {
    value: i32,
    receiver: mpsc::Receiver<Message>,
}

impl CounterActor {
    fn new(receiver: mpsc::Receiver<Message>) -> Self {
        CounterActor { value: 0, receiver }
    }
    
    async fn run(&mut self) {
        while let Some(msg) = self.receiver.recv().await {
            match msg {
                Message::Increment => self.value += 1,
                Message::Decrement => self.value -= 1,
                Message::GetValue(respond_to) => {
                    let _ = respond_to.send(self.value);
                }
            }
        }
    }
}

// Handle pour interagir avec l'actor
#[derive(Clone)]
struct CounterHandle {
    sender: mpsc::Sender<Message>,
}

impl CounterHandle {
    fn new() -> Self {
        let (sender, receiver) = mpsc::channel(32);
        let mut actor = CounterActor::new(receiver);
        
        tokio::spawn(async move {
            actor.run().await;
        });
        
        CounterHandle { sender }
    }
    
    async fn increment(&self) {
        self.sender.send(Message::Increment).await.unwrap();
    }
    
    async fn decrement(&self) {
        self.sender.send(Message::Decrement).await.unwrap();
    }
    
    async fn get_value(&self) -> i32 {
        let (send, recv) = tokio::sync::oneshot::channel();
        self.sender.send(Message::GetValue(send)).await.unwrap();
        recv.await.unwrap()
    }
}

#[tokio::main]
async fn main() {
    let handle = CounterHandle::new();
    
    handle.increment().await;
    handle.increment().await;
    handle.decrement().await;
    
    let value = handle.get_value().await;
    println!("Valeur: {}", value);
}
```

## 12.2 Producer-Consumer avec Buffer

```rust
use std::sync::{Arc, Mutex, Condvar};
use std::collections::VecDeque;
use std::thread;

struct BoundedBuffer<T> {
    buffer: Mutex<VecDeque<T>>,
    not_empty: Condvar,
    not_full: Condvar,
    capacity: usize,
}

impl<T> BoundedBuffer<T> {
    fn new(capacity: usize) -> Self {
        BoundedBuffer {
            buffer: Mutex::new(VecDeque::new()),
            not_empty: Condvar::new(),
            not_full: Condvar::new(),
            capacity,
        }
    }
    
    fn put(&self, item: T) {
        let mut buffer = self.buffer.lock().unwrap();
        
        while buffer.len() >= self.capacity {
            buffer = self.not_full.wait(buffer).unwrap();
        }
        
        buffer.push_back(item);
        self.not_empty.notify_one();
    }
    
    fn take(&self) -> T {
        let mut buffer = self.buffer.lock().unwrap();
        
        while buffer.is_empty() {
            buffer = self.not_empty.wait(buffer).unwrap();
        }
        
        let item = buffer.pop_front().unwrap();
        self.not_full.notify_one();
        item
    }
}

fn main() {
    let buffer = Arc::new(BoundedBuffer::new(5));
    let mut handles = vec![];
    
    // Producteurs
    for i in 0..3 {
        let buffer = Arc::clone(&buffer);
        handles.push(thread::spawn(move || {
            for j in 0..5 {
                buffer.put(format!("P{}:{}", i, j));
                println!("Produit P{}:{}", i, j);
            }
        }));
    }
    
    // Consommateurs
    for i in 0..2 {
        let buffer = Arc::clone(&buffer);
        handles.push(thread::spawn(move || {
            for _ in 0..7 {
                let item = buffer.take();
                println!("Consommé par C{}: {}", i, item);
            }
        }));
    }
    
    for handle in handles {
        handle.join().unwrap();
    }
}
```

## 12.3 Work Stealing Queue

```rust
use crossbeam_deque::{Injector, Stealer, Worker};
use std::thread;
use std::sync::Arc;

fn main() {
    // File globale
    let injector: Arc<Injector<i32>> = Arc::new(Injector::new());
    
    // Files de travail locales
    let workers: Vec<Worker<i32>> = (0..4).map(|_| Worker::new_fifo()).collect();
    let stealers: Vec<Stealer<i32>> = workers.iter().map(|w| w.stealer()).collect();
    
    // Ajouter du travail
    for i in 0..100 {
        injector.push(i);
    }
    
    let mut handles = vec![];
    
    for (id, worker) in workers.into_iter().enumerate() {
        let injector = Arc::clone(&injector);
        let stealers = stealers.clone();
        
        handles.push(thread::spawn(move || {
            let mut processed = 0;
            
            loop {
                // 1. Essayer la file locale
                if let Some(task) = worker.pop() {
                    println!("Worker {} traite {} (local)", id, task);
                    processed += 1;
                    continue;
                }
                
                // 2. Essayer la file globale
                if let crossbeam_deque::Steal::Success(task) = injector.steal() {
                    println!("Worker {} traite {} (global)", id, task);
                    processed += 1;
                    continue;
                }
                
                // 3. Voler à un autre worker
                let mut stolen = false;
                for (i, stealer) in stealers.iter().enumerate() {
                    if i != id {
                        if let crossbeam_deque::Steal::Success(task) = stealer.steal() {
                            println!("Worker {} vole {} de {}", id, task, i);
                            processed += 1;
                            stolen = true;
                            break;
                        }
                    }
                }
                
                if !stolen {
                    break;  // Plus de travail
                }
            }
            
            println!("Worker {} terminé, traité: {}", id, processed);
        }));
    }
    
    for handle in handles {
        handle.join().unwrap();
    }
}
```

## 12.4 Read-Write Lock Pattern

```rust
use parking_lot::RwLock;
use std::sync::Arc;
use std::thread;

struct Cache<K, V> {
    data: RwLock<std::collections::HashMap<K, V>>,
}

impl<K: std::hash::Hash + Eq + Clone, V: Clone> Cache<K, V> {
    fn new() -> Self {
        Cache {
            data: RwLock::new(std::collections::HashMap::new()),
        }
    }
    
    fn get(&self, key: &K) -> Option<V> {
        self.data.read().get(key).cloned()
    }
    
    fn set(&self, key: K, value: V) {
        self.data.write().insert(key, value);
    }
    
    fn get_or_compute<F>(&self, key: K, compute: F) -> V
    where
        F: FnOnce() -> V,
    {
        // Essayer d'abord en lecture
        if let Some(value) = self.data.read().get(&key) {
            return value.clone();
        }
        
        // Sinon, calculer et écrire
        let mut write_guard = self.data.write();
        
        // Double-check (quelqu'un d'autre a pu insérer)
        if let Some(value) = write_guard.get(&key) {
            return value.clone();
        }
        
        let value = compute();
        write_guard.insert(key, value.clone());
        value
    }
}

fn main() {
    let cache = Arc::new(Cache::<String, i32>::new());
    let mut handles = vec![];
    
    for i in 0..10 {
        let cache = Arc::clone(&cache);
        handles.push(thread::spawn(move || {
            let key = format!("key{}", i % 3);
            let value = cache.get_or_compute(key.clone(), || {
                println!("Computing for {}", key);
                i * 10
            });
            println!("Thread {} got: {}", i, value);
        }));
    }
    
    for handle in handles {
        handle.join().unwrap();
    }
}
```

## 12.5 Lock-Free Stack

```rust
use std::sync::atomic::{AtomicPtr, Ordering};
use std::ptr;

struct Node<T> {
    data: T,
    next: *mut Node<T>,
}

pub struct LockFreeStack<T> {
    head: AtomicPtr<Node<T>>,
}

impl<T> LockFreeStack<T> {
    pub fn new() -> Self {
        LockFreeStack {
            head: AtomicPtr::new(ptr::null_mut()),
        }
    }
    
    pub fn push(&self, data: T) {
        let new_node = Box::into_raw(Box::new(Node {
            data,
            next: ptr::null_mut(),
        }));
        
        loop {
            let old_head = self.head.load(Ordering::Acquire);
            unsafe { (*new_node).next = old_head; }
            
            if self.head.compare_exchange_weak(
                old_head,
                new_node,
                Ordering::Release,
                Ordering::Relaxed,
            ).is_ok() {
                break;
            }
        }
    }
    
    pub fn pop(&self) -> Option<T> {
        loop {
            let old_head = self.head.load(Ordering::Acquire);
            
            if old_head.is_null() {
                return None;
            }
            
            let next = unsafe { (*old_head).next };
            
            if self.head.compare_exchange_weak(
                old_head,
                next,
                Ordering::Release,
                Ordering::Relaxed,
            ).is_ok() {
                let node = unsafe { Box::from_raw(old_head) };
                return Some(node.data);
            }
        }
    }
}

impl<T> Drop for LockFreeStack<T> {
    fn drop(&mut self) {
        while self.pop().is_some() {}
    }
}

unsafe impl<T: Send> Send for LockFreeStack<T> {}
unsafe impl<T: Send> Sync for LockFreeStack<T> {}

fn main() {
    use std::sync::Arc;
    use std::thread;
    
    let stack = Arc::new(LockFreeStack::new());
    let mut handles = vec![];
    
    for i in 0..10 {
        let stack = Arc::clone(&stack);
        handles.push(thread::spawn(move || {
            stack.push(i);
        }));
    }
    
    for handle in handles {
        handle.join().unwrap();
    }
    
    while let Some(value) = stack.pop() {
        println!("Popped: {}", value);
    }
}
```

## 12.6 Thread Pool Custom

```rust
use std::sync::{mpsc, Arc, Mutex};
use std::thread;

type Job = Box<dyn FnOnce() + Send + 'static>;

struct ThreadPool {
    workers: Vec<Worker>,
    sender: Option<mpsc::Sender<Job>>,
}

struct Worker {
    id: usize,
    thread: Option<thread::JoinHandle<()>>,
}

impl ThreadPool {
    fn new(size: usize) -> ThreadPool {
        assert!(size > 0);
        
        let (sender, receiver) = mpsc::channel();
        let receiver = Arc::new(Mutex::new(receiver));
        
        let mut workers = Vec::with_capacity(size);
        
        for id in 0..size {
            workers.push(Worker::new(id, Arc::clone(&receiver)));
        }
        
        ThreadPool {
            workers,
            sender: Some(sender),
        }
    }
    
    fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        let job = Box::new(f);
        self.sender.as_ref().unwrap().send(job).unwrap();
    }
}

impl Drop for ThreadPool {
    fn drop(&mut self) {
        drop(self.sender.take());
        
        for worker in &mut self.workers {
            println!("Arrêt du worker {}", worker.id);
            
            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        let thread = thread::spawn(move || loop {
            let message = receiver.lock().unwrap().recv();
            
            match message {
                Ok(job) => {
                    println!("Worker {} exécute une tâche", id);
                    job();
                }
                Err(_) => {
                    println!("Worker {} déconnecté", id);
                    break;
                }
            }
        });
        
        Worker {
            id,
            thread: Some(thread),
        }
    }
}

fn main() {
    let pool = ThreadPool::new(4);
    
    for i in 0..8 {
        pool.execute(move || {
            println!("Tâche {} commence", i);
            thread::sleep(std::time::Duration::from_millis(250));
            println!("Tâche {} terminée", i);
        });
    }
    
    thread::sleep(std::time::Duration::from_secs(2));
    println!("Arrêt du pool");
}
```

---

# Partie 13 : Projets Pratiques

## 13.1 Serveur Web Concurrent

```rust
use std::io::{Read, Write};
use std::net::{TcpListener, TcpStream};
use std::sync::Arc;
use std::thread;

fn handle_client(mut stream: TcpStream) {
    let mut buffer = [0; 1024];
    stream.read(&mut buffer).unwrap();
    
    let response = "HTTP/1.1 200 OK\r\n\
        Content-Type: text/html\r\n\
        \r\n\
        <html><body><h1>Hello from Rust!</h1></body></html>";
    
    stream.write_all(response.as_bytes()).unwrap();
    stream.flush().unwrap();
}

fn main() {
    let listener = TcpListener::bind("127.0.0.1:8080").unwrap();
    println!("Serveur démarré sur http://127.0.0.1:8080");
    
    // Version avec thread pool
    let pool = ThreadPool::new(4);
    
    for stream in listener.incoming() {
        let stream = stream.unwrap();
        pool.execute(|| {
            handle_client(stream);
        });
    }
}

// Version async avec Tokio
use tokio::io::{AsyncReadExt, AsyncWriteExt};
use tokio::net::TcpListener;

#[tokio::main]
async fn main() {
    let listener = TcpListener::bind("127.0.0.1:8080").await.unwrap();
    println!("Serveur async sur http://127.0.0.1:8080");
    
    loop {
        let (mut socket, _) = listener.accept().await.unwrap();
        
        tokio::spawn(async move {
            let mut buffer = [0; 1024];
            socket.read(&mut buffer).await.unwrap();
            
            let response = "HTTP/1.1 200 OK\r\n\
                Content-Type: text/html\r\n\
                \r\n\
                <html><body><h1>Hello from async Rust!</h1></body></html>";
            
            socket.write_all(response.as_bytes()).await.unwrap();
        });
    }
}
```

## 13.2 Crawler Web Concurrent

```rust
use reqwest;
use scraper::{Html, Selector};
use std::collections::HashSet;
use std::sync::{Arc, Mutex};
use tokio::sync::mpsc;

#[derive(Clone)]
struct Crawler {
    visited: Arc<Mutex<HashSet<String>>>,
    max_depth: usize,
}

impl Crawler {
    fn new(max_depth: usize) -> Self {
        Crawler {
            visited: Arc::new(Mutex::new(HashSet::new())),
            max_depth,
        }
    }
    
    async fn crawl(&self, url: String, depth: usize) -> Vec<String> {
        if depth > self.max_depth {
            return vec![];
        }
        
        // Vérifier si déjà visité
        {
            let mut visited = self.visited.lock().unwrap();
            if visited.contains(&url) {
                return vec![];
            }
            visited.insert(url.clone());
        }
        
        println!("Crawling (depth {}): {}", depth, url);
        
        // Télécharger la page
        let response = match reqwest::get(&url).await {
            Ok(r) => r,
            Err(_) => return vec![],
        };
        
        let body = match response.text().await {
            Ok(b) => b,
            Err(_) => return vec![],
        };
        
        // Extraire les liens
        let document = Html::parse_document(&body);
        let selector = Selector::parse("a[href]").unwrap();
        
        let links: Vec<String> = document
            .select(&selector)
            .filter_map(|el| el.value().attr("href"))
            .filter(|href| href.starts_with("http"))
            .map(String::from)
            .collect();
        
        // Crawler les liens en parallèle
        let mut handles = vec![];
        
        for link in links.iter().take(5) {
            let crawler = self.clone();
            let link = link.clone();
            
            handles.push(tokio::spawn(async move {
                crawler.crawl(link, depth + 1).await
            }));
        }
        
        let mut all_links = links;
        
        for handle in handles {
            if let Ok(child_links) = handle.await {
                all_links.extend(child_links);
            }
        }
        
        all_links
    }
}

#[tokio::main]
async fn main() {
    let crawler = Crawler::new(2);
    let links = crawler.crawl("https://example.com".to_string(), 0).await;
    
    println!("\nTrouvé {} liens", links.len());
}
```

## 13.3 Pipeline de Traitement de Données

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    // Pipeline : Source -> Transform -> Filter -> Sink
    
    let (source_tx, transform_rx) = mpsc::channel();
    let (transform_tx, filter_rx) = mpsc::channel();
    let (filter_tx, sink_rx) = mpsc::channel();
    
    // Source : génère des données
    let source = thread::spawn(move || {
        for i in 0..100 {
            source_tx.send(i).unwrap();
        }
    });
    
    // Transform : double les valeurs
    let transform = thread::spawn(move || {
        for value in transform_rx {
            transform_tx.send(value * 2).unwrap();
        }
    });
    
    // Filter : garde seulement les multiples de 3
    let filter = thread::spawn(move || {
        for value in filter_rx {
            if value % 3 == 0 {
                filter_tx.send(value).unwrap();
            }
        }
    });
    
    // Sink : collecte les résultats
    let sink = thread::spawn(move || {
        let mut results = vec![];
        for value in sink_rx {
            results.push(value);
        }
        results
    });
    
    source.join().unwrap();
    transform.join().unwrap();
    filter.join().unwrap();
    let results = sink.join().unwrap();
    
    println!("Résultats: {:?}", results);
}

// Version async avec streams
use tokio_stream::{self as stream, StreamExt};

#[tokio::main]
async fn main() {
    let results: Vec<i32> = stream::iter(0..100)
        .map(|x| x * 2)              // Transform
        .filter(|x| x % 3 == 0)       // Filter
        .collect()
        .await;
    
    println!("Résultats async: {:?}", results);
}
```

## 13.4 Rate Limiter Concurrent

```rust
use std::sync::Arc;
use std::time::{Duration, Instant};
use tokio::sync::Mutex;
use tokio::time::sleep;

struct RateLimiter {
    tokens: Mutex<f64>,
    max_tokens: f64,
    refill_rate: f64,  // tokens par seconde
    last_refill: Mutex<Instant>,
}

impl RateLimiter {
    fn new(max_tokens: f64, refill_rate: f64) -> Self {
        RateLimiter {
            tokens: Mutex::new(max_tokens),
            max_tokens,
            refill_rate,
            last_refill: Mutex::new(Instant::now()),
        }
    }
    
    async fn acquire(&self) -> bool {
        let mut tokens = self.tokens.lock().await;
        let mut last_refill = self.last_refill.lock().await;
        
        // Refill tokens
        let now = Instant::now();
        let elapsed = now.duration_since(*last_refill).as_secs_f64();
        *tokens = (*tokens + elapsed * self.refill_rate).min(self.max_tokens);
        *last_refill = now;
        
        if *tokens >= 1.0 {
            *tokens -= 1.0;
            true
        } else {
            false
        }
    }
    
    async fn acquire_blocking(&self) {
        loop {
            if self.acquire().await {
                return;
            }
            sleep(Duration::from_millis(10)).await;
        }
    }
}

#[tokio::main]
async fn main() {
    let limiter = Arc::new(RateLimiter::new(5.0, 2.0));  // 5 tokens, 2/sec
    
    let mut handles = vec![];
    
    for i in 0..20 {
        let limiter = Arc::clone(&limiter);
        handles.push(tokio::spawn(async move {
            limiter.acquire_blocking().await;
            println!("Requête {} passée à {:?}", i, Instant::now());
        }));
    }
    
    for handle in handles {
        handle.await.unwrap();
    }
}
```

## 13.5 Cache Concurrent avec Expiration

```rust
use std::collections::HashMap;
use std::sync::Arc;
use std::time::{Duration, Instant};
use tokio::sync::RwLock;

struct CacheEntry<V> {
    value: V,
    expires_at: Instant,
}

struct Cache<K, V> {
    data: RwLock<HashMap<K, CacheEntry<V>>>,
    default_ttl: Duration,
}

impl<K: std::hash::Hash + Eq + Clone, V: Clone> Cache<K, V> {
    fn new(default_ttl: Duration) -> Self {
        Cache {
            data: RwLock::new(HashMap::new()),
            default_ttl,
        }
    }
    
    async fn get(&self, key: &K) -> Option<V> {
        let data = self.data.read().await;
        
        data.get(key).and_then(|entry| {
            if entry.expires_at > Instant::now() {
                Some(entry.value.clone())
            } else {
                None
            }
        })
    }
    
    async fn set(&self, key: K, value: V) {
        self.set_with_ttl(key, value, self.default_ttl).await;
    }
    
    async fn set_with_ttl(&self, key: K, value: V, ttl: Duration) {
        let mut data = self.data.write().await;
        
        data.insert(key, CacheEntry {
            value,
            expires_at: Instant::now() + ttl,
        });
    }
    
    async fn cleanup(&self) {
        let mut data = self.data.write().await;
        let now = Instant::now();
        
        data.retain(|_, entry| entry.expires_at > now);
    }
}

#[tokio::main]
async fn main() {
    let cache = Arc::new(Cache::<String, i32>::new(Duration::from_secs(5)));
    
    // Set
    cache.set("key1".to_string(), 42).await;
    
    // Get
    if let Some(value) = cache.get(&"key1".to_string()).await {
        println!("Trouvé: {}", value);
    }
    
    // Spawn cleanup task
    let cache_clone = Arc::clone(&cache);
    tokio::spawn(async move {
        loop {
            tokio::time::sleep(Duration::from_secs(60)).await;
            cache_clone.cleanup().await;
        }
    });
}
```

---

# Annexes

## A. Commandes Cargo Utiles

```bash
# Création et build
cargo new projet          # Nouveau projet
cargo build               # Compiler (debug)
cargo build --release     # Compiler (release)
cargo run                 # Compiler et exécuter
cargo check               # Vérifier sans compiler

# Tests
cargo test                # Lancer les tests
cargo test nom_test       # Test spécifique
cargo test -- --nocapture # Voir les prints

# Documentation
cargo doc                 # Générer la doc
cargo doc --open          # Ouvrir dans le navigateur

# Dépendances
cargo add serde           # Ajouter une dépendance
cargo update              # Mettre à jour
cargo tree                # Arbre des dépendances

# Analyse
cargo clippy              # Linter
cargo fmt                 # Formatter
cargo audit               # Audit de sécurité
```

## B. Crates Essentielles pour la Concurrence

```toml
[dependencies]
# Runtime async
tokio = { version = "1", features = ["full"] }
async-std = "1"

# Canaux et synchronisation
crossbeam = "0.8"
flume = "0.11"
parking_lot = "0.12"

# Parallélisme
rayon = "1.10"

# Async utilities
futures = "0.3"
tokio-stream = "0.1"

# HTTP async
reqwest = { version = "0.12", features = ["json"] }
hyper = { version = "1", features = ["full"] }

# Serialization
serde = { version = "1", features = ["derive"] }
serde_json = "1"
```

## C. Ressources pour Approfondir

1. **The Rust Programming Language** (gratuit) : https://doc.rust-lang.org/book/
2. **Rust by Example** : https://doc.rust-lang.org/rust-by-example/
3. **Rustonomicon** (unsafe Rust) : https://doc.rust-lang.org/nomicon/
4. **Async Book** : https://rust-lang.github.io/async-book/
5. **Tokio Tutorial** : https://tokio.rs/tokio/tutorial

---

*Ce cours couvre les concepts essentiels de Rust et de la programmation concurrente. La pratique régulière et la lecture du code source de projets open source sont les meilleures façons de progresser.*
