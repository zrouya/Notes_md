
Visual Studio (depuis la version 2017), gère le support de [[Docker]] et le développement d'applications [[DotNET Core|.Net Core]] conteneurisées.

- Lors de la **création** d'un projet .Net, l'option "**Enable Docker**" est disponible.
- Pour un **projet déjà créé**, il est possible de faire un **click droit** sur le projet, puis **Add-> Support Docker**

Visual Studio générera un **Dockerfile** à la racine du projet, par exemple : 

```dockerfile
#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ContainerBasedApplication.csproj", "."]
RUN dotnet restore "./././ContainerBasedApplication.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "./ContainerBasedApplication.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./ContainerBasedApplication.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ContainerBasedApplication.dll"]
```

Note : On utilise ici l'[[Images Docker|image]] **.Net sdk** pour **publier** l'application (générer les fichiers de l'application à partir du code source), alors que l'image "**asp.net**" sert au **runtime** de l'application (et est donc utilisée comme image de base).
Cette [[Build multi-stage d'images Docker|séparation]] rend la **conteneurisation** plus **flexible**, **légère** et **sécurisée**.
