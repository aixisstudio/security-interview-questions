# Security Interview Questions

[English](README.md) | [Français](README.fr.md) | **Español** | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Русский](README.ru.md)

Preguntas de entrevista de ciberseguridad con respuestas

## 🎯 Ciberseguridad entrevista preguntas - con respuestas

### Networking

**¿Qué pasa cuando escribes una URL?**
Resolución DNS → TCP Handhake (SYN/SYN-ACK/ACK) → TLS handshake → HTTP petición → respuesta render. Bono: mencione ARP, NAT, routing en el camino.

**TCP vs UDP?**
TCP: fiable, ordenado, orientado a la conexión (web, SSH). UDP: fuego-y-olvidado, baja latencia (DNS, VoIP, QUIC construye fiabilidad en la parte superior).

**¿Qué es una inundación SYN y cómo mitiga?**
Medias conexiones abiertas agotan el atraso. Mitigate: Cookies SYN, límite de tarifas, afinación atrasada.

** Explique NAT vs PAT.**
NAT mapas IPs 1:1 o piscina; PAT (sobrecarga de NAT) multiplexes muchos hosts detrás de una IP usando puertos — lo que hace su router de casa.

## Seguridad fundamentals

¿La CIA Triad?
Confidencialidad, Integridad, Disponibilidad — las tres propiedades que cada control protege.

**Symmetric vs asymmetric crypto - cuando cada uno?**
Symmetric (AES): datos rápidos y a granel. Asimetría (RSA/ECC): intercambio clave, firmas. Los sistemas reales utilizan híbridos (TLS: apretón de manos asimétrico → AES sesión).

¿Hashing vs encryption?
La piratería es unidireccional (integridad, contraseñas — use bcrypt/argon2). El cifrado es reversible con una clave (confidencialidad).

**¿Qué es XSS y los 3 tipos?**
Reflected (in URL/response), Stored (persisted in DB), DOM-based (client-side sink). Mitigate: codificación de salida, CSP, saneamiento.

**CSRF vs SSRF**
CSRF engaña al navegador de usuario* para actuar; SSRF engaña al *servidor* para hacer solicitudes. Diferentes límites de confianza.

################################################################################################################################################################################################################################################################

**¿Cómo investigas un proceso sospechoso?**
`ps`/`lsof`/`ss` para conexiones → check binario hash (VirusTotal) → árbol de proceso de padres → mecanismos de persistencia (cron, lanzado, registro) → indicadores de red (beaconing, DGA-mirando dominios).

**Un usuario informa que su laptop "habla a un servidor en un país que no conocen". ¿Primeros pasos?**
Identificar el proceso de propiedad de la conexión (por proceso existen herramientas de visibilidad como [Snitch] (https://github.com/aixisstudio/Snitch) para esto), comprobar la reputación del dominio, ver intervalos de programación, instantánea antes de matar.

**¿Cómo se abusa de DNS?**
Tunneling (datos en las consultas TXT), DGA para C2, DNS-over-HTTPS para evadir la visibilidad, envenenamiento por caché (principalmente fijado por azarización + DNSSEC).

## Behavioral

*Dime sobre un incidente de seguridad que manejaste*
Estructura: detección → contención → erradicación → recuperación → lecciones. Mostrar que sabes que la revisión post-incidente importa más que heroicos.

**¿Cómo te mantienes actual?**
Fuentes nombradas: asesorías, piensos de proveedores, comunidad, laboratorios. Una respuesta real con nombres reales gana "Leo mucho".

## 🧪 Scenario Preguntas (con marcos de respuesta)

**"Usted encuentra un proceso desconocido acuñando a una IP cada 60s. Atravesadme."**
→ Identificar el proceso (`lsof -i`, `ps`), hash it (VirusTotal), comprobar la persistencia (lanchd/registry/service), aislar el anfitrión, luego cazar el mismo patrón de baliza en toda la flota. Mention Snitch-style egress monitoring as the detection layer.

** "¿Cómo detectaría la exfiltración de datos sobre DNS?"**
→ Buscar: alto volumen de consulta a un dominio, etiquetas largas ( &lt;63 chars), entropía en subdominios, TXT abuso de registro. Herramientas: dnstap, DNS pasivo, sus propios registros de resolución.

**"Un desarrollador empujó una llave AWS a GitHub. ¿Tiempo?"**
→ Assume comprometido instantáneamente (los escáneres lo encuentran en minutos). Revoke → rotar → auditar CloudTrail para uso → fuerza-push historia limpieza → postmortem en prevención (git-secrets, ganchos pre-commit).

**"Explicar producciones/cons de interceptación de TLS en una empresa."**
→ Pro: visibilidad en egresos cifrados. Con: rompe cert pinning, privacidad/exposición legal, punto central de descifrado = objetivo. Alternativas: huella dactilar JA3, análisis de metadatos solo.

Preguntas de diseño

**"Diseñar un sistema de gestión de secretos para una puesta en marcha."**
→ Vault/SOPS + credos de corta duración + no secretos en env vars en reposo + rotación + registro de auditoría + procedimiento de break-glass. Mención que el *proceso* importa más que la herramienta.

¿Cómo aseguras el CI/CD?**
→ Firmas firmadas, protección de ramas, corredores efímeros, OIDC en lugar de las teclas de nube almacenadas, dependencia pinning + SBOM, fichas de menor privilegio, firma de artefactos (cosign).

## І Junior-Specific Expectations

Los entrevistadores de nivel inferior quieren: **honest "No lo sé" + cómo lo descubrirías**, fluidez básica de la CIA, sabiendo la diferencia entre authN/authZ, y curiosidad. Nadie espera que recite RFCs.

---
## Contribuir

Los pull requests son bienvenidos: abre una PR y será revisada y fusionada. Mantenido por [aixisstudio](https://github.com/aixisstudio), autor de [Snitch](https://github.com/aixisstudio/Snitch), firewall de aplicaciones open source.

## Licencia: MIT
