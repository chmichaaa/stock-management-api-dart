# 🚀 **Gestion de Stock - API REST + Client Dart**

Ce projet implémente une **API REST** en **Express.js** permettant de gérer des produits et des commandes, ainsi qu'un **client Dart** qui interagit avec cette API pour récupérer et ajouter des produits et des commandes.

## 🎯 **Objectifs du Projet**

L'objectif de ce projet est de créer une solution permettant :

1. **Backend (API REST en Express.js)** :
   - Gérer les produits (ajout et récupération).
   - Gérer les commandes (ajout et récupération).

2. **Frontend (Client Dart)** :
   - Afficher la liste des produits et des commandes récupérées depuis l'API.
   - Permettre l'ajout de nouveaux produits et commandes.

## ⚙️ **Fonctionnalités**

### **API REST (Express.js)**
L'API fournit les fonctionnalités suivantes :

- **GET /produits** : Récupère la liste de tous les produits disponibles.
- **POST /produits** : Permet d'ajouter un nouveau produit.
- **GET /commandes** : Récupère la liste de toutes les commandes.
- **POST /commandes** : Permet de créer une nouvelle commande.

Les données sont stockées dans un fichier `db.json` et chaque modification est immédiatement sauvegardée.

### **Client Dart (Frontend)**
Le client Dart offre les fonctionnalités suivantes :

- **🛒 Afficher les produits** : Effectuer une requête GET pour récupérer et afficher les produits.
- **➕ Ajouter un produit** : Utiliser une requête POST pour ajouter un produit à la liste.
- **📦 Afficher les commandes** : Effectuer une requête GET pour récupérer et afficher les commandes.
- **➕ Ajouter une commande** : Utiliser une requête POST pour ajouter une commande.

## 🔧 **Prérequis**

Avant de commencer, assurez-vous d'avoir installé les outils suivants :

- [Node.js](https://nodejs.org/en/) pour le serveur backend.
- [Dart SDK](https://dart.dev/get-dart) pour le client Dart.

## 💻 **Installation**

### 1. **Cloner le dépôt**

Clonez le projet depuis GitHub :

``bash
git clone https://github.com/chemin-de-votre-repo/gestion-stock-dart.git
cd gestion-stock-dart

2. Installer les dépendances
Backend - API REST (Express.js)

Accédez au dossier du serveur API :

cd serveur_api

Installez les dépendances avec npm :

npm install

Démarrer le serveur

Lancez le serveur Express.js :

node server.js

Le serveur sera accessible à l'URL suivante : http://localhost:3000.
Frontend - Client Dart

Accédez au dossier de l'application Flutter :

cd ../application_mobile

Installez les dépendances Dart :

dart pub get

Lancer l'application

Lancez l'application Dart :

dart run main.dart

Cela démarrera l'application Dart et elle sera accessible sur votre appareil ou émulateur.
🗂️ Structure du Projet

Voici la structure des dossiers et fichiers :

/gestion-stock-app/
├── application_mobile/               # Application Flutter mobile
│   ├── main.dart                     # Point d'entrée de l'application mobile
│   ├── pubspec.yaml                  # Configuration Flutter
│   └── pubspec.lock                  # Verrouillage des versions des dépendances
│ 
├── client_api/                       # Client API Flutter pour interagir avec l'API
│   ├── client_api.dart               # Méthodes pour interagir avec l'API
│   ├── pubspec.yaml                  # Configuration des dépendances du client API
│   └── pubspec.lock                  # Fichier de verrouillage des dépendances
│ 
├── serveur_api/                      # Backend API Node.js
│   ├── db.json                       # Données persistantes des produits/commandes
│   ├── package-lock.json             # Verrouillage des dépendances pour le backend
│   ├── package.json                  # Définition des dépendances et scripts
│   └── server.js                     # Code du serveur backend
│ 
├── README.md                         # Documentation du projet
└── .gitignore                        # Liste des fichiers à ignorer par Git

🔧 Fonctionnement de l'API

    Route GET /produits
    Récupère tous les produits stockés dans db.json :

// serveur_api/server.js
app.get('/produits', (req, res) => {
  fs.readFile('./db.json', 'utf8', (err, data) => {
    if (err) return res.status(500).send('Erreur de lecture.');
    const produits = JSON.parse(data).produits;
    res.status(200).json(produits);
  });
});

    Route POST /produits
    Ajoute un nouveau produit :

// serveur_api/server.js
app.post('/produits', express.json(), (req, res) => {
  const produit = req.body;
  fs.readFile('./db.json', 'utf8', (err, data) => {
    if (err) return res.status(500).send('Erreur de lecture.');
    const db = JSON.parse(data);
    db.produits.push(produit);
    fs.writeFile('./db.json', JSON.stringify(db), (err) => {
      if (err) return res.status(500).send('Erreur de sauvegarde.');
      res.status(201).json(produit);
    });
  });
});

    Route GET /commandes
    Récupère toutes les commandes :

// serveur_api/server.js
app.get('/commandes', (req, res) => {
  fs.readFile('./db.json', 'utf8', (err, data) => {
    if (err) return res.status(500).send('Erreur de lecture.');
    const commandes = JSON.parse(data).commandes;
    res.status(200).json(commandes);
  });
});

    Route POST /commandes
    Crée une nouvelle commande :

// serveur_api/server.js
app.post('/commandes', express.json(), (req, res) => {
  const commande = req.body;
  fs.readFile('./db.json', 'utf8', (err, data) => {
    if (err) return res.status(500).send('Erreur de lecture.');
    const db = JSON.parse(data);
    db.commandes.push(commande);
    fs.writeFile('./db.json', JSON.stringify(db), (err) => {
      if (err) return res.status(500).send('Erreur de sauvegarde.');
      res.status(201).json(commande);
    });
  });
});

🤖 Interaction avec l'API - Client Dart
1. Récupérer et afficher les produits

import 'dart:convert';
import 'package:http/http.dart' as http;

Future<void> getProduits() async {
  final response = await http.get(Uri.parse('http://localhost:3000/produits'));

  if (response.statusCode == 200) {
    var produits = jsonDecode(response.body);
    print('Produits disponibles :');
    for (var produit in produits) {
      print('Nom: ${produit['nom']}, Prix: ${produit['prix']}');
    }
  } else {
    print('Erreur lors de la récupération des produits.');
  }
}

2. Ajouter un produit

import 'dart:convert';
import 'package:http/http.dart' as http;

Future<void> ajouterProduit(String nom, double prix, int stock, String categorie) async {
  final response = await http.post(
    Uri.parse('http://localhost:3000/produits'),
    headers: {'Content-Type': 'application/json'},
    body: jsonEncode({
      'nom': nom,
      'prix': prix,
      'stock': stock,
      'categorie': categorie
    }),
  );

  if (response.statusCode == 201) {
    print('Produit ajouté avec succès');
  } else {
    print('Erreur lors de l\'ajout du produit.');
  }
}

📸 Captures d'écran
Test API avec Postman

Voici à quoi cela ressemble lors des tests dans Postman :
Requête GET /produits
Requête POST /produits
Interface Client Dart

Voici un aperçu de l'interface du client Dart pour afficher les produits et ajouter des commandes :

