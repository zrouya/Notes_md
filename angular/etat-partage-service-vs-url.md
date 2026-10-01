---
tags: [angular, state-management, services, routing]
---

# État partagé : service ou URL ?

Un état d'interface (panneau ouvert, onglet, filtre) peut être mémorisé dans un **[[services-angular|service]] singleton** avec des [[angular-signals|signaux]], ou dans l'**URL** (paramètre, query param, [[routeroutlet-nommes|outlet nommé]]).

## Service avec signaux

```ts
@Injectable({ providedIn: 'root' })
export class ChatPanelService {
  private readonly conversationId = signal<number | null>(null);
  readonly currentId = this.conversationId.asReadonly();
  readonly isOpen = computed(() => this.conversationId() !== null);

  open(id: number) { this.conversationId.set(id); }
  close()          { this.conversationId.set(null); }
}
```

```html
<!-- layout -->
@if (chat.isOpen()) {
  <app-chat-panel [conversationId]="chat.currentId()!" (closed)="chat.close()" />
}
```

N'importe quel composant fait `inject(ChatPanelService).open(7)` : l'émetteur et le layout **ne se connaissent pas**, ils communiquent via le service.

## Query param (compromis)

```ts
this.router.navigate([], { queryParams: { chat: 7 }, queryParamsHandling: 'merge' });
chatId = toSignal(inject(ActivatedRoute).queryParamMap.pipe(map(p => p.get('chat'))));
```

## Comparatif

| | Service | URL |
|---|---|---|
| Survit à F5 | ❌ | ✅ |
| Lien partageable | ❌ | ✅ |
| Précédent/Suivant | ❌ | ✅ |
| Simplicité | ✅ | Moyenne |

**Règle** : si l'utilisateur doit pouvoir **partager ou retrouver** l'affichage → URL. État éphémère (modale, menu, panneau) → service.

Pour un état partagé volumineux : NgRx / NgRx SignalStore (même principe, plus structuré).

## Voir aussi

- [[services-angular]]
- [[parametres-de-route-angular]]
- [[state-managment-angular]]
