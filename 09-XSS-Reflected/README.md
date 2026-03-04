![Vulnerabilidad](https://img.shields.io/badge/Vulnerabilidad-XSS%20(Reflected)-red?style=for-the-badge)
![Nivel](https://img.shields.io/badge/Nivel%20de%20Seguridad-Medium-orange?style=for-the-badge)
![Herramienta](https://img.shields.io/badge/Herramienta-Navegador%20Web-blue?style=for-the-badge)
![Estado](https://img.shields.io/badge/Estado-Completado-success?style=for-the-badge)

# Práctica 09: Cross-Site Scripting Reflejado (XSS Reflected) (Nivel: Medium)

## 1. Descripción de la Vulnerabilidad
El **Cross-Site Scripting Reflejado (Reflected XSS)** ocurre cuando una aplicación web recibe datos de un usuario (generalmente a través de parámetros en la URL o un formulario) y los incluye inmediatamente en la respuesta HTTP (la página web) sin validarlos ni codificarlos (sanitizarlos) adecuadamente. Esto permite a un atacante inyectar código JavaScript malicioso que se ejecutará en el navegador de la víctima al hacer clic en un enlace manipulado.

---

## 2. Análisis del Nivel de Seguridad
En el nivel **Medium**, el desarrollador ha implementado una protección básica en el backend (PHP) utilizando la función `str_replace()`. El servidor busca específicamente la cadena exacta `<script>` dentro del parámetro de entrada (en este caso, el nombre introducido) y la elimina o reemplaza por un espacio en blanco para intentar neutralizar el ataque.

> **⚠️ Debilidad del mecanismo:** La función `str_replace()` utilizada en este nivel es **sensible a mayúsculas y minúsculas** (case-sensitive) y no es recursiva. Esto significa que solo busca la coincidencia exacta de `<script>` en minúsculas. Un atacante puede burlar este filtro fácilmente alterando el formato de la etiqueta HTML (usando mayúsculas) o utilizando etiquetas alternativas que ejecuten JavaScript sin usar la palabra restringida.

---

## 3. Metodología de Explotación
Para eludir el filtro de validación del servidor y lograr la inyección, se aplicó una técnica de evasión de listas negras:

1. **Reconocimiento:** Se introdujo un texto de prueba (`test`) en el campo de nombre para observar cómo el servidor lo reflejaba en la página (`Hello test`).
2. **Prueba de Filtro:** Al inyectar el clásico `<script>alert(1)</script>`, se observó que el servidor eliminaba la etiqueta de apertura, dejando el código inservible.
3. **Bypass del Filtro:** Se aprovechó la vulnerabilidad de la función `str_replace()` manipulando el *casing* (mayúsculas/minúsculas) de las etiquetas para que el filtro no las reconociera, pero el navegador sí (ya que HTML no distingue entre mayúsculas y minúsculas).

   **Payload inyectado:**
   `<SCRIPT>alert(document.cookie)</SCRIPT>` (o variaciones similares como `<Script>` o el uso de `<img src=x onerror=...>`).

---

## 4. Análisis de Resultados (Evidencias)
El servidor recibió el parámetro modificado. Al no encontrar la coincidencia exacta en minúsculas (`<script>`), el filtro dejó pasar el payload intacto y lo incrustó directamente en el código fuente de la página de respuesta.

* **Resultado:** El navegador de la víctima recibió el código HTML, interpretó la etiqueta `<SCRIPT>` como válida y ejecutó la función maliciosa, mostrando una ventana emergente con la cookie de sesión actual (`PHPSESSID`).

### Datos Clave del Bypass
| Filtro del Servidor | Payload de Evasión Inyectado |
| :--- | :--- |
| Elimina `<script>` | `<SCRIPT>alert(document.cookie)</SCRIPT>` |

---

## 5. Galería de Evidencias
A continuación se detallan las capturas de pantalla que documentan el proceso. *(Puedes encontrar las imágenes en esta misma carpeta)*:

**Captura 24: Evidencia técnica de la ejecución. El servidor refleja el payload evadiendo el filtro y el navegador dispara la alerta mostrando la cookie de sesión.**

![Ejecución de XSS Reflejado mostrando cookies](24.png)

---

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