# TP - Todo list

## Sommaire

- [Sommaire](#sommaire)
- [Mise en place](#mise-en-place)
- [Présentation du Sujet](#présentation-du-sujet)
- [Création du formulaire](#création-du-formulaire)
- [Affichage des tâches](#affichage-des-tâches)
- [Suppresion des tâches](#suppresion-des-tâches)
- [Composant Todo](#composant-todo)
- [Bonus : Marquer une tâche comme complétée](#bonus--marquer-une-tâche-comme-complétée)
- [Pour aller plus loin](#pour-aller-plus-loin)

## Mise en place

Pour commencer, clonez ce repository sur votre ordinateur ou téléchargez le zip.

Assurez-vous d'avoir NodeJs et npm (ou un autre package manager) d'installé.

Ouvrez un terminal dans le répertoire du tp et installez les dépendances en fonction de votre package manager :

```shell
# Avec npm
npm install

# Avec pnpm
pnpm install

# Avec yarn
yarn install
```

Démarez l'application avec :

```shell
npm run dev
```

Et ouvrez l'url [http://localhost:5173/](http://localhost:5173/) dans votre navigateur.

Enfin, ouvrez le dossier dans votre éditeur de code favoris.

## Présentation du Sujet

Vous allez, à travers ce TP, créer une application web de type "Todo list" (liste de tâches).

Nous allons donc implémenter étapes par étape :

- Formulaire pour créer une tâche
- Stockage et affichage des tâches
- Suppression d'une tâche
- Ajout d'un système de complétion de tâche

Vous pouvez vous aider du cours de la diapo : [presentation.pdf](./presentation.pdf).

À chaque fin de section, vous trouverez un lien vers la solution si vous bloquez trop longtemps.

## Création du formulaire

Ouvrez le fichier [App.vue](./src/App.vue).

Dans la balise `<template>`, ajoutez un élément `<form>` avec un `<input>` de type `text` et un `<button>` pour ajouter le todo.

Dans la balise `<script>`, créez une ref `newTodo` pour contenir la nouvelle tâche.

Pour lier cette ref au champ de saisie, utilisez la directive `v-model` sur l'élément `<input>`.

> Nous pourrions utiliser les attributs `value` et `@input` pour faire cette liaison, mais `v-model` est plus simple et concis.

Ensuite, de retour dans la balise `<script>`, créez une ref pour contenir les tâches.

> Ici, nous n'utilisons pas l'API `reactive` malgrès que l'on stock une liste d'éléments car nous aurons besoin de remplacer cette liste par une nouvelle liste lors de la suppression d'une tâche.

Créez une fonction `addTodo` qui ajoute une nouvelle tâche à la liste (si le champ de saisie n'est pas vide).  
Nous représenterons une tâche par un objet avec les propriétés suivantes pour l'instant :

- `id` : un identifiant unique (pour le générer utilisez [cet exemple](https://github.com/leo-jcq/tp-todo-app/blob/solutions/id-example.vue))
- `text` : le texte de la tâche

> N'oubliez pas de réinitialiser la valeur du champ de saisie après l'ajout.

Enfin, pour que la fonction `addTodo` soit appelée lors de la soumission du formulaire, ajoutez un écouteur d'événement `submit` sur le formulaire (n'oubliez pas d'empêcher le comportement par défaut du formulaire).

Solution : [https://github.com/leo-jcq/tp-todo-app/blob/solutions/form.vue](https://github.com/leo-jcq/tp-todo-app/blob/solutions/form.vue).

## Affichage des tâches

Pour afficher les tâches, créez une balise `<ul>` dans le template, et utilisez une directive `v-for` pour afficher chaque tâche dans un élément `<li>` (faites la boucle sur `todos` et non `todos.value` car Vue désctructure automatiquement les refs dans le template).

> N'oublez pas d'utiliser la directive `:key` avec l'identifiant unique de chaque tâche. Cela permet à Vue de savoir quel élément a changé, a été ajouté ou supprimé.

Vous pouvez maintenant tester l'application en ajoutant des tâches via le formulaire.

Pour finir, ajoutez un ajoutez un `<li>` qui s'affiche uniquement lorsque la liste des tâches est vide, avec le texte "Aucune tâche à afficher" (utilisez l'attribut `v-if`).

Solution : [https://github.com/leo-jcq/tp-todo-app/blob/solutions/todo-display.vue](https://github.com/leo-jcq/tp-todo-app/blob/solutions/todo-display.vue).

## Suppresion des tâches

Nous allons maintenant ajouter la possibilité de supprimer une tâche.

Pour cela, ajoutez un bouton "Supprimer" à côté de chaque tâche dans la liste.

Ensuite, créez une fonction `removeTodo` qui prend en paramètre l'identifiant d'une tâche et qui la supprime de la liste des tâches.

> Pour cela, on peut utiliser la méthode `filter` de la classe `Array` : [https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/filter](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

Enfin, ajoutez un écouteur d'événement `click` sur le bouton "Supprimer" pour appeler la fonction `removeTodo` avec l'identifiant de la tâche correspondante.

Solution : [https://github.com/leo-jcq/tp-todo-app/blob/solutions/remove-todo.vue](https://github.com/leo-jcq/tp-todo-app/blob/solutions/remove-todo.vue)

## Composant Todo

Notre application fonctionne, mais il y a beaucoup de code au même endroit. Nous allons simplifier cela en créant un composant `Todo` pour représenter une tâche.

Créez un nouveau fichier dans `src/components/Todo.vue`.

Initialisez le composant avec les balises `<script setup>` et `<template>`.

Déplacez le code HTML représentant une tâche (l'élément `<li>`) dans le template du composant `Todo` (pensez à retirer la directive `v-for` et l'attribut `:key`).

Dans la balise `<script>`, déclarez un prop `todo` de type `Object` (ne pas oublier de le mettre obligatoire) avec la fonction `defineProps` pour recevoir les données de la tâche.

Retournez dans [App.vue](./src/App.vue) et importez le composant `Todo` dans la balise `<script>`.

```js
import Todo from "./components/Todo.vue";
```

Puis, utilisez ce composant dans le template à la place de l'élément `<li>`, en lui passant la tâche en prop.

Il nous faut maintenant gérer la suppression des tâches depuis le composant `Todo`.

Pour cela, dans la balise `<script>` de [Todo.vue](./components/Todo.vue), déclarez un émetteur d'événement `remove` en utilisant la fonction `defineEmits`.

Modifiez le bouton "Supprimer" pour qu'il émette l'événement `remove` avec l'identifiant de la tâche lorsqu'il est cliqué.

> Pour utiliser un emit dans le template, on utilise la fonction `$emit` en lui passant le nom de l'événement souhaité, puis les paramètres.

Retournez dans [App.vue](./src/App.vue) et ajouter un écouteur d'événement `@remove` sur le composant `Todo` en passant la fonction `removeTodo`.

> N'appelez pas la fonction directement, passez-la simplement comme référence.

Vérifiez que tout fonctionne comme avant (ajout, suppression, ...), mais avec un code mieux organisé !

Solutions :

- Composant App : [https://github.com/leo-jcq/tp-todo-app/blob/solutions/component-App.vue](https://github.com/leo-jcq/tp-todo-app/blob/solutions/component-App.vue)
- Composant Todo : [https://github.com/leo-jcq/tp-todo-app/blob/solutions/component-Todo.vue](https://github.com/leo-jcq/tp-todo-app/blob/solutions/component-Todo.vue)

## Bonus : Marquer une tâche comme complétée

Nous allons maintenant ajouter la possibilité de marquer une tâche comme complétée.

Pour cela, dans [App.vue](./src/App.vue), modifiez la fonction `addTodo` pour ajouter une propriété `done` initialisée à `false` à chaque tâche.

Ensuite, dans [Todo.vue](./src/components/Todo.vue), ajoutez une case à cocher (`<input type="checkbox">`) à côté du texte de la tâche et liez sa propriété `checked` à la propriété `done` de la tâche.

Définissez un emit `done` dans la balise `<script>` qui sera utilisable pour notifier le composant parent lorsque l'état de complétion change.

> Ici, nous passons par un évènement et non par une modification directe de l'objet `todo` dans le composant `Todo` car les props sont en lecture seule.

Ajoutez un écouteur d'événement `@change` sur la case à cocher pour émettre l'événement `done` avec l'identifiant de la tâche.

Dans le composant [App.vue](./src/App.vue), créez une fonction `toggleDone` qui prend en paramètre l'identifiant d'une tâche, récupère cette tâche dans la liste (utiliser la méthode `find` de la classe `Array` : [https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/find](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/find)), et inverse la valeur de la propriété `done` de cette tâche.

Passez ensuite cette fonction à l'écouteur d'événement `@done` du composant `Todo`.

Pour quelle tâche soit marquée comme complétée dans l'interface, ajoutez la class conditionnelle `done` sur l'élément `<li>` de la tâche dans [Todo.vue](./src/components/Todo.vue) qui applique un style différent lorsque la propriété `done` est `true` (voir [https://vuejs.org/guide/essentials/class-and-style#binding-to-objects](https://vuejs.org/guide/essentials/class-and-style#binding-to-objects)).

Ensuite, nous pouvons décider d'afficher uniquement les tâches non complétées ou toutes les tâches dans la liste.

Pour cela, dans [App.vue](./src/App.vue), créez une ref `showCompleted` initialisée à `false` pour savoir si l'on doit afficher les tâches complétées ou non.

Ajouter ensuite un bouton dans le `<template>` pour basculer la valeur de `showCompleted` entre `true` et `false` lors d'un clic sur celui ci.  
Le texte du bouton doit également changer en fonction de la valeur de `showCompleted` (par exemple "Masquer les tâches complétées" si `showCompleted` est `true`, et "Afficher les tâches complétées" sinon).

Créez une propriété calculée `filteredTodos` qui retourne la liste des tâches en fonction de la valeur de `showCompleted` (si `false`, ne retourner que les tâches non complétées, sinon retourner toutes les tâches).

> Pour cela, vous pouvez utiliser la méthode `filter` de la classe `Array`.

> Ici l'utilisation d'une propriété calculée permet de recalculer la liste filtrée que lorsque des utiles au calcul changent (comme `tasks` ou `showCompleted`).

Enfin, utilisez cette propriété calculée `filteredTodos` dans le template pour afficher les tâches au lieu de la liste complète des tâches.

Solutions :

- Composant App : [https://github.com/leo-jcq/tp-todo-app/blob/solutions/done-App.vue](https://github.com/leo-jcq/tp-todo-app/blob/solutions/done-App.vue)
- Composant Todo : [https://github.com/leo-jcq/tp-todo-app/blob/solutions/dont-Todo.vue](https://github.com/leo-jcq/tp-todo-app/blob/solutions/dont-Todo.vue)

## Pour aller plus loin

Merci d'avoir suivi ce TP jusqu'au bout !

Voici quelques idées pour aller plus loin avec cette application :

- Mettre les tâches complétées en bas de la liste
- Enregistrer les tâches dans le localStorage du navigateur pour les conserver entre les sessions
- Ajouter la possibilité d'éditer le texte d'une tâche
- Autre chose de votre choix !
