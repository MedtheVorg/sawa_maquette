# Home Page

## Notification Functionality
- **Current status**: This functionality does not exist in the mobile app.
- **To do**: Needs to add related APIs to return a list of notifications and set a notification or all as read.


## Mood Functionality
- **Current functionality**: API allows registering a mood for the day and returns an error message in case the action was already done.
- **New functionality**: The API needs to also return the time the action was taken.

## Proches Aidés List Functionality
- **Current status**: An API already exists which shows the aidés data.
- **To do**: The API needs to also return the aide related pap status (in percentage form).
- **Note**: Some design text is out of context, needs elaboration from client.

## "Ma charge d'aidant" Button
- **Clarification needed**: Where does this button take the user?

# Ressources Page
  - **Current status**: An API exists returning a list of YouTube video URLs with tags. The page has basic search and tag filters, allowing users to click and load videos.
  - **To do**:
    - Update database and API to fetch and store video duration metadata from YouTube.
    - Add a "level" column (3 possible values) to the resources table.
    - Implement filters: "Nouveautés" (by `created_at` threshold), "Mes favoris" (track user favorite status), and "Non vus" (track if user has clicked/viewed).
    - Implement sorting: "Plus récent" (by `created_at`), "Plus court" (by duration), and "A -> Z" (alphabetical).
    - Build a tracking feature for viewed videos to support the "Récemment vus" and "Historique" sections.
    - Implement the "envoyer" button inside the existing sharing modal to send a simple in-app notification to the target user with the resource name/URL (no UI changes).
    - Update the API response to include the total count of videos and the total count of the user's favorited videos for the header statistics.
  - **Note**:
    - "Format" filter: needs elaboration from client on how to determine this value.
    - "Plus populaire" sorting: needs elaboration from client on how to calculate this.
    - "Recommandées pour vous" section: needs elaboration/simplification from client.
    - "65% completion" label: needs elaboration (suggested to simplify to a binary seen/unseen status).
    - "Récent" filter in paginated list: needs clarification as it is redundant with the main sorting.

# Documents Page
  - **Current status**: Users can access shared and private sections. The private section requires premium access. Current features include file upload (private by default), full CRUD operations, and search/filter for documents.
  - **To do (Backend API & Database only)**:
    - Update the database and API to support the newly introduced folder structure (mapping files to specific folders).
    - Add full CRUD API routes for folders (Create, Read, Update/Rename, Delete).
    - Add a "Déplacer vers" API endpoint to allow changing the folder a document belongs to.
    - Add an API endpoint to fetch a specific folder's data and its list of documents.
  - **Note**:
    - The options "Gérer le partage", "Télécharger", "Renommer", and "Supprimer" are already supported.
    - Share modal logic: When "Privé uniquement" is selected, pass an empty list of users to the share API to make the file private. Otherwise, pass the list of selected users (Care Manager / co-aidants).
