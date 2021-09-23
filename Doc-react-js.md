<!-- # Summary -->
<!-- # Plan
# Partie 1 - Installation de l'environnement React 

#### Prérequis 
## A. Création d'un projet React
## I. ## I. 3 Méthodes

-   ### Manuel avec webpack et babel
    
-   ###  Create-React-App
    #### 1. Installation de Create-React-App

- ### Vite
    #### 1. Installation de vite

## II. Pourquoi vite plutôt que CRA

# Partie 2 - Comment architecturer son projet React

### A. IHM de référence

### B. Conception technique 

#### 1. React router

#### 2. Gestion des états globaux
#### Installation de redux et redux-toolkit

## Schéma

#### Exemple de la gestion de l'état de la configuration de notre application globale

#### 3. Découpage des composants 

#### 4. Requêtes Api

#### 5. Communication entre les composants

#### 6. Architecture globale  des fichiers 

### C. Bonnes pratiques

#### 1. Séparer la logique du rendu visuel

#### 2. Principe de responsabilité unique

#### 3. Destructuration des objets

#### 4. Utilisation des propTypes

#### 5. Utilisation des conditions de rendu 

#### 6. Gestion des classes CSS
 		
#### 7. Utilisation d'un linter

#### 8. Evitez les excès de commentaires

### D. Librairies pratiques -->

 

# Partie 1 - Installation de l'environnement React 

#### Prérequis 

Installation du serveur et gestionnaire de paquets

node js => https://nodejs.org/en/

## A. Création d'un projet React
## I. 3 Méthodes
-   ### Manuel avec webpack et babel
   Suivre la prcocédure https://imranhsayed.medium.com/set-up-react-app-with-webpack-webpack-dev-server-and-babel-from-scratch-df398174446d

    **liens utiles:**

    https://babeljs.io/docs/en/babel-preset-react

    https://webpack.js.org/loaders/babel-loader/

-   ###  Create-React-App

    #### 1. Installation de Create-React-App
Suivre la prcocédure https://create-react-app.dev/docs/getting-started

- ### Vite
    #### 1. Installation de vite
Suivre la prcocédure https://vitejs.dev/guide/

## II. Pourquoi VITE plutôt que CRA

**CRA**

Create React App est un outil efficace pour créer facilement un projet React, en effet il fournit de nombreuses fonctionnalités utiles telles que le HMR (Hot Module Replacement) ainsi que le serveur de développement. Cependant, il présente un gros inconvénient qui est le manque de performance que ce soit pour le développement ou le build,  surtout pour les gros projets. CRA utilise derriére webpack.

**VITE**

C'est un outil de nouvelle génération pour construire le front, Outil créé par Evan You créateur de vue-js. Il dispose d'une interface de ligne de commande solide qui facilite considérablement le processus de configuration du projet. Il fonctionne très rapidement et possède de nombreuses fonctionnalités intéressantes fournies par CRA. De plus, il existe des plugins qui peuvent être ajoutés et rendent le processus de développement plus simple.
Lorsque CRA et Vite sont comparés, il existe de nombreuses différences en termes d'expérience et de temps de construction. Dans CRA, il n'y a littéralement pas de fichier de configuration pour le processus de construction, tout est super simple. Cela facilite la vie des petits projets, mais lorsque le projet prend de l'ampleur, le processus de construction doit être configuré en fonction des besoins du projet. En revanche, Vite fournit un fichier vite.config.js qui contient des configurations supplémentaires pour le projet et il est vraiment simple et facile à configurer.  

L'autre différence avec CRA est le temps du build de l'application pour le développement, CRA regroupe tous les fichiers de l'application à chaque changement alors que Vite ne regroupe que les fichiers modifiés, d'ou le gain de performance. 

En resumé pour les petits projets CRA peut être suffisant, en revanche utilisez vite si le projet prend de l'ampleur.

# Partie 2 - Comment architecturer son projet React

### A. IHM de référence
![IHM](./IHM_My_business.png)

### B. Conception technique 

#### 1. React router

Documentation : https://reactrouter.com/web/guides/quick-start

Dans le contexte de notre IHM, l'application globale aura un router qui redirigera vers chaque features et ensuite chaque feature gére son propre router.

#### 2. Gestion des états globaux

#### Installation de redux et redux-toolkit
Suivre la proccédure de cette documentation https://redux.js.org/introduction/getting-started

#### Schéma des multiples reducers
![IHM](./out/redux/redux.svg)

#### Exemple de gestion d'état de notre config
Dans ce contexte nous créons un fichier configSlice.js. Cet état nous permettra de mettre à disposition de toute l'application et micro-applications les données du magasin selectionné et celles de l'utilisateur. 

**configSlice.js**
```js configSlice.js
import { createSlice } from '@reduxjs/toolkit';
import { useSelector } from 'react-redux';

const configSlice = createSlice({
  name: 'configSlice',
  initialState: {
    selectedStore: null,
    user: undefined,
  },
  reducers: {
    updateSelectedStore: (state, action) => {
      const selectedStore = { ...state.selectedStore, ...action.payload };
      return { ...state, selectedStore };
    },
    updateUser: (state, action) => ({ ...state, user: action.payload }),
  },
});


// On exporte les fonctions qui nous retournent la valeur associée dans l'état
export const getSelectedStore = (state) => state.config.selectedStore;
export const getUser = (state) => state.config.user;

//On exporte les actions pour les utiliser dans les composants qui vont déclencher l'action au travers d'un dispatch
export const { updateSelectedStore, updateUser } = configSlice.actions;

// On exporte le reducer pour l'intégrer au REDUCER global 
export default configSlice.reducer;

// On export des hooks customisés afin d'éviter la duplication de code dans tous les composants
export const useUser = () => useSelector(getUser);
export const useSelectedStore = () => useSelector(getSelectedStore);
export const useSelectedStoreId = () => useSelectedStore()?.id;

}
``` 

**Déclencher l'action de mise à jour du state config**

Imaginons un composant User.jsx qui affichera le nom de l'utilisateur et qui s'occupera de déclencher l'action nécessaire pour la mise à jour du state global
```jsx 
import { useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import {useUser, updateUser} from 'configSlice.js'
import {getUser} from 'user/services'

export const User = () => {
    const dispatch = useDispatch();
    // Récupération des données users au travers de notre custom hook
    const user = useUser();

    //dispatch l'action updateUser no va ensuite mettre à jour le store user de redux avec les données récoltées 
    useEffect(async () => {
         dispatch(updateUser(await getUser()));
    }, []);

    return <h1>{user?.name}</h1>
    }
 ```


#### 3. Découpage des composants 
#### Schéma
![IHM](./out/listOfComponents/listOfComponents.svg)
    - Communication entre composants 
    - Eviter les rendus superflus
    - Tests des composants (librairies + cas d'utilisations)
    - Exemple de structure des fichiers d'un composant (custom hooks(ex: useFecth, useInput), services, jsx)

#### 4. Requêtes Api

    - fetch, axios 
    - react-query 
    - useEffect

#### 5. Communication entre les composants

    - props direct 
    - useContext

#### 6. Architecture globale  des fichiers 

* schéma 

### C. Bonnes pratiques

#### 1. Séparer la logique du rendu visuel

    - Exemple de custom hooks
    - Bonnes pratiques 

#### 2. Principe de responsabilité unique

#### 3. Destructuration des objets

#### 4. Utilisation des propTypes

#### 5. Utilisation des conditions de rendu 

    - Simplifier la lecture et compréhension du code avec les conditions à la volée plutôt que des ternaires

#### 6. Gestion des classes CSS

    - Package classnames		
    - Package styled components 		

#### 7. Utilisation d'un linter

#### 8. Evitez les excès de commentaires

### D. Librairies pratiques

 
