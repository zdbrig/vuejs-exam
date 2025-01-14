# Évaluation Avancée Vue.js

Ce document contient une liste de questions avancées pour évaluer un candidat expérimenté sur Vue.js, suivies de leur réponse.

---

## Question 1
**Dans Vue 3, quelle est la principale différence entre l’API Options (Options API) et l’API Composition (Composition API) ?**

**Réponse :**  
- L’API Options (Options API) est basée sur la déclaration d’options comme `data`, `computed`, `methods`, etc. au sein d’un objet.  
- L’API Composition (Composition API) utilise des fonctions comme `setup` et divers "composables" (`ref`, `reactive`, `computed`, etc.) pour regrouper la logique par fonctionnalités plutôt que par type d’options.  
- La Composition API facilite la réutilisation de la logique à travers des fonctions réutilisables et offre une meilleure organisation du code dans des applications complexes.

---

## Question 2
**Comment gérer la réactivité avancée (par exemple pour des objets ou des tableaux profonds) dans la Composition API, et pourquoi pourrait-on parfois préférer `ref` plutôt que `reactive` ?**

**Réponse :**  
- `reactive` crée un objet réactif profond, c’est-à-dire que toutes les propriétés internes sont rendues réactives. Cela peut être pratique, mais aussi coûteux si l’objet est très complexe.  
- `ref` crée une réactivité uniquement sur la valeur stockée. Pour des données complexes ou si l’on souhaite un contrôle granulaire sur la réactivité (ou faire un "shallow" re-render), on peut préférer `ref`.  
- Parfois, l’utilisation de `ref` pour des données complexes (sous forme d’objets ou tableaux) permet de décider à quel moment déclencher la réactivité sur chaque propriété en la manipulant manuellement.

---

## Question 3
**Pouvez-vous expliquer comment fonctionnent les watchers dans la Composition API, notamment la différence entre `watch` et `watchEffect` ?**

**Réponse :**  
- `watch(source, callback)` observe des sources réactives spécifiques (une ou plusieurs) et exécute une fonction de rappel quand ces sources changent. On peut aussi configurer des options comme `deep` ou `immediate`.  
- `watchEffect` exécute automatiquement une fonction et réagit à toutes les dépendances réactives qu’elle utilise, sans qu’il soit nécessaire de les déclarer explicitement.  
- `watchEffect` est idéal pour des effets parallèles (effets de bord) qui doivent réagir à tout changement de dépendance dans la fonction, alors que `watch` est plus précis quand on veut observer spécifiquement certains états.

---

## Question 4
**Dans le contexte de l’optimisation des performances, que fait l’option `v-once` et comment l’utiliser correctement ?**

**Réponse :**  
- La directive `v-once` indique à Vue de rendre l’élément et de ne plus ré-exécuter son rendu ou ses calculs de réactivité par la suite.  
- Elle est utile lorsqu’on sait qu’un bloc d’interface ne changera jamais après le rendu initial.  
- Pour l’utiliser, on ajoute `v-once` sur l’élément ou le composant concerné, par exemple :  
  ```html
  <div v-once>{{ valeurInchangeable }}</div>
