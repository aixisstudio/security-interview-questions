# Security Interview Questions

[English](README.md) | **Français** | [Español](README.es.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Русский](README.ru.md)

Questions d'entretien cybersécurité et réseau, avec réponses

Questions d'entrevue sur la cybersécurité — avec réponses

Réseautage

**Que se passe-t-il lorsque vous tapez une URL?**
Résolution DNS → TCP handhake (SYN/SYN-ACK/ACK) → TLS handhake → requête HTTP → rendu de réponse. Bonus : mention ARP, NAT, routage en chemin.

**TCP vs UDP?**
TCP: fiable, ordonné, axé sur la connexion (web, SSH). UDP: feu et mort, faible latence (DNS, VoIP, QUIC construit la fiabilité en haut).

**Qu'est-ce qu'une inondation SYN et comment atténuer?**
Les connexions demi-ouvertes épuisent l'arriéré. Mitigate: Cookies SYN, limitation des taux, réglage de l'arriéré.

**Exposer NAT vs PAT.**
NAT maps IPs 1:1 ou pool; PAT (NAT surcharge) multiplexe de nombreux hôtes derrière une IP en utilisant des ports — ce que votre routeur d'origine fait.

Principes de sécurité

** Triade de la CIA?**
Confidentialité, intégrité, disponibilité — les trois propriétés que chaque contrôle protège.

**Symmétrique vs crypto asymétrique — quand chaque?**
Symmétrique (AES): données rapides et en vrac. Asymétrie (RSA/ECC): échange de clés, signatures. Les systèmes réels utilisent l'hybride (TLS : poignée de main asymétrique → session AES).

**Hashing vs cryptage?**
Hashing est à sens unique (intégrité, mots de passe — utiliser bcrypt/argon2). Le chiffrement est réversible avec une clé (confidentialité).

**Qu'est-ce que XSS et les 3 types?**
Reflecté (en URL/réponse), stocké (persisté en DB), basé sur DOM (évier côté client). Mitigate: encodage de sortie, CSP, sanitisation.

**CSRF vs SSRF?**
CSRF assaille le navigateur de l'utilisateur* en agissant ; SSRF assaille le serveur* en faisant des requêtes. Différentes frontières de confiance.

Pratique

**Comment enquêtez-vous sur un processus suspect?**
`ps`/`lsof`/`ss` pour les connexions → vérifier le hash binaire (VirusTotal) → l'arborescence du processus parent → les mécanismes de persistance (cron, lancé, registre) → les indicateurs de réseau (beaconing, les domaines à l'image de DGA).

**Un utilisateur signale son ordinateur portable « parle à un serveur dans un pays qu'il ne connaît pas ». Premiers pas?**
Identifier le processus propriétaire de la connexion (des outils de visibilité par processus comme [Snitch](https://github.com/aixisstudio/Snitch) existent pour cela), vérifier la réputation du domaine, regarder les intervalles de balisage, instantané avant de tuer.

**Comment le DNS est-il maltraité?**
Tunneling (données dans les requêtes TXT), DGA pour C2, DNS-over-HTTPS pour éviter la visibilité, empoisonnement cache (essentiellement fixé par randomisation + DNSSEC).

Comportement

**Parlez-moi d'un incident de sécurité que vous avez traité.**
Structure : détection → confinement → éradication → récupération → leçons. Montrez-vous que la revue post-incident importe plus que les héroïques.

**Comment restez-vous à jour?**
Sources nommées : avis, alimentation des fournisseurs, communauté, laboratoires. Une vraie réponse avec de vrais noms bat "J'ai beaucoup lu."

Questions de scénario (avec cadres de réponse)

**"Vous trouvez une balise de processus inconnue sur une IP toutes les années 60. Faites-moi traverser."**
→ Identifier le processus (`lsof -i`, `ps`), le hachage (VirusTotal), vérifier la persistance (lancement/enregistrement/service), isoler l'hôte, puis chasser le même modèle de balise dans toute la flotte. Mentionnez la surveillance d'évacuation de style Snitch comme couche de détection.

**« Comment détecteriez-vous l'exfiltration de données sur le DNS? »**
→ Recherchez : volume de requête élevé pour un domaine, longues étiquettes (>63 caractères), entropie dans les sous-domaines, abus d'enregistrement TXT. Outils : dnstap, DNS passif, vos propres journaux de résolution.

**"Un développeur a poussé une clé AWS vers GitHub. Calendrier?"**
→ Supposer compromis instantanément (les scanners le trouvent en quelques minutes). Revoke → rotation → audit CloudTrail pour l'utilisation → nettoyage de l'historique force-pouss → postmortem sur la prévention (sécrétions-git, pré-engagement des crochets).

** « Expliquez les avantages et les inconvénients de l'interception des SLT dans une entreprise. »**
→ Pro : visibilité dans l'évacuation chiffrée. Con: casse cert pinning, vie privée/exposition légale, point de décryptage central = cible. Solutions envisagées: empreintes digitales JA3, analyse des métadonnées uniquement.

Questions de conception

**"Concevoir un système de gestion des secrets pour une startup."**
→ Vault/SOPS + creds à courte durée de vie + aucun secret dans les env vars au repos + rotation + journal d'audit + procédure de verre de rupture. Mentionnez que le *processus* est plus important que l'outil.

**« Comment sécuriser CI/CD? »**
→ Signed commits, branch protection, coureurs éphémères, OIDC au lieu des clés de cloud stockées, dépendance pinning + SBOM, jetons moins privilèges, signature d'artefact (cosign).

Attentes pour les jeunes

Les intervieweurs de niveau junior veulent : **honnête "Je ne sais pas" + comment vous le savez**, base CIA triad fluency, connaissant la différence entre authN/authZ, et la curiosité. Personne ne s'attend à ce que vous récitiez les RFC.

---
## Contribuer

Les pull requests sont les bienvenues : ouvre une PR, elle sera relue et fusionnée. Maintenu par [aixisstudio](https://github.com/aixisstudio), auteur de [Snitch](https://github.com/aixisstudio/Snitch), pare-feu applicatif open source.

## Licence: MIT
