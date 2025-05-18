# Git Reset vs Git Revert: Manejo de Historial y Corrección de Errores

En el desarrollo de software, cometer errores o necesitar deshacer cambios es una situación común. Git nos ofrece herramientas poderosas para manejar estas situaciones, siendo `git reset` y `git revert` dos de las más importantes. Aunque sus nombres puedan sonar similares, funcionan de maneras muy diferentes y tienen distintos casos de uso, especialmente en lo que respecta a la modificación del historial de commits.

Dominar estos comandos es clave para un control de versiones eficiente, permitiendo corregir errores y ajustar el historial de cambios de forma segura y controlada.

## 1. Diferencias Clave entre `git reset` y `git revert`

*   **`git reset`**:
    *   **Acción principal:** Mueve el puntero de la rama actual (y opcionalmente `HEAD`) a un commit anterior específico.
    *   **Impacto en el historial:** Puede reescribir el historial de commits. Los commits que quedan "después" del punto al que se resetea pueden volverse inaccesibles (o ser eliminados, dependiendo del modo).
    *   **Uso principal:** Deshacer commits recientes en tu rama local *antes* de compartirlos con otros, o para "limpiar" el historial local. Es como "volver en el tiempo" y pretender que ciertos commits nunca ocurrieron.
    *   **Precaución:** Debe usarse con mucho cuidado en ramas compartidas, ya que reescribir el historial que otros ya tienen puede causar grandes problemas.

*   **`git revert`**:
    *   **Acción principal:** Crea un **nuevo commit** que introduce cambios opuestos a los de un commit específico anterior.
    *   **Impacto en el historial:** No reescribe el historial. Añade un nuevo commit encima, preservando el historial original intacto.
    *   **Uso principal:** Deshacer los cambios de un commit específico de forma segura, especialmente en ramas compartidas, ya que no altera el historial existente. Es como decir "cometí un error en el commit X, así que aquí hay un nuevo commit Y que lo corrige".
    *   **Seguridad:** Es la forma más segura de deshacer cambios en repositorios colaborativos.

## 2. `git reset`: Volviendo en el Tiempo (con Precaución)

El comando `git reset` se utiliza para mover el puntero de la rama actual (`HEAD`) a un commit anterior. Tiene tres modos principales que determinan qué sucede con los cambios en tu staging area y tu directorio de trabajo.

**Para usar `git reset` eficazmente:**

1.  **Identifica el commit de destino:** Usa `git log` para ver tu historial y encontrar el hash del commit al que quieres regresar. `HEAD` actualmente apunta al último commit de tu rama.

    ```bash
    git log --oneline # Muestra el log de forma concisa
    ```

2.  **Ejecuta `git reset` con el modo deseado y el hash del commit:**

    *   **`git reset --soft <hash-del-commit>`**:
        *   Mueve `HEAD` al `<hash-del-commit>`.
        *   Los commits posteriores al `<hash-del-commit>` son eliminados del historial de la rama actual.
        *   **Los cambios de esos commits eliminados se mantienen en tu staging area.** Esto es útil si quieres reagrupar varios commits pequeños en uno solo o corregir el último commit.
        *   Tu directorio de trabajo no se modifica.

    *   **`git reset --mixed <hash-del-commit>` (Este es el modo por defecto si no especificas uno)**:
        *   Mueve `HEAD` al `<hash-del-commit>`.
        *   Los commits posteriores al `<hash-del-commit>` son eliminados del historial de la rama actual.
        *   **Los cambios de esos commits eliminados se mantienen en tu directorio de trabajo, pero son removidos del staging area.** Necesitarás hacer `git add` de nuevo si quieres conservarlos.
        *   Tu directorio de trabajo no se modifica (los archivos conservan los cambios).

    *   **`git reset --hard <hash-del-commit>`**:
        *   Mueve `HEAD` al `<hash-del-commit>`.
        *   Los commits posteriores al `<hash-del-commit>` son eliminados del historial de la rama actual.
        *   **¡PELIGRO! Los cambios de esos commits eliminados son borrados tanto del staging area como de tu directorio de trabajo.** Es una operación destructiva y los cambios pueden ser difíciles o imposibles de recuperar si no tienes copias.
        *   **Usa `git reset --hard` con extrema precaución**, especialmente si los commits ya han sido compartidos. Es más seguro para commits locales que aún no has subido (`push`).

**Ejemplo Práctico de `git reset --hard` (para deshacer el último commit local):**
Supongamos que hiciste un commit por error:
```bash
# Creas un archivo y haces un commit
echo "Este es un error" > error.txt
git add error.txt
git commit -m "Commit erróneo con error.txt"
git log --oneline # Verás el "Commit erróneo"
```
Para deshacer este último commit y eliminar `error.txt`:
```bash
# HEAD~1 se refiere al commit anterior al actual (HEAD)
git reset --hard HEAD~1
git log --oneline # El "Commit erróneo" habrá desaparecido
ls # error.txt ya no existirá
```
O si quieres volver a un commit específico por su hash:
```bash
# Supongamos que el hash del commit bueno anterior es "abcdef1"
git reset --hard abcdef1
```

## 3. `git revert`: Deshaciendo Cambios de Forma Segura

`git revert` es la forma recomendada de deshacer cambios en un commit que ya ha sido compartido con otros colaboradores, porque no altera el historial existente.

**Cómo funciona `git revert`:**

1.  **Identifica el commit a revertir:** Usa `git log` para encontrar el hash del commit cuyos cambios quieres deshacer.
    ```bash
    git log --oneline
    ```

2.  **Ejecuta `git revert <hash-del-commit-a-revertir>`:**
    ```bash
    git revert <hash-del-commit-a-revertir>
    ```
    *   Git creará un **nuevo commit** que aplica los cambios inversos al commit especificado. Por ejemplo, si el commit original añadió una línea, el commit de revert la eliminará.
    *   Se abrirá tu editor de texto configurado para que puedas escribir el mensaje del nuevo commit de revert. Por defecto, el mensaje será algo como "Revert '[mensaje del commit original]'". Es una buena práctica añadir una breve explicación de por qué se está revirtiendo, si es necesario.
    *   Guarda y cierra el editor para finalizar el commit de revert.

**Ejemplo Práctico de `git revert`:**
```bash
# Creamos un archivo y hacemos un commit
echo "Contenido especial" > archivo_especial.txt
git add archivo_especial.txt
git commit -m "Añadido archivo_especial.txt con contenido importante"
git log --oneline # Anota el hash de este commit, ej: "1234567"
```
Ahora, supongamos que ese "contenido importante" era un error y queremos deshacerlo:
```bash
git revert 1234567
# Se abre el editor para el mensaje del commit de revert. Guarda y cierra.
git log --oneline
# Verás un nuevo commit "Revert 'Añadido archivo_especial.txt...'"
# El archivo_especial.txt ya no tendrá el "Contenido especial" o habrá sido eliminado.
# El commit original "1234567" sigue en el historial.
```

## 4. ¿Cuándo Usar `git reset` vs `git revert`?

*   **Usa `git reset` (principalmente `--soft` o `--mixed`) cuando:**
    *   Estás trabajando en tu **rama local** y los commits **no han sido compartidos** (`push`) aún.
    *   Quieres corregir los últimos commits (ej. cambiar el mensaje, añadir/quitar archivos, combinar varios commits en uno).
    *   Quieres descartar completamente commits locales recientes y no te importa perder esos cambios (con `--hard`, con mucha precaución).

*   **Usa `git revert` cuando:**
    *   Necesitas deshacer los cambios de un commit que **ya ha sido compartido** con otros (`push`).
    *   Quieres mantener un historial claro y auditable de todos los cambios, incluyendo las reversiones.
    *   La colaboración y la estabilidad del historial compartido son prioritarias.

## 5. Comunicación y Buenas Prácticas

*   **Comunicación en Equipo:** Si trabajas en equipo, es crucial comunicar cualquier acción que modifique el historial compartido.
*   **Evita `git reset --hard` en Ramas Compartidas:** A menos que todo el equipo esté coordinado y sepa lo que está sucediendo, esto puede causar que otros pierdan trabajo o tengan historiales divergentes muy difíciles de reconciliar.
*   **Mensajes de Commit Claros:** Para `git revert`, asegúrate de que el mensaje del commit de revert explique por qué se está deshaciendo el cambio. Esto es vital para la trazabilidad.

Dominar `git reset` y `git revert` te dará mucha flexibilidad para manejar tu historial de Git, pero siempre recuerda el impacto potencial de cada comando, especialmente en entornos colaborativos.

---

**Siguiente Tema:** [Uso de Git Tag y Git Checkout para Gestión de Versiones y Revisión](06-git-tag-git-checkout.md)

**Volver al:** [Índice Principal](../README.md)
