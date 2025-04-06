<!--
SPDX-FileCopyrightText: 2025 Benoît Rolandeau <benoit.rolandeau@allcircuits.com>

SPDX-License-Identifier: MIT
-->

# Web server

## Table of contents <!-- omit from toc -->

- [Web server](#web-server)
  - [Presentation](#presentation)
  - [Needs](#needs)
    - [MUST have](#must-have)
    - [SHOULD have](#should-have)
    - [COULD have](#could-have)

## Presentation

This file contains **WHAT** your customer expects from the web server.

## Needs

### MUST have

- `NWS1`: the things must be able to register themselves to the server thanks to their unique
  identifier and their type.
- `NWS2`: the things must be registered to the server before they can interact to the server.
- `NWS3`: if a thing is unregistered or deleted, the server must delete all the data related to
  this thing.
- `NWS4`: if a thing is unregistered or deleted, it could not interact with the server anymore.
- `NWS5`: if a thing is unregistered or deleted, it must be able to register itself again.
- `NWS6`: when a thing register to the server, it must receive an API key to interact with the
  server. This API key is unique to the device.
- `NWS7`: all the requests done by the things must be authenticated with their API key, except the
  registration and the hello requests.
- `NWS8`: the server must be able to receive any kind of telemetry data from the things. A telemetry
  data is a data retrieved from the environment and which is only relevant in its instantness.
  The telemetry data are linked to a thing.
  For instance: the temperature, the humidity, the pressure, the luminosity, etc.
- `NWS9`: the server must store the telemetry data with the timestamp of the reception.
- `NWS10`: the server must keep the history of the telemetry data.
- `NWS11`: the server must be able to receive attributes from the things and the mobile app. The
  attributes are data which are relevant for a long time and for which the history is not important.
  The attributes are linked to a thing.
  For instance: the temperature unit, the location of the thing, etc.
- `NWS12`: the server attributes received from the mobile app are `server` attributes.
- `NWS13`: the server attributes received from the things are `client` attributes.
- `NWS14`: the mobile app and the things must be able to retrieve all the attributes. But also to
  filter the attributes by their types: `server` or `client`.
- `NWS15`: the mobile app must be able to create, update or delete the `server` attributes linked to
  a thing.
- `NWS16`: a thing must be able to create, update or delete its `client` attributes.
- `NWS17`: the server must store the attributes with the timestamp of the reception.
- `NWS18`: the server must store the data permanently (when you restart the server, the data must
  still be there).
- `NWS19`: the server must have a gui interface to display the requests done by the things and the
  mobile app. The requests must be displayed with their timestamp and the thing which did the
  request. The interface must be updated in real-time. The log level of each request must be
  displayed. The view must be able to filter the requests by their log level.
- `NWS20`: the server must have a gui interface to display the telemetry data received from the
  things. The telemetry data must be displayed with their timestamp and the thing which sent the
  data. The interface must be updated in real-time.
- `NWS21`: the server must have a gui interface to display the attributes received from the things
- `NWS22`: the server must have a gui interface to display the attributes received from the mobile
  app
- `NWS23`: the server must have a gui interface to display the things registered to the server. The
  things must be displayed with their unique identifier, their type, their API key. The interface
  must be updated in real-time.

### SHOULD have

- `NWS+1`: the server should be able to communicate with the things and the mobile app by WebSocket.
  This implies that the web server create a WebSocket server. The WebSocket should be used to
  interact with the things and mobile app in real-time. The HTTP server must be kept.
- `NWS+2`: the server should store its data with sqlite (see: https://pub.dev/packages/sqflite)

### COULD have

- `NWS++1`: the user on its mobile app could be able to authenticate itself with a login and a
  password. The login and the password must be stored in the server. The password must be hashed.
  This implies to define new HTTP requests to authenticate the user and to create a new user. But
  also to protect the previous HTTP requests which need to be authenticated.

Modification du serveur mise en place :

## ⚙️ Technologies utilisées

- `Dart`
- `shelf` : framework de serveur HTTP
- `shelf_router` : pour le routage REST
- `WebSocket` : pour la communication en temps réel
- `uuid` : pour la génération d’API keys
- Fichier local `data.json` : base de données simple pour persistance

---

## 📌 Fonctionnalités principales

### 1. 🔐 Enregistrement d'un capteur (`POST /register`)

Permet à un capteur de s'enregistrer sur le serveur.

- **Entrée JSON** : `{ "id": "capteur123", "type": "accelerometer" }`
- **Réponse** : API key générée aléatoirement

> ✅ Si déjà enregistré, renvoie une erreur 400.

---

### 2. 📋 Liste des capteurs enregistrés (`GET /things`)

Renvoie tous les capteurs enregistrés avec leur type et API key.

---

### 3. 📡 Envoi de données de télémétrie (`POST /telemetry/<id>`)

Un capteur envoie ses données, qui sont timestampées et stockées.

- **Entrée JSON** : `{ "x": 3.4, "y": -1.2, "z": 9.8 }`
- **Stockage** : `telemetryData[id]` → liste des `{data, timestamp}`

---

### 4. 📊 Récupération des télémétries (`GET /telemetry/<id>`)

Renvoie toutes les données enregistrées pour un capteur.

---

### 5. 🧠 Attributs personnalisés (`POST /attributes/<id>?type=client|server`)

Ajoute ou met à jour des attributs liés à un capteur ou au serveur.

- Requiert le header `Authorization` avec la bonne API Key
- Attributs sont timestampés et stockés

---

### 6. 🧾 Récupération des attributs (`GET /attributes/<id>?type=client|server`)

- `type=client` : renvoie les attributs du capteur
- `type=server` : renvoie les attributs globaux serveur
- Sans type : renvoie les deux

---

### 7. 🗑️ Suppression des attributs (`DELETE /attributes/<id>?type=client|server`)

Supprime les attributs d’un capteur ou ceux du serveur.

---

### 8. ❌ Désenregistrement (`DELETE /unregister/<id>`)

Supprime un capteur de la base : ses données, attributs, et télémétries.

---

## 💾 Persistance des données

Toutes les données sont enregistrées localement dans `data.json` :

```json
{
  "thingsRegistry": {
    "capteur123": { "type": "accelerometer", "apiKey": "xxxx-xxxx" }
  },
  "telemetryData": {
    "capteur123": [{ "data": { "x": 3.4, "y": -1.2 }, "timestamp": "..." }]
  },
  "clientAttributesData": {
    "capteur123": {
      "battery": { "value": 88, "timestamp": "..." }
    }
  },
  "serverAttributes": {
    "mode": { "value": "eco", "timestamp": "..." }
  }
}
```
