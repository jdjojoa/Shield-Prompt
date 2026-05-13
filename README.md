# Chatbot Security Lab

##  Descripción

Laboratorio de seguridad ofensiva y defensiva para chatbots e inteligencia artificial.

---

# Objetivos

- Simular ataques IA
- Analizar Prompt Injection
- Proteger sistemas conversacionales

---

# Funcionalidades

- Prompt Injection Lab
- Jailbreak Testing
- Role Bypass
- AI Firewall
- Sistema RAG
- Historial conversacional

---

#  Ataques Simulados

- Prompt Injection
- Context Manipulation
- Role Override
- Jailbreak
- Data Leakage

---

#  Tecnologías

- Python
- Flask

# ------- Pasos a seguir desde la terminal de VSCode -------
---

## 1. crear el entorno virtual (Terminal 1)
py -m venv .venv

## 3. Ingresar al entorno virtual
.\.venv\Scripts\activate

.\.venv\Scripts\Activate.ps1

## 2. Instalar dependencias:
python -m pip install --upgrade pip
pip install -r requirements.txt
pip install requests pandas colorama tabulate  [alternativo]
pip install openpyxl [alternativo]
pip list 


## 4. Ejecutar el programa
python app.py


### Relación e interacción entre los archivos del sistema

- Este proyecto funciona bajo una arquitectura cliente-servidor donde:

1.) Frontend (cliente): index.html + script.js

2.) Backend (servidor): app.py
