![Vulnerabilidad](https://img.shields.io/badge/Vulnerabilidad-CSP%20Bypass-red?style=for-the-badge)
![Nivel](https://img.shields.io/badge/Nivel%20de%20Seguridad-Medium-orange?style=for-the-badge)
![Herramienta](https://img.shields.io/badge/Herramienta-Navegador%20Web-blue?style=for-the-badge)
![Estado](https://img.shields.io/badge/Estado-Completado-success?style=for-the-badge)

# RA3.2: Puesta en Producción Segura (DVWA)

## Descripción del Proyecto
Este repositorio contiene la documentación técnica y resolución práctica de la unidad **RA3.2: Ciberseguridad en entornos de las tecnologías de la información**.

El objetivo es desplegar un entorno de laboratorio controlado utilizando **Damn Vulnerable Web Application (DVWA)** para identificar, analizar y explotar vulnerabilidades web comunes (**OWASP Top 10**). 

Todas las prácticas se han realizado en nivel de dificultad **MEDIUM**, documentando el proceso de bypass de filtros y las evidencias de éxito.

---

## Despliegue e Instalación
Se utiliza **Docker** para garantizar la reproducibilidad. El servicio se expone en el puerto **9090**.

1. **Puesta en marcha:**
   `docker run --rm -it -p 9090:80 vulnerables/web-dvwa`

2. **Acceso:** `http://localhost:9090` (o la IP de tu máquina).
3. **Credenciales:** `admin` / `password`.

---

## Entorno y Herramientas
* **Navegador:** Mozilla Firefox.
* **Justificación:** Uso de la función nativa **"Edit and Resend"** de la pestaña Network para la manipulación de cabeceras HTTP (`Content-Type`, `Referer`, etc.) y parámetros POST.
* **Herramientas de apoyo:** Burp Suite Community Edition (para ataques de diccionario en Brute Force).

---

## Índice de Vulnerabilidades (Nivel Medium)

Haz clic en cada enlace para ver la metodología y capturas de pantalla:

1. [**Brute Force**](./01-Brute-Force/) - Ataque de diccionario con Burp Suite y análisis de Length/Time.
2. [**Command Injection**](./02-Command-Injection/) - Bypass de filtros de operadores mediante `&` y `|`.
3. [**CSP Bypass**](./03-CSP-Bypass/) - Explotación de Nonce estático.
4. [**CSRF**](./04-CSRF/) - Ataque encadenado mediante File Upload para bypass de Referer.
5. [**XSS (DOM Based)**](./05-XSS-DOM/) - Ruptura de contexto HTML y vector `onerror`.
6. [**File Inclusion**](./06-File-Inclusion/) - Inclusión de archivos locales (`/etc/passwd`) mediante rutas absolutas.
7. [**File-Upload**](./07-File-Upload/) - Subida de Webshell PHP manipulando el MIME Type.
8. [**JavaScript Attacks**](./08-JavaScript-Attacks/) - Ingeniería inversa de funciones de generación de tokens.
9. [**XSS (Reflected)**](./09-XSS-Reflected/) - Evasión de filtros de etiquetas `<script>`.
10. [**SQL Injection**](./10-SQL-Injection/) - Exfiltración de base de datos mediante inyección numérica.
11. [**SQL Injection (Blind)**](./11-SQL-Injection-Blind/) - Inferencia lógica booleana.
12. [**XSS (Stored)**](./12-XSS-Stored/) - Bypass de `maxlength` en el DOM y ofuscación de etiquetas.
13. [**Weak Session IDs**](./13-Weak-Session-IDs/) - Predicción de sesiones basada en Unix Timestamp.

---

## Aviso Legal
Este proyecto tiene fines estrictamente **educativos**. El uso de estas técnicas contra sistemas sin autorización es ilegal.



<div align="center">
    <p>Desarrollado con ❤️ por <b>MaikelPlay</b></p>
    <a href="https://github.com/MaikelPlay">
        <img src="https://img.shields.io/badge/GitHub-MaikelPlay-181717?style=flat&logo=github&logoColor=white" alt="GitHub">
    </a>
    <a href="https://hub.docker.com/u/maikelplay">
        <img src="https://img.shields.io/badge/Docker-MaikelPlay-2496ED?style=flat&logo=docker&logoColor=white" alt="DockerHub">
    </a>
    <a href="https://www.linkedin.com/in/mikel-jordan-moral/">
        <img src="https://img.shields.io/badge/LinkedIn-Mikel_Jordan-0077B5?style=flat&logo=linkedin&logoColor=white" alt="LinkedIn">
    </a>
    <a href="https://maikelplay.github.io/portfolio-web/">
        <img src="https://img.shields.io/badge/Portfolio-Visit_Web-8A2BE2?style=flat&logo=google-chrome&logoColor=white" alt="Portfolio">
    </a>
</div>