# Maltego Security Lab

## Descripción
Este laboratorio está enfocado en el análisis **OSINT** y el reconocimiento de infraestructura pública utilizando **Maltego**. 

El objetivo principal es analizar el dominio `latinoamericacomparte.com` mediante transforms para identificar y mapear relaciones entre el dominio, registros DNS, servidores de correo, direcciones IP, Netblocks, ASN y otras entidades técnicas visibles públicamente.

A partir de una entidad inicial de tipo **Domain**, se construye un grafo de relaciones que permite visualizar la infraestructura asociada al sitio.

> ⚠️ **Uso ético:** Todas las actividades de este laboratorio se realizan exclusivamente sobre dominios y recursos autorizados o de uso público. Realizar reconnaissance sin autorización puede violar la Ley 1273 de 2009 (Colombia).

---

## Objetivos
- Configurar correctamente la entidad inicial del dominio en Maltego
- Obtener registros DNS clave (MX, NS, SOA y SPF)
- Identificar direcciones IP asociadas al dominio
- Obtener Netblock y ASN a partir de una IP
- Analizar las propiedades de los nodos dentro del grafo
- Construir y documentar un grafo de relaciones de infraestructura
- Interpretar los resultados en un contexto de inteligencia de fuentes abiertas (OSINT)

---

## Herramientas Utilizadas
- Maltego Community Edition
- Navegador web
- Transforms de Maltego (DNS, WHOIS, Netblock, etc.)
- Consulta de registros DNS
- Técnicas OSINT
- Análisis de grafos

---

## Sitio Analizado
**https://latinoamericacomparte.com/**

**Dominio principal:** `latinoamericacomparte.com`

---

## Elementos Analizados

| Entidad              | Detalle                                      |
|----------------------|----------------------------------------------|
| **Dominio**          | `latinoamericacomparte.com` (entidad inicial) |
| **Registro MX**      | `mail.latinoamericacomparte.com`             |
| **Name Servers (NS)**| `ns05.domaincontrol.com`<br>`ns06.domaincontrol.com` |
| **Registro SOA**     | Autoridad DNS y contacto técnico (`dns@jomax.net`) |
| **Registro SPF**     | `secureserver.net`, `mail.latinoamericacomparte.com`, `64.202.187.143`, etc. |
| **Dirección IP**     | `64.202.187.143`                             |
| **Netblock**         | `64.202.187.0 - 64.202.187.255`              |
| **ASN**              | Identificado a partir del Netblock           |

---

## Procedimiento Recomendado

1. Crear un nuevo Graph en Maltego
2. Agregar entidad **Domain** → `latinoamericacomparte.com`
3. Ejecutar los siguientes transforms principales:
   - `To MX Record`
   - `To NS Record`
   - `To SOA Record`
   - `To SPF Record`
   - `To IP Address`
   - `To Netblock`
   - `To ASN`
4. Revisar las propiedades de los nodos relevantes (especialmente el nodo MX)
5. Organizar, etiquetar y limpiar el grafo
6. Exportar capturas de evidencia

---

## Resultados Esperados
- Comprensión del uso de Maltego para reconnaissance OSINT
- Identificación clara de la infraestructura pública de un dominio
- Capacidad para relacionar: Domain → DNS Records → IP → Netblock → ASN
- Interpretación visual de grafos generados por Maltego
- Documentación técnica de hallazgos con evidencias
- Fortalecimiento de habilidades en reconocimiento pasivo

---

## Criterios del Informe Entregable
1. **Descripción de la herramienta** — Breve explicación de Maltego y su utilidad en OSINT
2. **Grafo principal** — Captura del grafo completo bien organizado
3. **Resultado por entidad** — Evidencias de MX, NS, SOA, SPF, IP, Netblock y ASN
4. **Análisis** — Interpretación de los hallazgos
5. **Conclusiones** — Riesgos de exposición identificados

**Formato de entrega:** Documento PDF

---

## Referencias
- [Maltego Official Documentation](https://docs.maltego.com/)
- [Maltego Transforms](https://docs.maltego.com/)
- OSINT Framework