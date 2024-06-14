# Service de Notifications Push

Il s'agit d'un service de notifications push simple utilisant les Service Workers, l'API Notification et l'API Push. Le backend est construit avec Express.js et utilise `web-push` pour gérer les notifications push.

## Prérequis

Avant de commencer, assurez-vous d'avoir les éléments suivants installés sur votre machine :

- [Node.js](https://nodejs.org/) (inclut npm)

## Installation

Suivez ces étapes pour configurer et exécuter l'application :

### 1. Cloner le Répertoire

```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. Installer les Dépendances
```bash
npm install

```

### 3. Configurer les Variables d'Environnement

Créez un fichier `.env` à la racine du projet et ajoutez les variables d'environnement suivantes. Vous devez générer des clés VAPID pour `VAPID_PUBLIC_KEY` et `VAPID_PRIVATE_KEY`. Vous pouvez générer ces clés en utilisant la commande suivante dans Node.js :
```bash
const webpush = require('web-push');
const vapidKeys = webpush.generateVAPIDKeys();
console.log(vapidKeys);
```

Ajoutez les clés générées à votre fichier `.env` :
```bash
VAPID_PUBLIC_KEY=your_public_key
VAPID_PRIVATE_KEY=your_private_key

```
### 4. Démarrer le Serveur
```bash
nodemon server.js

```
Le serveur devrait maintenant fonctionner sur `http://localhost:3000`.

## Utilisation

### 1. Servir le Frontend

Ouvrez `index.html` dans votre navigateur. Vous pouvez utiliser un serveur HTTP simple pour servir ce fichier. Par exemple, vous pouvez utiliser le package `http-server` de npm :

```bash
npx http-server
```
ou live server avec vscode

Accédez à l'adresse fournie par `http-server` dans votre navigateur.

### 2. Activer les Notifications

Cliquez sur le bouton "Enable notification" sur la page web. Cela va :

- Vérifier la prise en charge des API nécessaires
- Demander la permission de notifications à l'utilisateur
- S'assurer que votre environnement os ou android accepte les notifications navigateur
- Enregistrer le service worker

### 3. Envoyer une Notification

Utilisez un client HTTP comme Postman ou votre navigateur pour envoyer une requête GET à `http://localhost:3000/send-notification`.
```bash
curl http://localhost:3000/send-notification
```


Cela enverra une notification push au client abonné.

## Explication du Code

### HTML (`index.html`)

Ce fichier contient un bouton pour activer les notifications et inclut le fichier `script.js`.

### JavaScript (`script.js`)

Ce script gère les éléments suivants :

- **Vérification des Permissions** : S'assure que le navigateur prend en charge les Service Workers, les Notifications et l'API Push.
- **Enregistrement du Service Worker** : Enregistre le service worker (`sw.js`).
- **Demande de Permission de Notification** : Demande à l'utilisateur la permission de notifications.
- **Gestion des Abonnements** : S'abonne aux notifications push et enregistre l'abonnement sur le serveur.

### Service Worker (`sw.js`)

Ce script fonctionne en arrière-plan et gère les événements push :

- **Abonnement** : S'abonne aux notifications push avec la clé du serveur d'application.
- **Affichage des Notifications** : Affiche une notification lorsqu'un événement push est reçu.

### Serveur (`server.js`)

Ceci est un serveur Express.js qui gère l'enregistrement des abonnements et les notifications push :

- **Stockage des Abonnements** : Stocke l'abonnement dans un tableau simple.Dans l'idéal il faudrait stocker les informations dans une base de données pour eviter le clean du tableau de dépendances des subscriptions.
- **Envoi des Notifications** : Envoie une notification push en utilisant `web-push`.
