---
tags: [dotnet, runtime, profiling, agent, instrumentation]
---

# CLR Profiler API (.NET)

API native du runtime .NET (CLR) permettant à un agent externe de s'enregistrer et de recevoir des événements du processus — sans que le code applicatif ne le sache.

## Activation

3 variables d'environnement suffisent, lues par le CLR au démarrage :

```bash
CORECLR_ENABLE_PROFILING=1
CORECLR_PROFILER={guid-de-l-agent}
CORECLR_PROFILER_PATH=/path/to/agent.so   # .so sur Linux, .dll sur Windows
```

## Ce que l'agent peut observer

Via l'interface `ICorProfilerCallback`, l'agent reçoit des hooks sur :
- Entrée/sortie de chaque méthode (JIT hooks)
- Exceptions levées ou catchées
- Allocations mémoire
- Chargement d'assemblies

## Points clés

- L'agent est chargé **dans le même processus** (shared library), pas via IPC
- Fonctionne sur Linux (`.so`) et Windows (`.dll`)
- C'est la base de l'auto-instrumentation AppInsights et des profilers .NET (dotTrace, etc.)
- Nécessite que le container démarre avec ces variables d'env — Azure Container Apps les injecte automatiquement en mode codeless

## Voir aussi

- [[appinsights-auto-instrumentation]]
- [[java-agent]]
