#  VueCommunityBuilder

Le projet est une application de réseau social construite avec Vue.js. Elle comprend des fonctionnalités de base pour ajouter des amis, gérer des groupes, et faire des recherches.

## Composants

L'application se compose de plusieurs composants Vue.js, notamment :

Network.vue
C'est le composant principal qui gère les utilisateurs et les groupes. Il permet de naviguer entre différentes vues et de passer des informations aux autres composants.

UserCard.vue
Ce composant affiche les détails d'un utilisateur. Il est utilisé dans le composant Network.vue pour afficher la liste des utilisateurs.

GroupCard.vue
Ce composant affiche les détails d'un groupe. Il est utilisé dans le composant Network.vue pour afficher la liste des groupes.

Search.vue
Ce composant permet de rechercher des utilisateurs ou des groupes par leur nom. Il est utilisé dans le composant Network.vue.

AddMemberDialog.vue
C'est un composant de boîte de dialogue qui s'affiche lorsque l'utilisateur souhaite ajouter de nouveaux amis ou groupes. Il filtre les utilisateurs et les groupes en fonction de la requête de recherche et permet à l'utilisateur de sélectionner ceux qu'il souhaite ajouter. L'utilisateur peut ajouter un maximum de 5 amis ou groupes à la fois.

## Utilisation

Pour utiliser l'application, il vous suffit de cloner le dépôt, d'installer les dépendances et de démarrer le serveur de développement.
```sh
git clone https://github.com/your-username/vue-social-network.git
cd vue-social-network
npm install
npm run serve
```
L'application sera disponible à l'adresse http://localhost:8080.

## Fichiers inclus

AddMemberDialog.vue : Ce fichier est un composant Vue.js qui gère l'ajout de nouveaux amis et groupes. Il inclut une boîte de dialogue qui s'affiche lorsque l'utilisateur clique sur le bouton d'ajout. Dans cette boîte de dialogue, l'utilisateur peut rechercher et sélectionner de nouveaux amis et groupes à ajouter.

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Run Unit Tests with [Vitest](https://vitest.dev/)

```sh
npm run test:unit
```

### Run End-to-End Tests with [Cypress](https://www.cypress.io/)

```sh
npm run test:e2e:dev
```

This runs the end-to-end tests against the Vite development server.
It is much faster than the production build.

But it's still recommended to test the production build with `test:e2e` before deploying (e.g. in CI environments):

```sh
npm run build
npm run test:e2e
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```
