# Page d'Accueil

## Fonctionnalité de Notification
- **Statut actuel** : Terminals API implémentés pour l'application mobile.
- **Terminé (API disponibles pour Mobile)** : 
  - `GET /api/notifications` : Retourne une liste paginée de notifications.
  - `POST /api/notifications/mark-all-read` : Marque toutes les notifications non lues comme lues.
  - `POST /api/notifications/{id}/read` : Marque une notification spécifique comme lue.

## Fonctionnalité d'Humeur (Mood)
- **Fonctionnalité actuelle** : L'API permet d'enregistrer une humeur pour la journée et retourne un message d'erreur si l'action a déjà été effectuée.
- **Terminé (API disponibles pour Mobile)** : 
  - `GET /api/aidants/daily-mood/result` : Retourne le statut actuel et l'heure de la dernière humeur enregistrée (datetime).

## Fonctionnalité de Liste des Proches Aidés
- **Statut actuel** : Une API existe déjà qui affiche les données des aidés.
- **À faire** : 
- L'API doit également retourner le statut du PAP lié à l'aidé (sous forme de pourcentage) (en cours).

- **Terminé (API & Données disponibles pour Mobile)** :
  - **Mise à jour des ressources** : Les réponses existantes de l'API Aide (via `AideResource`) incluent désormais un tableau `documents`. Ce tableau contient automatiquement tous les documents qui appartiennent au dossier associé de cet Aidé (chargés avec leurs médias et tags). Vous n'avez PAS besoin d'une requête séparée pour récupérer les documents d'un Aidé !
  - `POST /api/documents/aides/create` : Utilisé lorsqu'un utilisateur télécharge un document depuis l'écran de profil d'un Aidé. Il télécharge le document spécifiquement pour cet Aidé et l'assigne automatiquement à son dossier (créant automatiquement le dossier s'il n'existe pas). Les aidants propriétaires peuvent passer `is_visible_to_co_aidant` (0 ou 1).


# Page de Ressources
  - **Statut actuel** : Une API existe retournant une liste d'URLs de vidéos YouTube avec des tags. La page dispose de filtres de recherche et de tags basiques, permettant aux utilisateurs de cliquer et de charger des vidéos.
  - **Terminé (API & Données disponibles pour Mobile)** :
    - **Mise à jour des ressources (`GET /api/masterclasses/all`)** : 
      - Les métadonnées de réponse (`data.meta`) incluent désormais `total_videos` et `favorited_count`. Le point d'accès supporte aussi entièrement la pagination côté serveur, la recherche (`?search=...`), et le filtrage par tags (`?tags=...`).
      - Vous pouvez maintenant passer `status` comme paramètre de requête : `?status=new` (publié dans les 7 derniers jours), `?status=favorites` (mis en favori par l'utilisateur actuel), ou `?status=unseen` (jamais cliqué/vu par l'utilisateur actuel).
      - Vous pouvez trier en passant `sort` : `?sort=-created_at` (Plus récent), `?sort=title` (A -> Z), `?sort=duration_seconds` (Plus court).
      - Vous pouvez filtrer par durée : `?duration={max_seconds}`.
    - `POST /api/masterclasses/{masterclass}/share` : Partage une masterclass en envoyant une notification in-app à un utilisateur cible (`target_user_id`).
  - **À faire (Script Backend)** :
    - Exécuter un script pour récupérer la durée réelle des vidéos sur YouTube et peupler la colonne `duration_seconds`.
  - **Note** :
    - D'autres filtres, options de tri, et éléments d'interface (tels que "Format", tri "Plus populaire", section "Recommandées pour vous", étiquette "65% completion", et le filtre redondant "Récent") seront implémentés plus tard.

# Page des Documents
  - **Statut actuel** : Les utilisateurs peuvent accéder aux sections partagées et privées. La section privée nécessite un accès premium. Les fonctionnalités actuelles incluent le téléchargement de fichiers (privé par défaut), des opérations CRUD complètes, et la recherche/filtrage de documents.
  - **Terminé (API disponibles pour Mobile)** : 
    - `GET /api/documents/folders/all` : Liste les dossiers unifiés (dossiers personnels + dossiers Aidés). Filtré dynamiquement pour que vous ne voyiez que les dossiers vous appartenant ou aux Aidés auxquels vous êtes lié.
    - `POST /api/documents/folders` : Crée un dossier personnel sur mesure (`name` requis).

    - `PUT /api/documents/folders/{folder}` : Renomme un dossier personnalisé (`name` requis). Politique de mise à jour : impossible de renommer les dossiers Aidés générés automatiquement.
    - `DELETE /api/documents/folders/{folder}` : Supprime un dossier personnalisé (les documents à l'intérieur resteront, mais seront détachés). Politique de suppression : impossible de supprimer les dossiers Aidés générés automatiquement.
    - `GET /api/documents/folders/{folder}/documents` : Récupère la liste paginée de documents strictement à l'intérieur de ce dossier.
    - `POST /api/documents/{document}/folder` : Déplace un document existant dans un dossier (`document_folder_id` requis). Nécessite une autorisation pour mettre à jour le dossier de destination.
  - **Note** :
    - Les options "Gérer le partage", "Télécharger", "Renommer", et "Supprimer" sont déjà supportées par les API Document existantes.
    - Logique du modal de partage : Quand "Privé uniquement" est sélectionné, passez une liste vide d'utilisateurs à l'API de partage pour rendre le fichier privé. Sinon, passez la liste des utilisateurs sélectionnés (Care Manager / co-aidants).

---

# Home Page

## Notification Functionality
- **Current status**: API endpoints implemented for mobile app.
- **Done (APIs available for Mobile)**: 
  - `GET /api/notifications`: Returns a paginated list of notifications.
  - `POST /api/notifications/mark-all-read`: Marks all unread notifications as read.
  - `POST /api/notifications/{id}/read`: Marks a specific notification as read.

## Mood Functionality
- **Current functionality**: API allows registering a mood for the day and returns an error message in case the action was already done.
- **Done (APIs available for Mobile)**: 
  - `GET /api/aidants/daily-mood/result`: Returns the current status and time of the last registered mood (datetime).

## Proches Aidés List Functionality
- **Current status**: An API already exists which shows the aidés data.
- **To do**: 
- The API needs to also return the aide related pap status (in percentage form) (work in progress).

- **Done (APIs & Data available for Mobile)**:
  - **Resource Update**: The existing Aide API responses (via `AideResource`) now include a `documents` array. This array automatically contains all documents that belong to this Aide's associated folder (eagerly loaded with their media and tags). You do NOT need a separate endpoint to fetch an Aide's documents!
  - `POST /api/documents/aides/create`: Used when a user uploads a document from an Aide's profile screen. It uploads the document specifically for that Aide and automatically assigns it to their folder (auto-creating the folder if it doesn't exist). Owner Aidants can pass `is_visible_to_co_aidant` (0 or 1).


# Ressources Page
  - **Current status**: An API exists returning a list of YouTube video URLs with tags. The page has basic search and tag filters, allowing users to click and load videos.
  - **Done (APIs & Data available for Mobile)**:
    - **Resource Update (`GET /api/masterclasses/all`)**: 
      - The response metadata (`data.meta`) now includes `total_videos` and `favorited_count`. The endpoint also fully supports server-side pagination, search (`?search=...`), and tag filtering (`?tags=...`).
      - You can now pass `status` as a query parameter: `?status=new` (published in last 7 days), `?status=favorites` (favorited by current user), or `?status=unseen` (never clicked/viewed by current user).
      - You can sort by passing `sort`: `?sort=-created_at` (Plus récent), `?sort=title` (A -> Z), `?sort=duration_seconds` (Plus court).
      - You can filter by duration: `?duration={max_seconds}`.
    - `POST /api/masterclasses/{masterclass}/share`: Shares a masterclass by sending an in-app notification to a target user (`target_user_id`).
  - **To do (Backend Script)**:
    - Run a script to fetch the actual duration of the videos from YouTube and populate the `duration_seconds` column.
  - **Note**:
    - Other filters, sorting options, and UI elements (such as "Format", "Plus populaire", "Recommandées pour vous", "65% completion" label, and redundant "Récent" filter) will be implemented later.

# Documents Page
  - **Current status**: Users can access shared and private sections. The private section requires premium access. Current features include file upload (private by default), full CRUD operations, and search/filter for documents.
  - **Done (APIs available for Mobile)**: 
    - `GET /api/documents/folders/all`: Lists unified folders (personal + Aide folders). Scoped dynamically so you only see folders belonging to you or Aides you are linked to.
    - `POST /api/documents/folders`: Creates a custom personal folder (`name` required).

    - `PUT /api/documents/folders/{folder}`: Renames a custom folder (`name` required). Update policy: cannot rename auto-generated Aide folders.
    - `DELETE /api/documents/folders/{folder}`: Deletes a custom folder (documents inside will remain, but become untethered). Delete policy: cannot delete auto-generated Aide folders.
    - `GET /api/documents/folders/{folder}/documents`: Fetches paginated list of documents strictly inside that folder.
    - `POST /api/documents/{document}/folder`: Moves an existing document into a folder (`document_folder_id` required). Requires authorization to update the destination folder.
  - **Note**:
    - The options "Gérer le partage", "Télécharger", "Renommer", and "Supprimer" are already supported by existing Document APIs.
    - Share modal logic: When "Privé uniquement" is selected, pass an empty list of users to the share API to make the file private. Otherwise, pass the list of selected users (Care Manager / co-aidants).
