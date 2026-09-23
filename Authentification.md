
1. HTTP Basic Authentication

	HTTP Basic Authentication est un protocole simple qui envoie le nom d'utilisateur et le mot de passe à chaque demande HTTP. Cependant, il n'est pas très sûr car les informations d'identification sont envoyées en clair (bien qu'elles soient codées en base64, ce qui est facilement décodable).

2. HTTP Digest Authentication

	HTTP Digest Authentication améliore la sécurité de HTTP Basic Authentication en envoyant un hachage du mot de passe, au lieu du mot de passe lui-même. Cependant, il est encore vulnérable aux attaques par force brute et aux attaques de collision si un algorithme de hachage faible est utilisé.

3. [[OAuth 2.0 - OpenID|OAuth / OpenID]]