# DNS Security Lab
## Descripción

Este laboratorio tiene como objetivo realizar un proceso de **reconocimiento pasivo** mediante técnicas de **DNS Footprinting** sobre el dominio `latinoamericacomparte.com`. Utilizando las herramientas `nslookup`, `dig` y `whois` disponibles en **Kali Linux**, se recopila información pública sobre la infraestructura DNS del dominio sin interacción directa con sus servidores.

Este tipo de reconocimiento es fundamental en la fase inicial de cualquier evaluación de seguridad, ya que permite construir un mapa técnico de la infraestructura del objetivo sin generar tráfico sospechoso.

> ⚠️ **Uso ético:** Todas las actividades de este laboratorio se realizan exclusivamente sobre dominios y recursos autorizados o de uso público. Realizar reconnaissance sin autorización puede violar la **Ley 1273 de 2009** (Colombia).

---

## Objetivos

- Verificar conectividad de red antes de ejecutar consultas DNS
- Resolver la dirección IP pública del dominio mediante `nslookup`
- Identificar los Name Servers autoritativos (registros NS)
- Obtener los servidores de correo asociados (registros MX)
- Ejecutar DNS inverso (Reverse DNS / registro PTR)
- Analizar metadatos DNS avanzados con `dig` (TTL, tamaño, tiempo de consulta)
- Trazar la jerarquía completa de resolución DNS con `dig +trace`
- Verificar el estado de DNSSEC (registros DNSKEY y DS)
- Cruzar la información DNS con datos de registro WHOIS
- Construir un mapa técnico de la infraestructura pública del dominio

---

## Herramientas Utilizadas

- **Kali Linux** — Sistema operativo de auditoría de seguridad
- **nslookup** — Consulta básica de registros DNS (A, NS, MX, PTR)
- **dig** — Consulta DNS avanzada con metadatos (TTL, trace, DNSKEY, DS)
- **whois** — Consulta de datos de registro de dominio
- **ping** — Verificación de conectividad de red
- **Técnicas de reconocimiento pasivo** — Sin interacción directa con los servidores objetivo

---

## Sitio Analizado

**https://latinoamericacomparte.com/**  
**Dominio principal:** `latinoamericacomparte.com`

---

## Elementos Identificados

| Entidad | Detalle |
|---|---|
| **IP Pública** | `64.202.187.143` |
| **Name Servers (NS)** | `ns05.domaincontrol.com` / `ns06.domaincontrol.com` |
| **Registro MX** | `mail.latinoamericacomparte.com` (prioridad 0) |
| **DNS Inverso (PTR)** | `ip-64-202-187-143.ip.secureserver.net` |
| **TTL** | 3600 segundos (renovación de caché cada hora) |
| **Registro SOA** | Serial `2025110701` — administrador `dns.jomax.net` (GoDaddy) |
| **DNSSEC** | `unsigned` — **NO habilitado** ⚠️ |
| **Proveedor de hosting** | SecureServer.net (infraestructura de GoDaddy) |
| **Registrante** | Protegido por Domains By Proxy LLC (GoDaddy Privacy) |
| **Fecha de creación** | 6 de agosto de 2025 |
| **Fecha de expiración** | 6 de agosto de 2026 |
| **Registro SPF** | `v=spf1 +mx +a +ip4:64.202.187.143 include:secureserver.net ~all` |

---

## Procedimiento Ejecutado

### Paso 0 — Verificación de Conectividad
```bash
ping 8.8.8.8
```
88 paquetes transmitidos, 0% de pérdida. Tiempo promedio: 24.9 ms (mín: 16.0 ms / máx: 63.4 ms).

---

### Paso 1 — Resolución Básica (nslookup)
```bash
nslookup latinoamericacomparte.com
```
El servidor DNS local (`190.248.0.8`) resolvió el dominio a la IP pública **64.202.187.143** (respuesta no autoritativa).

---

### Paso 2 — Servidores Autoritativos (NS)
```bash
nslookup -type=ns latinoamericacomparte.com
```
Se identificaron dos name servers: `ns05.domaincontrol.com` y `ns06.domaincontrol.com`, ambos de GoDaddy LLC. La doble configuración garantiza redundancia en la resolución DNS.

---

### Paso 3 — Servidores de Correo (MX)
```bash
nslookup -type=mx latinoamericacomparte.com
```
Registro MX único: `mail.latinoamericacomparte.com` con prioridad **0** (máxima). El dominio gestiona su propio servidor de correo interno, sin delegar a servicios externos como Google Workspace o Microsoft 365.

---

### Paso 4 — DNS Inverso (Reverse DNS / PTR)
```bash
nslookup 64.202.187.143
```
La IP resuelve al hostname `ip-64-202-187-143.ip.secureserver.net`, confirmando hosting en SecureServer.net. El registro PTR correctamente configurado indica buena reputación del servidor en sistemas antispam.

---

### Paso 5 — Análisis Avanzado con dig
```bash
dig latinoamericacomparte.com
```
Confirmó: IP `64.202.187.143`, estado `NOERROR`, tipo `A` (IPv4), TTL **3600 s**, tiempo de respuesta **76 ms**, tamaño de mensaje **70 bytes**, servidor DNS `190.248.0.8` por UDP.

---

### Paso 6 — Trazado Jerárquico (dig +trace)
```bash
dig -4 +trace latinoamericacomparte.com
```
Cadena completa de resolución:
```
Root Servers (a-m.root-servers.net)
    └─► TLD .com (a-m.gtld-servers.net — Verisign)
            └─► ns05.domaincontrol.com
                    └─► 64.202.187.143
```

---

### Paso 7 — Verificación DNSSEC
```bash
dig DNSKEY latinoamericacomparte.com
dig DS latinoamericacomparte.com
```
Ambas consultas devolvieron `ANSWER: 0`. El dominio **NO tiene DNSSEC habilitado** (`unsigned`), dejándolo expuesto a ataques de **DNS spoofing** y **cache poisoning**.

---

### Paso 8 — Cruce con WHOIS
```bash
whois latinoamericacomparte.com
```
Datos de registro confirmados. Los datos del propietario real están protegidos por **Domains By Proxy LLC**. El dominio cuenta con cuatro protecciones de estado activas: `clientDeleteProhibited`, `clientRenewProhibited`, `clientTransferProhibited` y `clientUpdateProhibited`.

---

## Resultados y Hallazgos

### Mapa de Infraestructura DNS

```
latinoamericacomparte.com
├── A Record ──────────────► 64.202.187.143 (SecureServer.net / GoDaddy)
├── NS Records ─────────────► ns05.domaincontrol.com
│                            ns06.domaincontrol.com
├── MX Record ──────────────► mail.latinoamericacomparte.com (prioridad 0)
├── PTR (Reverse DNS) ──────► ip-64-202-187-143.ip.secureserver.net
├── SOA ────────────────────► ns05.domaincontrol.com / dns.jomax.net
├── SPF ────────────────────► +mx +a +ip4:64.202.187.143 include:secureserver.net ~all
└── DNSSEC ─────────────────► ⚠️ unsigned (NO habilitado)
```

---

## Conclusiones

**El dominio está activo y resuelve correctamente.**  
La IP `64.202.187.143` fue confirmada de forma consistente por `nslookup`, `dig` y el trazado `+trace`. El TTL de 3600 segundos indica renovación de caché cada hora.

**Toda la infraestructura está centralizada en GoDaddy / SecureServer.**  
Registro de dominio, name servers, hosting y servidor de correo se encuentran en el mismo proveedor, representando un **punto único de falla**.

**El dominio gestiona su propio servidor de correo interno.**  
`mail.latinoamericacomparte.com` con prioridad MX 0 indica que el correo no se delega a servicios externos, lo que implica mayor responsabilidad operativa en su administración y mantenimiento.

**DNSSEC no está habilitado — vulnerabilidad identificada.**  
El estado `unsigned` confirmado por `dig DNSKEY`, `dig DS` y WHOIS deja el dominio vulnerable a **DNS spoofing** y **cache poisoning**. Se recomienda habilitarlo desde el panel de GoDaddy.

**La resolución jerárquica DNS funciona correctamente.**  
El trazado `dig +trace` mostró la cadena completa `root servers → gtld-servers.net (Verisign) → ns05/ns06.domaincontrol.com` sin rupturas en ningún nivel.

**El SPF protege parcialmente el correo.**  
La directiva `~all` (softfail) en lugar de `-all` (hardfail) significa que correos no autorizados serán marcados como sospechosos pero **no rechazados** automáticamente. Se recomienda endurecer la política.

**Los datos del registrante están protegidos por privacidad.**  
Domains By Proxy de GoDaddy oculta los datos del propietario real, lo cual es una buena práctica para proteger la identidad del administrador.

---

## Referencias

- [Kali Linux Official Documentation](https://www.kali.org/docs/)
- [dig Manual — Linux man pages](https://linux.die.net/man/1/dig)
- [nslookup Reference](https://linux.die.net/man/1/nslookup)
- [WHOIS Protocol — RFC 3912](https://datatracker.ietf.org/doc/html/rfc3912)
- [DNSSEC Overview — ICANN](https://www.icann.org/resources/pages/dnssec-what-is-it-why-important-2019-03-05-en)
- [SPF Record Syntax — RFC 7208](https://datatracker.ietf.org/doc/html/rfc7208)
- [Ley 1273 de 2009 — Colombia (Delitos Informáticos)](https://www.funcionpublica.gov.co/eva/gestornormativo/norma.php?i=34492)
- OSINT Framework — https://osintframework.com/

---

*Laboratorio realizado en el espacio académico **Ciberseguridad Blue Team y White Hat** — Universidad Santo Tomás Seccional Tunja, 2026.*