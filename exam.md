# Questions d'entretien Vue.js - Niveau Avancé

## Niveau Débutant/Intermédiaire

### Q1: Quelle est la différence entre v-show et v-if ?
**Réponse:** 
- v-if supprime/ajoute réellement l'élément du DOM
- v-show cache l'élément avec display: none
- v-show a de meilleures performances pour les toggles fréquents
- v-if est meilleur pour les conditions qui changent rarement

### Q2: Expliquez le cycle de vie d'un composant Vue.js
**Réponse:**
1. beforeCreate: Instance initialisée
2. created: Données réactives configurées
3. beforeMount: Template compilé
4. mounted: DOM inséré
5. beforeUpdate: Données changées, avant mise à jour DOM
6. updated: DOM mis à jour
7. beforeDestroy: Avant destruction
8. destroyed: Instance détruite

## Niveau Intermédiaire

### Q3: Comment fonctionne la réactivité dans Vue 3 ?
**Réponse:**
Vue 3 utilise Proxy pour la réactivité, contrairement à Vue 2 qui utilisait Object.defineProperty.
```javascript
const proxy = new Proxy(target, {
  get(target, key) {
    track(target, key)
    return target[key]
  },
  set(target, key, value) {
    target[key] = value
    trigger(target, key)
    return true
  }
})
```

### Q4: Expliquez la différence entre Composition API et Options API
**Réponse:**
- Options API organise le code par options (data, methods, computed)
- Composition API organise le code par logique fonctionnelle
- Composition API offre une meilleure réutilisation du code
- Meilleure inférence TypeScript avec Composition API

## Niveau Avancé

### Q5: Comment optimiser les performances d'une liste avec v-for ?
**Réponse:**
1. Toujours utiliser key avec v-for
2. Éviter v-if avec v-for
3. Utiliser computed pour filtrer/trier les listes
4. Utiliser virtual scrolling pour les grandes listes
```javascript
computed: {
  filteredList() {
    return this.items.filter(item => item.active)
  }
}
```

### Q6: Expliquez le fonctionnement des watchers profonds et leur impact sur les performances
**Réponse:**
```javascript
watch: {
  obj: {
    deep: true,
    immediate: true,
    handler(newVal, oldVal) {
      // Coûteux en performances car surveille toutes les propriétés imbriquées
    }
  }
}
```
Alternative optimisée:
```javascript
watch: {
  'obj.specificProp'(newVal) {
    // Surveille uniquement une propriété spécifique
  }
}
```

### Q7: Comment implémenter un système de plugins dans Vue.js ?
**Réponse:**
```javascript
const MyPlugin = {
  install(app, options) {
    // Ajouter une propriété globale
    app.config.globalProperties.$myMethod = () => {}
    
    // Ajouter une directive
    app.directive('my-directive', {})
    
    // Ajouter un mixin
    app.mixin({})
    
    // Ajouter un composant
    app.component('my-component', {})
  }
}
```

## Niveau Expert

### Q8: Comment créer un composant de rendu personnalisé (render function) ?
**Réponse:**
```javascript
export default {
  props: ['level'],
  render() {
    const tag = `h${this.level}`
    return h(tag, {}, this.$slots.default())
  }
}
```

### Q9: Expliquez comment fonctionne la compilation des templates Vue.js
**Réponse:**
1. Parse le template en AST (Abstract Syntax Tree)
2. Transforme l'AST (optimisations, directives)
3. Génère le code render function
4. Crée le Virtual DOM
5. Applique les différences au DOM réel

### Q10: Comment implémenter un système de state management personnalisé avec l'API Composition ?
**Réponse:**
```javascript
import { reactive, readonly } from 'vue'

export function createStore(options) {
  const state = reactive(options.state)
  
  const store = {
    state: readonly(state),
    dispatch(action, payload) {
      options.actions[action]?.call(this, {
        state,
        dispatch: this.dispatch,
        commit: this.commit
      }, payload)
    },
    commit(mutation, payload) {
      options.mutations[mutation]?.call(this, state, payload)
    }
  }
  
  return store
}
```
