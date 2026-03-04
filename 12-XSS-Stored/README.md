![Vulnerabilidad](https://img.shields.io/badge/Vulnerabilidad-XSS%20(Stored)-red?style=for-the-badge)
![Nivel](https://img.shields.io/badge/Nivel%20de%20Seguridad-Medium-orange?style=for-the-badge)
![Herramienta](https://img.shields.io/badge/Herramienta-DevTools-blue?style=for-the-badge)
![Estado](https://img.shields.io/badge/Estado-Completado-success?style=for-the-badge)

# Práctica 12: Cross-Site Scripting Almacenado (XSS Stored) (Nivel: Medium)

## 1. Descripción de la Vulnerabilidad
El **Cross-Site Scripting Almacenado (Stored XSS)** o Persistente es la variante más crítica de XSS. Ocurre cuando una aplicación web recibe datos maliciosos de un atacante y los **guarda permanentemente** en su base de datos (por ejemplo, en un foro, un comentario de un blog o un libro de visitas). Posteriormente, cuando cualquier usuario legítimo (incluyendo administradores) visita la página afectada, el servidor le envía el código malicioso infectando su navegador automáticamente.

---

## 2. Análisis del Nivel de Seguridad
En el nivel **Medium**, el desarrollador ha implementado medidas de seguridad diferentes para los dos campos del formulario del libro de visitas (Guestbook):
* **Campo "Message":** Está protegido con funciones como `htmlspecialchars()` o `strip_tags()`, lo que impide inyectar código HTML/JS válido.
* **Campo "Name":** El backend utiliza `str_replace()` para eliminar la etiqueta exacta `<script>`, pero sigue siendo vulnerable a la inyección.

> **⚠️ Debilidad del mecanismo:** Para evitar que se inyecte código en el campo "Name", el desarrollador confió en una validación del lado del cliente añadiendo el atributo HTML `maxlength="10"` a la etiqueta `<input>`, limitando la longitud del nombre. Sin embargo, las restricciones del lado del cliente (DOM) nunca deben usarse como medida de seguridad única, ya que el usuario tiene control total sobre ellas y puede modificarlas fácilmente.

---

## 3. Metodología de Explotación
Para superar las restricciones del nivel medio y lograr la inyección persistente, se siguió esta metodología enfocada en el campo "Name":

1. **Reconocimiento del DOM:** Se utilizó la herramienta de Inspección de Elementos del navegador (DevTools - F12) para analizar el código HTML del campo "Name".
2. **Bypass de la Restricción del Cliente:** En el inspector, se localizó el atributo `maxlength="10"` y se modificó manualmente su valor a `maxlength="100"`. Esto permitió saltarse la restricción del navegador y escribir un payload largo.
3. **Evasión del Filtro del Servidor:** Sabiendo que el servidor elimina la etiqueta `<script>` en minúsculas, se utilizó una variación con mayúsculas para engañar al filtro `str_replace()`.
   
   **Payload inyectado en el campo Name:**
   `<SCRIPT>alert("XSS Stored!")</SCRIPT>`

---

## 4. Análisis de Resultados (Evidencias)
Al enviar el formulario, el servidor procesó el campo "Name". Como no encontró la coincidencia exacta `<script>`, permitió que el payload se guardara en la base de datos y lo publicó en el muro del Guestbook.

* **Resultado:** El ataque persistente tuvo éxito. Ahora, cada vez que cualquier usuario recarga la página o entra al libro de visitas, el navegador descarga el registro de la base de datos, renderiza la etiqueta `<SCRIPT>` y ejecuta la alerta bloqueando la navegación. 

### Datos Clave del Ataque
| Campo Vulnerable | Bypass del Cliente | Payload de Evasión (Servidor) |
| :--- | :--- | :--- |
| `txtName` (Nombre) | Modificación de `maxlength` | `<SCRIPT>alert("XSS Stored!")</SCRIPT>` |

---

## 5. Galería de Evidencias
A continuación se detallan las capturas de pantalla que documentan el proceso. *(Puedes encontrar las imágenes en esta misma carpeta)*:

**Captura 27: Inspección y modificación del DOM en vivo. Cambio del atributo maxlength para permitir la escritura del payload completo.**

![Modificación del DOM en el navegador](27.png)

**Captura 28: Evidencia técnica de la ejecución. El payload guardado en la base de datos se ejecuta persistentemente al cargar el Guestbook.**

![Ejecución persistente de XSS Stored](28.png)

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