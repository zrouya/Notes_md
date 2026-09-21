---
tags: [moc, docker]
---

# Images et Build

Construction des images Docker (Dockerfile, layers, multi-stage) et leur distribution via des registries.

## Notes

- [[Images Docker]] — composition d'une image (fichiers, dépendances, metadata)
- [[Docker files]] — le Dockerfile, patron de construction du conteneur
- [[Build des images Docker]] — commande `docker image build`
- [[Build multi-stage d'images Docker]] — optimisation du build en plusieurs étapes
- [[Layers d'images docker]] — empilement de layers identifiés par SHA
- [[Tags des images Docker]] — pointeurs vers un commit d'image
- [[Standard OCI]] — standard d'image OCI
- [[UFS Layer]] — système de fichiers en union utilisé par Docker
- [[Docker registry]] — application serveur de stockage/distribution d'images
- [[Docker repository]] — notion de repository au sein d'un registry
- [[DockerHUB]] — registry public officiel de Docker
- [[Publier une image Docker vers un Azure Container Registry]] — push d'images vers un ACR
- [[Fichier yaml]] — notions YAML utilisées notamment dans les Dockerfile/Compose
