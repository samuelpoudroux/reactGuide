<!-- # SOMMAIRE -->
# Méthodologie React-js
#Sommaire
 1. [Partie 1. Installation de l'environnement React](#partie-1.-installation-de-l'environnement-react)
    1. [Prérequis](#prérequis) 
    2. [Création d'un projet React](#création-d'un-projet-react) 
        1. [Méthodes](#méthodes)
            1. [Manuel avec webpack et babel](#manuel-avec-webpack-et-babel)
            2. [Create-React-App](#create-react-app)
                1. [Installation de Create-React-App](#create-react-app)
            3. [Vite](#vite)
                1. [Installation de vite](#installation-de-vite)
            4. [Pourquoi VITE plutôt que CRA](#pourquoi-vite-plutôt-que-cra)       
 2. [Partie 2. Comment architecturer son projet React](#partie-2.-comment-architecturer-son-projet-react)
    1. [IHM de référence](#ihm-de-référence)
    2. [Conception technique](#conception-technique)
    3. [React Router](#react-router)
    4. [Gestion des états globaux](#gestion-des-états-globaux)
        1. [Installation de redux et redux-toolkit](#installation-de-redux-et-redux-toolkit)
        2. [Schéma des multiples reducers de notre application](#schéma-des-multiples-reducers-de-notre-application)
        3. [Exemple de gestion d'état de notre config](#exemple-de-gestion-d'état-de-notre-config)
    5. [Découpage des composants](#découpage-des-composants)
        1. [Schéma](#schéma)
        2. [Architecture fichier d'un composant](#architecture-fichier-d'un-composant)
    6. [Gestion Requêtes Api](#gestion-requêtes-api)
        1. [React-Query](#react-query)
            1. [L'interêt factuel d'utiliser React-query](#l'interêt-factuel-d'utiliser-react-query)
                1. [Composant Consolidations sans l'utilisation de react Query](#composant-consolidations-sans-l'utilisation-de-react-query)
                2. [Composant Consolidations avec l'utilisation de react Query](#composant-consolidations-avec-l'utilisation-de-react-query)
    7. [Communication entre les composants](#communication-entre-les-composants)
    8. [Architecture globale des fichiers de l'application](#architecture-globale-des-fichiers-de-l'application)
    9. [Bonnes pratiques](#bonnes-pratiques)
        1. [Séparer la logique du rendu visuel](#séparer-la-logique-du-rendu-visuel)
        2. [Evitez les excès de commentaires](#evitez-les-excès-de-commentaires)
        3. [Convention de nommage](#convention-de-nommage)
        4. [Principe de responsabilité unique](#principe-de-responsabilité-unique)
        5. [Destructuration des objets](#destructuration-des-objets)
        6. [Utilisation des propTypes](#utilisation-des-proptypes)
        7. [Utilisation des conditions de rendu](#utilisation-des-conditions-de-rendu) 
        8. [Gestion des classes CSS](#gestion-des-classes-css)	
        9. [Utilisation d'un linter](#utilisation-d'un-linter)
        10. [Utilisation de sonarlint](#utilisation-de-sonarlint)
        11. [Librairies pratiques](#librairies-pratiques)

# Partie 1. Installation de l'environnement React 

#### Prérequis 

Installation du serveur et gestionnaire de paquets

node js => https://nodejs.org/en/

## Création d'un projet React
## Méthodes
- ### Manuel avec webpack et babel
   Suivre la prcocédure https://imranhsayed.medium.com/set-up-react-app-with-webpack-webpack-dev-server-and-babel-from-scratch-df398174446d

    **liens utiles:**

    https://babeljs.io/docs/en/babel-preset-react

    https://webpack.js.org/loaders/babel-loader/

- ###  Create-React-App

    #### Installation de Create-React-App
Suivre la prcocédure https://create-react-app.dev/docs/getting-started

- ### Vite
    #### Installation de vite
Suivre la prcocédure https://vitejs.dev/guide/

## II. Pourquoi VITE plutôt que CRA

**CRA**

Create React App est un outil efficace pour créer facilement un projet React, en effet il fournit de nombreuses fonctionnalités utiles telles que le HMR (Hot Module Replacement) ainsi que le serveur de développement. Cependant, il présente un gros inconvénient qui est le manque de performance que ce soit pour le développement ou le build,  surtout pour les gros projets. CRA utilise derriére webpack.

**VITE**

C'est un outil de nouvelle génération pour construire le front, Outil créé par Evan You créateur de vue-js. Il dispose d'une interface de ligne de commande solide qui facilite considérablement le processus de configuration du projet. Il fonctionne très rapidement et possède de nombreuses fonctionnalités intéressantes fournies par CRA. De plus, il existe des plugins qui peuvent être ajoutés et rendent le processus de développement plus simple.
Lorsque CRA et Vite sont comparés, il existe de nombreuses différences en termes d'expérience et de temps de construction. Dans CRA, il n'y a littéralement pas de fichier de configuration pour le processus de construction, tout est super simple. Cela facilite la vie des petits projets, mais lorsque le projet prend de l'ampleur, le processus de construction doit être configuré en fonction des besoins du projet. En revanche, Vite fournit un fichier vite.config.js qui contient des configurations supplémentaires pour le projet et il est vraiment simple et facile à configurer.  

L'autre différence avec CRA est le temps du build de l'application pour le développement, CRA regroupe tous les fichiers de l'application à chaque changement alors que Vite ne regroupe que les fichiers modifiés, d'ou le gain de performance. 

En resumé pour les petits projets CRA peut être suffisant, en revanche utilisez vite si le projet prend de l'ampleur.

# Partie 2. Comment architecturer son projet React

### IHM de référence
![IHM](./IHM_My_business.png)

### Conception technique 

#### React router

Documentation : https://reactrouter.com/web/guides/quick-start

Dans le contexte de notre IHM, l'application globale aura un router qui redirigera vers chaque features et ensuite chaque feature gére son propre router.

**Exemple App.js**
```jsx
import { BrowserRouter as Router, Redirect, Route, Switch } from 'react-router-dom';
//Importer le composants nécessaires

const App =() => {
    return (
            <Router history={history}>                      
                    <Switch>
                        <Route path="/" component={Home} />
                        <Route path="/direct-order" component={DirectOrder} />
                        <Route path="/stock-reporting" component={StockReporting} />
                        <Route path="/reset-local-prices" component={ResetLocalPrices} />
                        <Route path="/fixture-catalog" component={FixtureCatalog} />
                        <Route path="/purchase-management" component={OrderValidation} />
                    </Switch>
            </Router>
           )
}              
```
**Exemple franchise.jsx feature**
```jsx
import { BrowserRouter as Router, Redirect, Route, Switch } from 'react-router-dom';
//Importer le composants nécessaires

const Franchise =() => {
    return (
            <Router history={history}>                      
                <Switch>
                    <Route path="/" component={Consolidations} />
                    <Route path="detailed-synthesis" component={ConsolidationDetails} />
                </Switch>
            </Router>
            )
}              
```

#### Gestion des états globaux

#### Installation de redux et redux-toolkit
Suivre la proccédure de cette documentation https://redux.js.org/introduction/getting-started

#### Schéma des multiples reducers de notre application
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

    //dispatch l'action updateUser qui va ensuite mettre à jour le store user de redux avec les données récoltées 

    useEffect(() => {
         dispatch(updateUser(getUser()));
    }, []);

    return <h1>{user?.name}</h1>
    }
 ```
 
### Découpage des composants
Découper l'IHM en composants permet de concéptualiser notre application à savoir les composants qui seront utilisables sur l'application globale ou uniquement par un composant plus restreint. 

A travers le schéma ci dessous, nous avons découpé les composants qui seraient utilisables dans un perimêtre global ou plutôt restreint. Plus on découpe mieux cela est pour la maintenabilité du code.

#### Schéma
![IHM](./out/listOfComponents/listOfComponents.svg)

#### Architecture fichier d'un composant
Prenons par exemple le composant Button qui est situé dans le perimêtre global, nous découperons le composant ainsi

    ├───Button
        ├───index.jsx
        ├───Button.css
        ├───Services.js
        ├───Services.test.js
        ├───Button.test.jsx
       
Si un composant posséde des composants réutilisables seulement dans ce composant, créer un dossier components dans le perimêtre associé du composant comme par **exemple**

    ───Button
        ├───index.jsx
        ├───Button.css
        ├───Services.js
        ├───Services.test.js
        ├───Button.test.jsx
        ├───Components
            ├───IconButton
                ├───index.jsx
                ├───IconButton.css
                ├───Services.js
                ├───Services.test.js
                ├───IconButton.test.jsx
### Gestion Requêtes Api
#### React-Query
Documentation => https://react-query.tanstack.com/overview

Cette bibliothéque  facilite la récupération, la mise en cache, la synchronisation et la mise à jour de l'état du serveur de nos applications React. Cela permet également d'éviter d'ajouter du code superflux.

Par exemple dans notre [IHM](#ihm-de-référence) nous avons besoin de créer un composant qui renverra une liste de consolidations. Ce composant peut être largement simplifié grace au hook react-query, ce qui simplifie la lisibilité du code en plus de tous les autres avantages liés à cette bibliothéque.

#### L'interêt factuel d'utiliser React-query
##### Composant Consolidations sans l'utilisation de react Query
```jsx
import React, {useEffetct, useState} from 'react'
import {fetchConsolidations } from 'services'

const Consolidations = () => {
    // C'est trois lignes peuvent être éco par react query
    const [data, setData] = useState()
    const [isLoading, setIsLoading] = useState(false)
    const [error, setError] = useState(false)

    // Cette fonction également
    const getConsolidations = async () => {
        setIsLoading(true)
         try {
            setData(await fetchConsolidations())
            setIsLoading(false)
        } catch (error) {
            setIsLoading(false)
            setError(true)
        }
    }

    // le useEffect également
    useEffect(() => {
        getConsolidations()
    }, []);

    if (isLoading) return "Loading...";
    if (error) return <div>Something went wrong.</div>
    return <div>data</div>
} 
```
##### Composant Consolidations avec l'utilisation de react Query
```jsx
import React, from 'react'
import { useQuery } from 'react-query'
import {fetchConsolidations } from 'services'

const Consolidations = () => {
    const { isLoading, error, data } = useQuery('consolidations', () =>fetchConsolidations())

    if (isLoading) return "Loading...";
    if (error) return <div>Something went wrong.</div>
    return <div>
             {data?.map((conso) => (
                <div key={conso}>{conso}</div>
               ))}
            </div>
} 
```

### Communication entre les composants
Plusieurs façons existent pour communiquer entre les composants

        - Props directs de parent à enfant 
        - En stockant les informations dans redux
        - En utilisant le hook useContext

Imaginons que la page d'accueil de l'application s'occupe de donner la couleur du bouton validation finale présent dans la features franchise, référence à notre [IHM](#ihm-de-référence)

### Props directs

Pour transmettre la proprieté à ce composant FinalValidationButton il faudrait que le app.js passe des props à admettons 4 enfants

```jsx
//le composant
<Franchise buttonColor="red"/>
//le composant
<FranchiseHome buttonColor="red"/>
//le composant
<Consolidations buttonColor="red"/>
//le composant
<FinalValidationButton buttonColor="red"/>
```
Cette méthode peut devenir vite fastidieuse et rendre notre application peu maintenable.

### Stocker la valeur dans redux

Cette méthode serait pour nous mettre en place une usine à gaz pour si peu. 

### UseContext

Cette méthode serait la mieux adaptée pour notre besoin. En effet elle permettra de fournir au composant qui est rééllement dans le besoin de consommer la valeur sans pour autant devoir transmettre la valeur à ses composants parents.
La bonne pratique serait de splitter les fichier de maniére suivante.

**ButtonColorContext.js**

```jsx
export const ButtonColorContext = React.createContext(null);
```

**App.jsx**

Dans notre cas le fait de fournir la valeur au composant franchise, tous ses enfants ont la possibilité de consommer la valeur au travers du hook useContext. Cela est bien pratique, pour éviter de passer des valeurs à des composants qui ne la consomme pas.
Cependant rien ne nous empéche de wrapper directement notre bouton final à l'intérieur du provider. Tout dépend du besoin de l'application.

```jsx
import{ ButtonColorContext } from "ButtonColorContext.jsx";

const App = () => {
  return (
      // on fournit la valeur au context
    <ButtonColorContext.Provider value="red">
      <Franchise />
    </ButtonColorContext.Provider>
  );
}
```

**FinalValidationButton.jsx**

```jsx
import { useContext } from 'React'

const FinalValidationButton = () => {
  // Avec ce hooks nous consommons la valeur de ce context, en l'occurence la couleur du bouton
  const color = useContext(ButtonColorContext);

  return (
    <button style={{color}}>Validation finale</button>
  );
}
```

### Architecture globale des fichiers de l'application
Si l'on se référe à notre [conception des états](#schéma-des-multiples-reducers-de-notre-application) et [l'inventaire de nos composants](#découpage-des-composants) nous aurions ce genre d'architecture fichiers. L'idée n'est pas de la détailler entiérement mais d'avoir un aperçu de structure de base.

    ───Application
        ├───node_modules
        ├───public
            ├───index.html
        ├───src
            ├───features
                ├───DirectOrder
                    ├───store
                        ├───slices
                    ├───components
                        ├───Component1
                        ├───Component2
                ├───StockReporing
                    ├───store
                        ├───slices
                    ├───components
                        ├───Component1
                        ├───Component2
                ├───FixtureCatalog
                    ├───store
                        ├───slices
                    ├───components
                        ├───Component1
                        ├───Component2
                ├───Franchise
                    ├───store
                        ├───slices
                            ConsolidationsSlice.js;
                            ConsolidationsDetailsSlice.js
                        reducer.js
                    ├───components
                        ├───Header
                        ├───Component2      
            ├───components
                ├───Header
                    ├───components
                        ├───User
                        ├───SearchBar
                        ├───Logo
                        ├───AppName
                ├───Table
                    ├───components
                        ├───TableHeader
                        ├───TableRow
                        ├───TableData
                        ├───TableFooter    
                ├───Button
                ├───SideBar
                    ├───components
                        ├───CustomLink
                ├───Select 
                ├───Checkbox
                ├───Pagination
            ├───store
                index.js
            App.js

###  Bonnes pratiques 

#### Séparer la logique du rendu visuel
Il est primordial de séparer la logique de l'UI dans un composant afin de faciliter la maintenabilité et aussi pouvoir eventuellement réutiliser la même logique dans d'autres composants au travers de customHooks.

Supposons pour notre cas d'exemple que nous avons notre composant Consolidations qui affiche une liste de consolidations.

Pour ce faire nous aurons besoin de récupérer les données via une api tierce. Pour ajouter un élement logique à notre problématique, admettons que nous devons ajouter à chaque consolidation une donnée comme isChecked par exemple. 

Nous aurons donc un fichier Consolidations.jsx , un custom hook useConsolidations qui renverra et traitera les données et pour finir un customHook useFetch qui nous permettra de réaliser nos requêtes vers l'api.

Supposons que nous utilisons pas le hook fournit par [REACT-QUERY](#composant-consolidations-sans-l'utilisation-de-react-query) et que nous souhaitons en créer un nous même. 

**custom hook UseFetch.js**
```jsx 
export const useFetch = (url, options) => {
  const [data, setData] = React.useState();
  const [hasError, setHasError] = React.useState();
  const [isLoading, setIsLoading] = React.useState();

  React.useEffect(async () => {
      try {
            setIsLoading(true)
            const res = await fetch(url, options);
            const json = await res.json()  
            setData(json);
            setIsLoading(false)
          } catch (error) {
            setIsLoading(true)
            setHasError(error)
  });

  return {data, isLoading, hasError};
};
```
**Custom hook UseConsolidations.js**

Ce qui nous permettra de récupérer de consolidations également dans d'autres composants peut importe l'UI et de récupérer la même logique de  traitement de données.

```jsx 
import {useFetch} from "UseFetch"
import {fakeAggregateData} from "fakeAggregateData"

export const useConsolidations = () => {
  const {data, isLoading, hasError} = useFetch("url/conolidations",{...options});
  const [consolidations, setConsolidations] = useState();

  React.useEffect(async () => {
  // quand data change on aggrége les données avec isChecked, 
  // ceci est un traitement simulant une logique...
      setConsolidations(fakeAggregateData(data))
  },[data]);

  return {consolidations, isLoading, hasError};
};
```

**Composant Consolidations.jsx qui contient que l'UI**

```jsx 
import {useFetch} from "UseFetch"
import {useConsolidations} from "useConsolidations"

export const Consolidations = () => {
  const {consolidations, isLoading, hasError} = useConsolidations();
 
  if (isLoading) return <div>Is loadging</div>
  if(hasError) return <div>hasError</div>
  
  return <div>
    {consolidations?.map(conso =>
         (<div key={conso}>{conso}</div>)
    )}
 </div> 
};
```










#### Evitez les excès de commentaires
Il est parfois nécessaire d’expliquer du code complexe par un commentaire afin de faciliter la compréhension du code.

Cependant l’excès de commentaire peut avoir l’effet totalement inverse. Premièrement, cela va diminuer la visibilité du code, rendant la lecture de celui-ci plus compliqué et plus longue.

Mais surtout cela peut introduire de l’incompréhension et entrainer une perte de temps importante. Effectivement, si les commentaires n’ont pas évolué en même temps que la modifications du code, ou gestion de bug, ils deviennent contre-productifs, apportant des explications qui ne sont plus d'actualité.

Pour pallier à ce problème, il est conseillé de limiter au maximum le nombre de commentaires. Et il est préférable de mettre en place des régles de convention de nommage.

#### Convention de nommage
Il est primordial de définir avec son équipe une convention de nommage et de la respecter. Rédiger ensemble des règles sur le nommage que ce soit pour les fonctions les composants et les variables. Cela permettra une homogénéisation du code ; ce qui permet à toute l’équipe de gagner en rapidité de lecture et faciliter la compréhension du code dans sa globalité.

**Exemple de convention**

        -Les variables du type Boolean et les fonctions retournant un Boolean doivent commencer par ‘is’,’has’ ou ‘should’.
        -Utilisez l’extension .jsx pour les composants React. (.tsx dans un contexte TypeScript)
        -Utilisez PascalCase pour les noms de fichiers (par exemple, LoginComponent.jsx).
        -Utilisez le nom de fichier comme nom de composant.
        -Les gestionnaires d’événements doivent commencer par ‘handle’ (par exemple, handleSubmit).
        -Les fonctions doivent être nommées pour leur but, et non pas comment elle le realise. Parce que la maniére dont vous réalisez le traitement peut évoluer au cours du temps, et vous ne devriez pas avoir besoin de modifier toutes les références de ces fonctions dans votre code à cause de cela.

#### Principe de responsabilité unique 
En programmation orientée objet, Robert C. Martin exprime le principe de responsabilité unique comme suit : une classe ne doit changer que pour une seule raison. Source: https://fr.wikipedia.org/wiki/Principe_de_responsabilit%C3%A9_unique

Prenons l'exemple d'un module qui compile et imprime un rapport. Imaginons que ce module peut changer pour deux raisons. D'abord, le contenu du rapport peut changer. Ensuite, le format du rapport peut changer. Ces deux choses changent pour des causes différentes; l'une substantielle, et l'autre cosmétique. Le principe de responsabilité unique dit que ces deux aspects du problème ont deux responsabilités distinctes, et devraient donc être dans des classes ou des modules séparés.

On peut transposer ça avec nos composants ou nos hooks ou nos fonctions. Il s'occupe d'une tâche bien précise ou affiche un élement bien précis, cela permet de maintenir le code beaucoup plus aisaiement et également débugger beaucoup plus facilement. Assurez vous que chaque fonction répond à un besoin précis et correctement.


#### Destructuration d'élements
ES6 a introduit le concept de déstructuration. La déstructuration vous permet de destrcuturer les propriétés d’un objet ou des éléments d’un tableau et les récupérer directement sous forme de variables assignées.

Cela permet également d’éviter la répétition très courante de props.value ou etc...


**Exemple**
```jsx
// Supposons que l'on récupére ces props
const props = {isLoading: true, data: "data", hasError:"hasError"}
// destructuration
const {isLoading, data, hasError} = props;
```
#### Utilisation des propTypes
Les propTypes et defaultProps sont des propriétés statiques, déclarées aussi haut que possible dans le code du composant. Elles devraient être immédiatement visibles par les autres développeurs qui lisent le fichier, car elles servent de documentation pour votre code.

Tous vos composants doivent avoir des propTypes dès qu’il possède des props.

Les propTypes permettent de déclarer les différents types des paramètres fournit à votre composant. Si un des types n’est pas respecté, un avertissement dans la console de votre navigateur sera ajouté. Cela permet de prévenir les erreurs dans votre code et de spécifier les types attendus de vos variables.

**Exemple**
```jsx 
import PropTypes from 'prop-types';
 
const Button = ({color}) => (<button>{color}</button>);

Button.propTypes = {
  color: PropTypes.string
};
```


#### Utilisation des conditions de rendu 

**Conditions dans le JSX**

Les valeurs false, null, undefined et true sont des enfants valides pour React. Or ils ne s’affichent pas dans le rendu. Ce qui permet de s’en servir de condition pour rendre conditionnel l'affichage de certains composants.

```jsx
const Component = () => (
    <div>
        {isLoading && !user && <Loader />}
        {!isLoading && user && <User/>}
    </div>
)
```
**Conditions ternaires**

Les conditions ternaires imbriquées ne sont généralement pas lisibles. Plus les conditions sont complexes plus la lecture est difficile. Juste pour un simple gain de syntaxe ou de place il est déconseillé de sacrifier la lisibilité.

 **Exemple**
```jsx
//Mauvaise pratique
const Component = () => {
    const [user, setUser] = useState({ isLoggedIn: true, hasEmail: true });

    return (
        <div>
            {user ? user.isLoggedIn ? <GreatingsUser/> : user.hasEmail? <GreatingsEmail/> :
                <GreatingsGuest /> : null }
        </div>
    )
}
```

```jsx
//Bonne pratique + de lignes de code mais plus facilement lisable
const Component = () => {
    const [user, setUser] = useState({ isLoggedIn: true, hasEmail: true });
    const {isLoggedIn, hasEmail} = user

    const renderUser = () => {
        if (isLoggedIn) {
            return <h1>GreatingUser</h1>;
        } else if (hasEmail) {
            return <h1>GreatingsEmail</h1>;
        } else {
            return <h1>GreatingsGuest</h1>;
        }
    };

    return <div>{renderUser()}</div>;
}

```

#### Gestion des classes CSS

**classnames**

classnames est un trés bon package pour générer des noms de classes de composants et ainsi gérer les styles dynamiques. En pratique, il existe de nombreux cas où différents styles doivent être appliqués au même composant. Pour éviter l'accumulation de conditions dans votre code, qui réduisent considérablement la lisibilité, nous pouvons préparer les noms de classe à l’aide de ce package. 

Documentation => https://www.npmjs.com/package/classnames

**Exemple**
```jsx
import classNames from 'classnames';

const btnClass = classNames('btn', {
      'btn-pressed': isPressed,
      'btn-over': !isPressed && isHovered
    });

return <button className={btnClass}>{label}</button>;
```

**styled-components**

Documentation => https://styled-components.com/docs/basics#getting-started

La force de cette librairie est de faciliter la création de composants visuels minimalistes et configurables, en combinant du CSS standard et un peu de JavaScript. Ces composants deviendront littéralement des pièces d'assemblage qui pourront être utilisées et partagées entre les UIs de toutes vos applications.

Avec styled, on peut isoler le CSS et le DOM de la vue, ce qui va simplifier amplement le code relatif à la partie "métier" de l'application, et améliorer la lisibilité.

**Solution en pure CSS-in-JS**

```jsx
const styles = {
  box: {
    width: '60%',
    border: '1px solid silver'
  },
  title: {
    fontSize: '1.2em',
    fontWeight: 'bold'
  },
  description: {
    fontSize: '0.8em'
  },
  important: {
    fontWeight: 'bold'
  }
}

const Component = () => (
    // notre composant intègre le style directement dans le jsx
    <div className={ styles.box }>
        <div className={ styles.title }>{ title }</div>
        <div className={ classnames(styles.description, styles.important) }>{ description }</div>
    <div/>  
)
```

**Solution avec styled-component**

Cette solution est parfaite pour adapter et préstyler les éléments que ce soit les containers, les views, les composants de maniére isolée en pouvant les rééutiliser dans d'autres endroits de l'application sans pour autant utiliser les classes css. 

```jsx
import styled from "styled-components";

const Box = styled.div`
  width: 60%;
  border: 1px solid silver;
`
const Title = styled.div`
  font-size: 1.2em;
  font-weight: bold;
`
const Description = styled.div`
  font-size: 0.8em;
  font-weight: ${ props => props.important ? 'bold' : 'normal '}
`

const Component = () => (
    <Box>
        <Title>{ title }</Title>
        <Description important>{ description }</Description>
    </Box>
)

```


#### Utilisation d'un linter
Utiliser un linter permet d'analyser statiquement du code et vérifie que celui-ci respecte un certain nombre de standard.  Cela assure d'être constant concernant le respect des normes et également d'être à jour réguliérement sans effort particulier sur les bonnes pratiques de développement concernant React-js ou autres languages d'ailleur. En effet, les mises à jour du Linter prennent en considération les évolutions des bonnes pratiques de développement.

Cet outil est très simple à mettre en place et assure une bonne qualité de votre code. Un outil indispensable ! 

Documentation => EsLint (https://eslint.org/), TsLint (https://palantir.github.io/tslint/)

###  Librairies pratiques

#### Frameworks basés sur React
[NEXT.JS](https://nextjs.org/)

[GATSBY.JS](https://www.gatsbyjs.com/)

#### Gestion d'états
[REDUX](https://redux.js.org/)

#### Formulaires
[FORMIK](https://formik.org/)

[REACT-HOOK-FORM](https://react-hook-form.com/)

#### Tableaux
[REACT-TABLE](https://react-table.tanstack.com/)

#### Requête api
[REACT-QUERY](https://react-query.tanstack.com/)

#### Tests
[REACT-TESTING-LIBRARY](https://testing-library.com/docs/react-testing-library/intro/)

[JEST](https://jestjs.io/docs/getting-started)


#### UI
[REACT-BOOTSTRAP](https://react-bootstrap.github.io/)

[MATERIAL-UI](https://mui.com/getting-started/usage/)

[ANTD](https://ant.design/)



