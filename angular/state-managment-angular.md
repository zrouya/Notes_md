---
tags: [angular, state-management, signals]
---

# State management Angular

Comment Angular sait qu'il doit mettre à jour le DOM quand les données changent.

## Zone.js (historique)

Zone.js intercepte tous les événements asynchrones (clics, `setTimeout`, requêtes HTTP…). Après chacun, Angular considère que quelque chose a *peut-être* changé et **revérifie tout l'arbre de composants**.

- Imprécis : vérifie aussi les composants inchangés
- « Magique » : on ne voit pas ce qui déclenche une mise à jour

## Signals (Angular 16+, stable en 17)

Avec les [[angular-signals|Signals]], la réactivité est **explicite et fine** : Angular sait quel composant dépend de quelle donnée et ne met à jour que ceux-là.

C'est ce qui permet le mode **zoneless** (sans zone.js), par défaut dans les nouveaux projets récents :

```ts
bootstrapApplication(AppComponent, {
  providers: [provideZonelessChangeDetection()]
});
```

## État partagé entre composants

- [[services-angular|Service]] singleton + signaux → [[etat-partage-service-vs-url]]
- Bibliothèques dédiées pour un état volumineux : NgRx, NgRx SignalStore

## Voir aussi

- [[angular-signals]]
- [[signals-vs-rxjs]]
- [[composants-angular-donnees-dynamiques]]
