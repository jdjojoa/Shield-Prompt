# HTTrack Security Lab

## Descripción

Este laboratorio está enfocado en la **clonación de sitios web** y el análisis de **exposición de información** utilizando **HTTrack**.  
El objetivo principal es analizar el sitio `latinoamericacomparte.com` mediante la generación de un espejo local completo para identificar recursos públicos, endpoints, dependencias externas y posibles datos sensibles visibles en el código fuente.

A partir de la URL objetivo, se construye una réplica local del sitio que permite inspeccionar su estructura sin interacción activa con el servidor.

> ⚠️ **Uso ético:** Todas las actividades de este laboratorio se realizan exclusivamente sobre dominios y recursos autorizados o de uso público. Clonar sitios sin autorización puede violar la Ley 1273 de 2009 (Colombia).

---

## Objetivos

- Verificar e instalar correctamente HTTrack en Kali Linux
- Clonar el sitio `latinoamericacomparte.com` y replicar su estructura localmente
- Levantar un servidor local con Python para verificar el funcionamiento del clon
- Analizar el código descargado mediante búsquedas `grep` en busca de APIs, tokens, claves y llamadas HTTP
- Documentar hallazgos y evaluar el nivel de exposición del sitio
- Comparar resultados con otros sitios estáticos analizados
- Interpretar los hallazgos en un contexto de reconocimiento pasivo (White Hat)

---

## Herramientas Utilizadas

- HTTrack Website Copier 3.49-6
- Kali Linux
- Python 3 (servidor HTTP local)
- Comandos `grep` para análisis de código
- Navegador web
- Técnicas de reconocimiento pasivo (OSINT)

---

## Sitio Analizado

**https://latinoamericacomparte.com/**  
**Dominio principal:** `latinoamericacomparte.com`

---

## Elementos Analizados

| Elemento              | Detalle                                                              |
|-----------------------|----------------------------------------------------------------------|
| **URL objetivo**      | `https://latinoamericacomparte.com/` (entidad inicial)              |
| **Archivos clonados** | 66 archivos — HTML, CSS, JS, PNG, JPG, GIF                          |
| **Tamaño total**      | 38.853.718 bytes (~38.8 MB) en 1 minuto 17 segundos                 |
| **API encontrada**    | `https://api.web3forms.com/submit` (formulario de contacto)         |
| **fetch() real**      | `const response = await fetch(contactFormNew.action, {...})`        |
| **Token expuesto**    | No encontrado (buena práctica)                                      |
| **access_key**        | Campo `name="access_key"` visible en el formulario HTML             |
| **Pasarela de pago**  | `https://checkout.bold.co/payment/LNK_Z48LF520TB`                  |
| **Dominios hermanos** | `colombiacomparte.com`, `ecuadorcomparte.com.co`, `chilecomparte.com.co`, `argentinacomparte.com` |
| **CDN externo**       | `cdnjs.cloudflare.com` — librería `anime.min.js`                    |

---

## Procedimiento Recomendado

1. Verificar la instalación de HTTrack en el sistema
2. Crear el directorio de trabajo para el laboratorio
3. Ejecutar HTTrack con los filtros adecuados para clonar el sitio
4. Verificar la estructura del directorio generado con `ls -la`
5. Levantar un servidor HTTP local con Python en el puerto 8080
6. Acceder desde el navegador y confirmar que el clon carga correctamente
7. Ejecutar los siguientes comandos de análisis:
   - `grep -r "api" .`
   - `grep -r "fetch" .`
   - `grep -r "token" .`
   - `grep -r "key" .`
   - `grep -r "http" .`
8. Documentar cada hallazgo con capturas de pantalla
9. Elaborar el informe con análisis y conclusiones

---

## Comandos Principales

```bash
# Verificar instalación
httrack --version

# Crear directorio y clonar el sitio
mkdir ~/lab1_latinoamerica && cd ~/lab1_latinoamerica

httrack "https://latinoamericacomparte.com/" \
  -O "$HOME/lab1_latinoamerica" \
  "*.netlify.app/*" \
  "+*.css" "+*.js" "+*.html" "+*.png" "+*.jpg" "+*.jpeg" "+*.gif" \
  -v

# Verificar estructura clonada
ls -la

# Levantar servidor local
python3 -m http.server 8080 --bind 127.0.0.1

# Análisis con grep
grep -r "api" .
grep -r "fetch" .
grep -r "token" .
grep -r "key" .
grep -r "http" .
```

---

## Resultados Obtenidos

### grep -r "api"
Se identificó el endpoint real `https://api.web3forms.com/submit` como acción del formulario de contacto del sitio, además de referencias a `fonts.googleapis.com` para carga de tipografía. Las coincidencias en archivos binarios son falsos positivos sin relevancia.

### grep -r "fetch"
Se encontró una llamada `fetch()` real escrita directamente en el código del sitio:
```js
const response = await fetch(contactFormNew.action, { ... })
```
A diferencia de los sitios estáticos de referencia donde `fetch` solo aparecía dentro de código minificado de Bootstrap, aquí confirma lógica dinámica propia que envía datos a servicios externos.

### grep -r "token"
**Sin resultados relevantes.** El sitio no expone tokens JWT, tokens de sesión ni credenciales de autenticación en el código fuente visible. Representa una buena práctica de seguridad.

### grep -r "key"
Se detectó el campo `name="access_key"` dentro del formulario de contacto, que es el identificador que web3forms usa para autenticar solicitudes. También se encontraron animaciones CSS (`@keyframes`) y propiedades de teclado (`event.key`) de la librería anime.js, que no representan riesgo.

### grep -r "http"
Se mapearon todas las dependencias externas del sitio: Google Fonts, CDN Cloudflare, API web3forms, pasarela de pago Bold (`checkout.bold.co/payment/LNK_Z48LF520TB`), cuatro dominios hermanos de la red latinoamericana y cinco redes sociales activas.

---

## Resultados Esperados

- Comprensión del uso de HTTrack para clonación y reconocimiento de sitios web
- Identificación clara de recursos públicos, dependencias y endpoints de un sitio real
- Capacidad para relacionar: URL → Archivos clonados → Código fuente → Endpoints → Dependencias externas
- Interpretación de hallazgos en el contexto de análisis de exposición
- Documentación técnica de hallazgos con evidencias y capturas
- Fortalecimiento de habilidades en reconocimiento pasivo y análisis White Hat

---

## Criterios del Informe Entregable

1. **Descripción de la herramienta** — Breve explicación de HTTrack y su utilidad en seguridad web
2. **Proceso de clonación** — Captura del proceso HTTrack con su resultado final
3. **Estructura clonada** — Evidencia del directorio generado y archivos descargados
4. **Servidor local** — Captura del sitio funcionando en el entorno local
5. **Resultado por búsqueda grep** — Evidencias de `api`, `fetch`, `token`, `key` y `http`
6. **Análisis** — Interpretación de los hallazgos de cada búsqueda
7. **Conclusiones** — Riesgos de exposición identificados y comparación con otros sitios

**Formato de entrega:** Documento PDF o Word

---

## Referencias

- [HTTrack Official Documentation](https://www.httrack.com/html/fcguide.html)
- [HTTrack Manual](https://www.httrack.com/html/shelldoc.html)
- OSINT Framework
- Ley 1273 de 2009 — Delitos informáticos (Colombia)