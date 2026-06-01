---
tags: [java, jvm, agent, instrumentation, profiling]
---

# Java Agent (JVM Instrumentation API)

Mécanisme officiel de la JVM permettant d'injecter du code dans un processus Java au démarrage, via réécriture de bytecode — sans modifier le code source.

## Activation

```bash
java -javaagent:/path/to/agent.jar -jar app.jar
```

Pour AppInsights :
```bash
java -javaagent:/agents/applicationinsights-agent.jar -jar app.jar
```

## Mécanisme

1. La JVM charge le `.jar` agent avant le code applicatif
2. L'agent s'enregistre via `java.lang.instrument.Instrumentation`
3. Il reçoit un hook sur chaque classe chargée → peut **réécrire le bytecode** à la volée
4. Résultat : du code de monitoring est inséré dans les méthodes sans toucher au source

## Ce que ça permet

- Intercepter tous les appels HTTP (entrée/sortie)
- Tracer les appels SQL, Redis, etc.
- Capturer les exceptions
- Mesurer les temps d'exécution méthode par méthode

## Points clés

- Fonctionne **dans le même processus JVM**, pas via socket ou IPC
- Pas de recompilation nécessaire — réécriture bytecode à chaud
- Utilisé par AppInsights Java, Datadog, New Relic, OpenTelemetry Java agent, etc.
- Azure Container Apps peut injecter le jar automatiquement en mode codeless

## Voir aussi

- [[appinsights-auto-instrumentation]]
- [[clr-profiler]]
