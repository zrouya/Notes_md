---
tags: [observability, opentelemetry, dotnet, dotnet-framework, app-service]
---

# OpenTelemetry sur .NET Framework 4.8 (ASP.NET classique)

Le SDK OpenTelemetry .NET **supporte .NET Framework 4.6.2+**. On peut donc instrumenter une vieille Web App ASP.NET et l'envoyer vers un collecteur OTLP.

## Packages NuGet

- `OpenTelemetry`
- `OpenTelemetry.Exporter.OpenTelemetryProtocol`
- `OpenTelemetry.Instrumentation.AspNet` → ajoute le `TelemetryHttpModule` dans le `web.config` (**vérifier qu'il y est**)
- `OpenTelemetry.Instrumentation.Http` (HttpClient / HttpWebRequest)
- `OpenTelemetry.Instrumentation.SqlClient` (si SQL Server)

## Initialisation (`Global.asax.cs`)

```csharp
private TracerProvider _tracerProvider;

protected void Application_Start()
{
    _tracerProvider = Sdk.CreateTracerProviderBuilder()
        .AddAspNetInstrumentation()
        .AddHttpClientInstrumentation()
        .AddSqlClientInstrumentation()
        .AddOtlpExporter()   // lit les variables OTEL_*
        .Build();
}

protected void Application_End() => _tracerProvider?.Dispose();
```

Idem avec `Sdk.CreateMeterProviderBuilder()` pour les métriques.

## Configuration (app settings de la Web App = variables d'env)

```
OTEL_SERVICE_NAME=mon-webapp
OTEL_EXPORTER_OTLP_ENDPOINT=https://otel-collector.<domaine>
OTEL_EXPORTER_OTLP_PROTOCOL=http/protobuf
OTEL_EXPORTER_OTLP_HEADERS=Authorization=Bearer <token>   (référence Key Vault)
```

## Pièges spécifiques .NET Framework

- **`http/protobuf` plutôt que gRPC** : sur .NET Framework, `HttpClient` gère mal HTTP/2 → l'export gRPC pose problème.
- **Désactiver l'agent Application Insights** de la Web App (`ApplicationInsightsAgent_EXTENSION_VERSION`) → sinon doublons et conflits.
- **Logs** selon le framework : Serilog → `Serilog.Sinks.OpenTelemetry` ; `Microsoft.Extensions.Logging` → `AddOpenTelemetry()` ; log4net / NLog → appenders communautaires.
- **Alternative zéro code** : l'auto-instrumentation OTel .NET (profiler CLR, variables `COR_PROFILER*`) supporte .NET Framework, mais elle est plus fragile à installer sur App Service.

## Voir aussi

- [[opentelemetry-sdk-config-otlp]]
- [[collecteur-otel-central-acces-reseau]] — joindre le collecteur depuis la Web App
