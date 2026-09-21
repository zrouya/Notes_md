
Angular permet de créer des [[Composants Angular|composants]] servant à wrapper du contenu (fonctionnalité content projection), grâce au composant ``ng-content`` : 

```html
<div>
	<!-- some template elements -->
	<ng-content /> <!-- cet élément sera remplacé par le template injecté par le composant parent -->
</div>
```

Composant parent : 
```html
<app-card>
	<!-- le template placé ici sera injecté à la place du composant ng-content -->
</app-card>
```