# OWASP ZAP Security Lab

## Descripción

Este laboratorio está enfocado en el análisis de tráfico HTTP/HTTPS y pruebas de seguridad web utilizando OWASP ZAP (Zed Attack Proxy).

El objetivo principal es interceptar, analizar y documentar peticiones realizadas hacia aplicaciones web con fines educativos y de investigación en ciberseguridad, siguiendo los lineamientos del OWASP Top 10.

> ⚠️ **Uso ético:** Las pruebas documentadas en este laboratorio se realizan exclusivamente sobre entornos autorizados. Ejecutar estas herramientas sobre sitios sin permiso expreso del propietario es ilegal (Colombia: Ley 1273 de 2009).

---

## Objetivos

- Interceptar tráfico HTTP y HTTPS mediante proxy
- Analizar requests y responses capturados
- Realizar reconocimiento con Spider y AJAX Spider
- Ejecutar escaneo activo de vulnerabilidades (Active Scan)
- Revisar headers, cookies y sesiones HTTP
- Detectar posibles vulnerabilidades alineadas con OWASP Top 10
- Documentar hallazgos y evidencias por módulo
- Generar reporte técnico con ZAP

---

## Herramientas Utilizadas

- OWASP ZAP v2.17+ (Zed Attack Proxy)
- Kali Linux
- Firefox (configurado como proxy)
- OWASP Top 10 2021

---

## Sitio Analizado

https://latinoamericacomparte.com/

---

## Módulos de ZAP Documentados

| Módulo | Función |
|---|---|
| History | Registro completo del tráfico HTTP/HTTPS capturado |
| Alerts | Vulnerabilidades detectadas clasificadas por nivel de riesgo |
| Spider | Reconocimiento y mapeo de URLs estáticas |
| AJAX Spider | Reconocimiento de rutas dinámicas con JavaScript |
| Active Scan | Pruebas activas de explotación sobre parámetros y formularios |
| HTTP Sessions | Análisis de cookies y gestión de sesiones |
| WebSockets | Intercepción de comunicación en tiempo real |

---

## Vulnerabilidades Evaluadas

- Cross-Site Scripting (XSS) — Reflejado y Almacenado
- SQL Injection
- CSRF (Cross-Site Request Forgery)
- Session Management / Cookie Misconfiguration
- Header Misconfiguration (CSP, X-Frame-Options, HSTS)
- Information Disclosure
- Path Traversal / Directory Listing
- Input Validation

---

## Configuración del Entorno

### 1. Instalar OWASP ZAP
```bash
sudo apt update
sudo apt install zaproxy -y
zaproxy --version
```

### 2. Configurar proxy en Firefox
```
HTTP Proxy: 127.0.0.1
Port:       8090
```

### 3. Cambiar puerto del proxy en ZAP
```
Tools > Options > Local Servers/Proxies > Port: 8090
```

### 4. Instalar certificado SSL de ZAP (para HTTPS)
```
ZAP: Tools > Options > Network > Server Certificates > Save
Firefox: Settings > Privacy & Security > Certificates > View Certificates > Import
```

---

## Resultados Esperados

- Comprensión del funcionamiento del tráfico web interceptado por proxy
- Identificación de riesgos de seguridad alineados con OWASP Top 10 2021
- Documentación técnica de cada módulo de ZAP con capturas de evidencia
- Generación del reporte oficial de ZAP en formato HTML/PDF
- Fortalecimiento de conocimientos en pentesting web

---

## Criterios del Informe Entregable

1. **Descripción de la herramienta** — párrafo breve sobre OWASP ZAP
2. **Resultado por módulo** — capturas que evidencien cada salida
3. **Conclusión técnica** — fallas encontradas alineadas con OWASP Top 10
4. **Mitigación** — sugerencias para controlar los hallazgos en un entorno real

**Formato de entrega:** Documento PDF

---

## Referencias

- [OWASP ZAP Documentation](https://www.zaproxy.org/docs/)
- [OWASP Top 10 2021](https://owasp.org/Top10/)