
Les Pipes Angular permettent de formatter les [[Data Binding Angular|property bindings]].
Ils faut pour cela importer au sein du composant le pipe correspondant.

Par exemple, pour un pipe de mise en forme des dates :

```html
<time>{{ registrationDate | date:'fullDate'}}</time> <!-- options possibles après le ':' -->
```

```typescript
import { Component } from '@angular/core'
import { DatePipe } from '@angular/common'

@Component({
	selector: 'date-component',
	imports: [DatePipe]
})
public class DateComponent {
	registrationDate: string;
}
```
