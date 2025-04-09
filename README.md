# 🏥 Medical Platform

Une plateforme de gestion des soins de santé complète connectant patients, médecins et personnel médical. Cette application full-stack permet la prise de rendez-vous, la gestion des dossiers médicaux, la communication sécurisée, et plus encore.

## 📚 Table des Matières

- [🧠 Aperçu](#-aperçu)
- [✨ Fonctionnalités](#-fonctionnalités)
- [📐 Architecture du Système](#-architecture-du-système)
- [⚙️ Backend](#️-backend)
  - [Technologies](#technologies)
  - [Structure de la Base de Données](#structure-de-la-base-de-données)
  - [Endpoints de l'API](#endpoints-de-lapi)
  - [Contrôleurs](#contrôleurs)
  - [Middleware](#middleware)
- [🎨 Frontend](#-frontend)
  - [Technologies](#technologies-1)
  - [Composants](#composants)
  - [Pages](#pages)
  - [Gestion de l'État](#gestion-de-létat)
- [🔐 Authentification & Sécurité](#-authentification--sécurité)
- [☁️ Services Cloud](#️-services-cloud)
- [🛠️ Installation & Configuration](#️-installation--configuration)
- [🧪 Variables d'Environnement](#-variables-denvironnement)
- [🚀 Déploiement](#-déploiement)
- [🔮 Améliorations Futures](#-améliorations-futures)
- [📄 Licence](#-licence)
- [👥 Contribution](#-contribution)
- [📞 Contact](#-contact)

## 🧠 Aperçu

Le projet Medical Platform est composé de deux principaux modules :

- **Backend (medicalPlatformBackend)** : Un serveur API Node.js/Express gérant la logique métier et l'accès à la base de données.
- **Frontend (medicalPlatformFrontend)** : Une application React offrant une interface utilisateur réactive et intuitive.

## ✨ Fonctionnalités

- ✅ Authentification multi-rôles (patient, médecin, infirmier, secrétaire, manager)
- ✅ Prise de rendez-vous et gestion des disponibilités
- ✅ Création de rapports médicaux avec transcription vocale
- ✅ Ajout de notes par le personnel soignant
- ✅ Interface de messagerie temps réel
- ✅ Paiements en ligne avec Stripe
- ✅ Tableaux de bord personnalisés
- ✅ Support multi-hôpitaux
- ✅ Génération de rapports en PDF

## 📐 Architecture du Système

- **Client** : Application monopage (SPA) basée sur React.
- **Serveur** : API REST construite avec Node.js et Express.js.
- **Base de Données** : Entrepôt de données cloud Snowflake.
- **Services Externes** :
  - Cloudinary : Stockage et gestion des images.
  - Google Cloud Speech-to-Text : Services de transcription audio.
  - Stripe : Traitement des paiements en ligne.
  - Socket.IO : Communication en temps réel bidirectionnelle.

## ⚙️ Backend

### Technologies

- **Node.js** : Environnement d'exécution JavaScript côté serveur.
- **Express.js** : Framework web minimaliste pour Node.js.
- **Snowflake** : Entrepôt de données cloud pour le stockage des données de l'application.
- **JWT & bcrypt.js** : Authentification sécurisée et hachage des mots de passe.
- **Multer** : Gestion des téléchargements de fichiers.
- **PDFKit** : Génération de documents PDF.
- **Socket.IO** : Communication en temps réel.
- **Stripe API** : Intégration pour le traitement des paiements.

### Structure de la Base de Données

La base de données Snowflake comprend les tables principales suivantes :

- `PATIENTS` : Informations sur les patients.
- `DOCTORS` : Profils et informations des médecins.
- `NURSES` : Profils du personnel infirmier.
- `SECRETARIES` : Profils du personnel administratif.
- `MANAGERS` : Profils du personnel de gestion.
- `HOSPITALS` : Informations sur les hôpitaux/cliniques.
- `APPOINTMENTS` : Rendez-vous programmés entre patients et médecins.
- `REPORTS` : Rapports médicaux créés par les médecins.
- `NURSENOTES` : Notes ajoutées par les infirmiers aux rapports médicaux.
- `MESSAGES` : Communications entre les professionnels de santé.

### Endpoints de l'API

<details>
<summary><b>📍 Routes Patient</b></summary>

- `POST /api/patient/register` : Créer un nouveau compte patient.
- `POST /api/patient/login` : Authentifier les patients.
- `GET /api/patient/get-profile` : Récupérer le profil du patient.
- `PUT /api/patient/update-profile` : Mettre à jour les informations du patient.
- `GET /api/patient/appointments` : Obtenir les rendez-vous du patient.
</details>

<details>
<summary><b>👨‍⚕️ Routes Médecin</b></summary>

- `POST /api/doctor/login` : Authentifier les médecins.
- `GET /api/doctor/list` : Obtenir la liste de tous les médecins.
- `GET /api/doctor/appointments` : Obtenir les rendez-vous du médecin.
- `PUT /api/doctor/complete-appointment` : Marquer un rendez-vous comme terminé.
- `GET /api/doctor/profile` : Obtenir le profil du médecin.
- `POST /api/doctor/add-report` : Ajouter un rapport médical pour un patient.
- `GET /api/doctor/patient-reports/:patientId` : Consulter les rapports médicaux d'un patient.
- `POST /api/doctor/upload-audio-report` : Transcrire un enregistrement vocal en texte et l'enregistrer comme rapport.
</details>

<details>
<summary><b>👩‍⚕️ Routes Infirmier</b></summary>

- `POST /api/nurse/login` : Authentification d'un infirmier.
- `GET /api/nurse/profile` : Voir le profil de l'infirmier.
- `PUT /api/nurse/add-note` : Ajouter une note à un rapport médical.
- `GET /api/nurse/patients` : Voir les patients affectés à l'infirmier.
</details>

<details>
<summary><b>👩‍💼 Routes Secrétaire</b></summary>

- `POST /api/secretary/login` : Authentification d'une secrétaire.
- `GET /api/secretary/doctors` : Liste des médecins à assigner.
- `POST /api/secretary/create-appointment` : Créer un rendez-vous pour un patient.
- `GET /api/secretary/appointments` : Voir les rendez-vous de l'hôpital.
</details>

<details>
<summary><b>👨‍💼 Routes Manager</b></summary>

- `POST /api/manager/login` : Authentification d'un gestionnaire.
- `GET /api/manager/dashboard` : Statistiques d'utilisation et activité globale.
- `PUT /api/manager/update-hospital` : Mise à jour des informations hospitalières.
</details>

<details>
<summary><b>🏥 Routes Hôpital</b></summary>

- `POST /api/hospital/register` : Enregistrement d'un hôpital.
- `GET /api/hospital/list` : Liste de tous les hôpitaux enregistrés.
</details>

<details>
<summary><b>💳 Routes Paiement (Stripe)</b></summary>

- `POST /api/payment/create-session` : Créer une session de paiement Stripe.
- `GET /api/payment/success` : Callback après un paiement réussi.
- `GET /api/payment/cancel` : Callback après un paiement annulé.
</details>

<details>
<summary><b>🗨️ Routes Chat (Socket.IO)</b></summary>

- Connexion WebSocket pour discussion en temps réel.
- Échange de messages entre médecin et personnel infirmier.
- Notification temps réel de nouveaux messages.
</details>

### Contrôleurs

Chaque route est gérée par un contrôleur spécifique selon le modèle MVC.
Exemples de fichiers contrôleurs :

- `patientController.js`
- `doctorController.js`
- `appointmentController.js`
- `hospitalController.js`
- `reportController.js`
- `paymentController.js`

### Middleware

- `authMiddleware.js` : Vérification des tokens JWT.
- `roleMiddleware.js` : Vérification des rôles (admin, doctor, nurse, etc.).
- `errorHandler.js` : Gestion des erreurs globales.
- `uploadMiddleware.js` : Gestion des fichiers (audio, image, PDF).

## 🎨 Frontend

### Technologies

- **React.js** : Bibliothèque JavaScript pour l'interface.
- **Redux Toolkit** : Gestion de l'état centralisé.
- **Axios** : Requêtes HTTP vers l'API.
- **React Router** : Navigation entre les pages.
- **Tailwind CSS** : Framework CSS utilitaire.
- **Socket.IO Client** : Support des chats temps réel.
- **PDF Viewer** : Affichage de fichiers médicaux.

### Composants

- Navbar, Sidebar, Footer
- LoginForm, RegisterForm, ProfileCard
- DoctorDashboard, PatientDashboard, AdminDashboard
- AppointmentCard, ReportViewer, ChatBox

### Pages

- `/login`
- `/register`
- `/dashboard`
- `/appointments`
- `/patients`
- `/reports/:id`
- `/chat`
- `/payment`

### Gestion de l'État

- Redux Slices : userSlice, appointmentSlice, reportSlice, etc.
- Auth persistée avec redux-persist.
- WebSocket listener dans le socketSlice.

## 🔐 Authentification & Sécurité

- JWT (JSON Web Tokens) pour chaque utilisateur.
- bcrypt pour le hachage sécurisé des mots de passe.
- CORS et Helmet pour la sécurité HTTP.
- Role-Based Access Control (RBAC) pour filtrer les accès selon le rôle.
- Rate Limiting pour prévenir les attaques DDoS.

## ☁️ Services Cloud

| Service | Utilisation |
|---------|-------------|
| Cloudinary | Upload & gestion des photos de profils |
| Google STT | Transcription audio des rapports |
| Stripe | Paiements et facturation |
| Snowflake | Stockage des données médicales |
| Socket.IO | Messagerie et notifications en temps réel |

## 🛠️ Installation & Configuration

### 1. Cloner les projets

```bash
git clone https://github.com/username/medicalPlatformBackend.git
git clone https://github.com/username/medicalPlatformFrontend.git
```

### 2. Installer les dépendances

**Backend:**
```bash
cd medicalPlatformBackend
npm install
```

**Frontend:**
```bash
cd medicalPlatformFrontend
npm install
```

### 3. Démarrer les serveurs de développement

**Backend:**
```bash
cd medicalPlatformBackend
npm run dev
```

**Frontend:**
```bash
cd medicalPlatformFrontend
npm start
```

## 🧪 Variables d'Environnement

### Backend `.env`

```env
PORT=5000
DB_USER=****
DB_PASSWORD=****
DB_ACCOUNT=****
DB_WAREHOUSE=****
DB_DATABASE=****
DB_SCHEMA=****
DB_ROLE=****
JWT_SECRET=secretkey
CLOUDINARY_API_KEY=*****
CLOUDINARY_SECRET=*****
CLOUDINARY_NAME=*****
STRIPE_SECRET_KEY=*****
GOOGLE_APPLICATION_CREDENTIALS=google-credentials.json
```

### Frontend `.env`

```env
REACT_APP_API_URL=http://localhost:5000/api
REACT_APP_SOCKET_URL=http://localhost:5000
REACT_APP_STRIPE_PUBLIC_KEY=*****
```

## 🚀 Déploiement

### Option 1: Déploiement Traditionnel

- **Backend**: Déployez sur Render, Heroku, Railway ou AWS EC2.
- **Frontend**: Déployez sur Netlify, Vercel ou AWS S3.

### Option 2: Conteneurisation Docker

1. Créez un fichier Dockerfile pour chaque module.
2. Construisez les images Docker:
   ```bash
   docker build -t medical-platform-backend ./medicalPlatformBackend
   docker build -t medical-platform-frontend ./medicalPlatformFrontend
   ```
3. Utilisez docker-compose pour orchestrer les services.

## 🔮 Améliorations Futures

- [ ] Ajout d'un calendrier intelligent pour les médecins
- [ ] Intégration avec des dossiers médicaux électroniques (DME) externes
- [ ] Authentification biométrique (empreinte, reconnaissance faciale)
- [ ] Support multilingue
- [ ] Statistiques de santé pour patients chroniques
- [ ] Intégration vidéo pour téléconsultation
- [ ] Applications mobiles natives (iOS/Android)
- [ ] Intégration avec des appareils IoT médicaux

## 📄 Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.

## 👥 Contribution

Les contributions sont les bienvenues ! N'hésitez pas à forker le projet, créer une branche feature, et soumettre une pull request.

1. Forkez le projet
2. Créez votre branche (`git checkout -b feature/amazing-feature`)
3. Committez vos changements (`git commit -m 'Add some amazing feature'`)
4. Poussez vers la branche (`git push origin feature/amazing-feature`)
5. Ouvrez une Pull Request

## 📞 Contact

KOCEILA DJABALLAH –– koceila.djaballah@gmail.com

Lien du projet: [https://github.com/ITNKOC/medical-platform](https://github.com/ITNKOC/medical-platform)
