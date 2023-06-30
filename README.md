#  VueCommunityBuilder
The project is a social network application built with Vue.js. It includes basic features to add friends, manage groups, and conduct searches.

## Components
The application consists of several Vue.js components, including:

### Network.vue

This is the main component that manages users and groups. It allows you to navigate between different views and pass information to other components.

### UserCard.vue

This component displays the details of a user. It is used in the Network.vue component to display the list of users.

### GroupCard.vue

This component displays the details of a group. It is used in the Network.vue component to display the list of groups.

### Search.vue

This component allows you to search for users or groups by their name. It is used in the Network.vue component.

### AddMemberDialog.vue

This is a dialog box component that appears when the user wants to add new friends or groups. It filters users and groups based on the search query and allows the user to select those they wish to add. The user can add a maximum of 5 friends or groups at a time.

### Usage
To use the application, all you need to do is clone the repository, install the dependencies, and start the development server.

```sh
git clone https://github.com/your-username/vue-social-network.git
cd vue-social-network
npm install
npm run serve
```
The application will be available at the address http://localhost:{Port}.

## Included Files
AddMemberDialog.vue: This file is a Vue.js component that manages the addition of new friends and groups. It includes a dialog box that appears when the user clicks on the add button. In this dialog box, the user can search for and select new friends and groups to add.

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
