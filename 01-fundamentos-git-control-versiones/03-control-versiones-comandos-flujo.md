# Control de Versiones con Git: Comandos Básicos y Flujo de Trabajo

Dominar Git desde el inicio puede parecer un desafío, pero es fundamental para cualquier desarrollador que desee llevar un registro ordenado de los cambios y gestionar las diferentes versiones de un proyecto. Siguiendo un flujo de trabajo claro y utilizando los comandos adecuados, el control de versiones se vuelve una herramienta poderosa.

## Flujo de Trabajo Básico en Git

El ciclo de vida de los cambios en Git generalmente sigue estos pasos:

1.  **Modificas archivos** en tu directorio de trabajo.
2.  **Seleccionas los cambios** que quieres incluir en tu próximo commit (los añades al área de "staging").
3.  **Realizas el commit**, que toma los archivos tal como están en el área de staging y almacena esa instantánea de forma permanente en tu repositorio Git (en la carpeta `.git`).

## 1. Inicio del Control de Versiones: `git init`

Como vimos anteriormente, el primer paso para que Git comience a rastrear un proyecto es inicializar un repositorio.

```bash
git init
```

Esto crea la carpeta oculta `.git` en el directorio raíz de tu proyecto. Esta carpeta es esencial, ya que actúa como la base de datos de Git, almacenando todo el historial y los metadatos del proyecto.

## 2. Creación y Adición de Archivos al Staging Area

Vamos a crear un archivo y ver cómo Git lo maneja.

### Creando un Archivo (Ejemplo con `nano`)

Puedes crear archivos con cualquier editor de texto. Desde la terminal, un editor sencillo es `nano`:

```bash
nano mi_primer_archivo.txt
```

Escribe algo de contenido, luego guarda y cierra el archivo (en `nano`, usualmente `Ctrl+O` para guardar, `Enter`, y `Ctrl+X` para salir).

### Verificando el Estado: `git status`

El comando `git status` es tu mejor amigo para entender qué está pasando en tu repositorio. Te muestra qué archivos han sido modificados, cuáles están en el staging area, y cuáles no están siendo rastreados.

```bash
git status
```

Después de crear `mi_primer_archivo.txt` y antes de añadirlo, `git status` te mostrará que hay un "Untracked file" (archivo no rastreado).

### Añadiendo Archivos al Staging Area: `git add`

Para indicarle a Git que quieres incluir los cambios de un archivo en el próximo commit, debes añadirlo al "staging area" (también conocido como "index" o "área de preparación").

```bash
git add mi_primer_archivo.txt
```

Si ahora ejecutas `git status` nuevamente, verás que `mi_primer_archivo.txt` aparece bajo "Changes to be committed" (cambios listos para ser confirmados).

El staging area es como un borrador o un espacio intermedio. Te permite agrupar varios cambios de diferentes archivos antes de empaquetarlos en un solo commit.

## 3. El Área de Staging (Staging Area / Index)

El área de staging es una característica poderosa de Git. Permite una revisión cuidadosa de los cambios antes de que se registren de forma permanente en el historial del repositorio. Los archivos en el staging area:

*   **No forman parte del historial de versiones todavía.**
*   Están "en espera" para ser incluidos en el próximo `git commit`.
*   Pueden ser removidos del staging area si decides no incluirlos en el commit.

### Remover un Archivo del Staging Area

Si añadiste un archivo al staging area por error o cambiaste de opinión, puedes removerlo (sin afectar el archivo en tu directorio de trabajo) usando:

```bash
git rm --cached mi_primer_archivo.txt
```

Después de esto, `git status` volverá a mostrar el archivo como "Untracked" (si era nuevo) o "Modified" pero no en staging (si ya existía y lo modificaste).

## 4. Realizando el Commit: `git commit`

Un "commit" es una instantánea de tu proyecto en un momento específico. Guarda los cambios que están en el staging area en el historial del repositorio local (la carpeta `.git`).

Para realizar un commit, se usa el comando `git commit` con la opción `-m` para incluir un mensaje descriptivo:

```bash
git commit -m "Añadido mi_primer_archivo.txt con contenido inicial"
```

**El mensaje del commit es crucial.** Debe ser claro, conciso y describir los cambios realizados. Buenos mensajes de commit facilitan la comprensión del historial del proyecto.

Después del commit, si ejecutas `git status`, te dirá "nothing to commit, working tree clean", lo que significa que todos los cambios rastreados han sido guardados.

## 5. Gestión de Múltiples Archivos

A menudo trabajarás con varios archivos a la vez.

*   **Añadir todos los archivos modificados y nuevos al staging area:**

    ```bash
    git add .
    ```

    El `.` representa el directorio actual y todos sus contenidos. Úsalo con precaución para asegurarte de que solo añades lo que realmente quieres.

*   **Añadir archivos específicos:**

    ```bash
    git add archivo1.txt archivo2.js carpeta_completa/
    ```

Puedes decidir si hacer un commit por cada cambio lógico pequeño o agrupar varios cambios relacionados en un solo commit.

## 6. Visualización del Historial de Cambios: `git log`

Para ver el historial de commits realizados en tu proyecto, utiliza:

```bash
git log
```

Este comando muestra una lista de los commits, desde el más reciente al más antiguo. Cada entrada de log incluye:
*   El hash del commit (un identificador único).
*   El autor (nombre y correo configurados).
*   La fecha del commit.
*   El mensaje del commit.

Existen muchas opciones para `git log` que permiten personalizar la salida (ej. `git log --oneline`, `git log --graph`).

## 7. Modificación de Archivos Existentes

Cuando editas un archivo que ya está siendo rastreado por Git (es decir, ya ha sido parte de al menos un commit):

1.  Modifica el archivo con tu editor.
2.  `git status` te mostrará el archivo como "modified".
3.  Para incluir estos cambios en el próximo commit, debes añadirlos de nuevo al staging area:

    ```bash
    git add nombre_del_archivo_modificado.txt
    ```

4.  Luego, realiza el commit:

    ```bash
    git commit -m "Descripción de la modificación realizada en el archivo"
    ```

Este flujo (modificar -> `git add` -> `git commit`) se repite constantemente durante el desarrollo. Asegura que Git mantenga un registro detallado de cada cambio, actualización o eliminación.

## 8. Manejo de Diferentes Tipos de Archivos por Git

Git es agnóstico al tipo de archivo. Trata de igual manera archivos de texto plano (`.txt`, `.md`), código fuente (`.js`, `.py`, `.java`), imágenes (`.png`, `.jpg`), binarios, etc. El proceso de `git add` y `git commit` es el mismo para registrar cambios en cualquier tipo de archivo.

---

## Términos Básicos de Terminal y Git (Resumen Rápido)

Aquí tienes una lista de comandos básicos de terminal y de Git que son útiles en el día a día:

**Comandos de Navegación y Directorios (Terminal):**
*   `cd <nombre_directorio>`: Cambiar directorio (entrar a una carpeta).
*   `cd ..`: Retroceder un nivel de directorio (salir de la carpeta actual a la anterior).
*   `cd` (o `cd ~`): Ir al directorio raíz del usuario (home).
*   `mkdir <nombre_directorio>`: Crear un nuevo directorio (carpeta).
*   `rmdir <nombre_directorio>`: Remover un directorio vacío. (Para directorios con contenido, se usa `rm -r <nombre_directorio>` con cuidado).
*   `ls` (en Linux/macOS) o `dir` (en Windows): Listar el contenido de un directorio.
*   `pwd` (en Linux/macOS): Mostrar el directorio de trabajo actual (print working directory).

**Comandos Básicos de Git:**
*   `git init`: Inicializa un nuevo repositorio Git en el directorio actual.
*   `git status`: Muestra el estado de los archivos en el directorio de trabajo y el staging area.
*   `git add <archivo_o_directorio>`: Añade cambios de archivos al staging area.
    *   `git add .`: Añade todos los cambios del directorio actual y subdirectorios.
*   `git commit -m "mensaje descriptivo"`: Guarda los cambios del staging area en el historial del repositorio.
*   `git log`: Muestra el historial de commits.
*   `git rm --cached <archivo>`: Remueve un archivo del staging area (pero no del directorio de trabajo).
*   `git diff`: Muestra las diferencias entre el directorio de trabajo y el staging area.
*   `git diff --staged`: Muestra las diferencias entre el staging area y el último commit.

---

**Siguiente Tema:** [Gestión de ramas en Git: Creación, Fusión y Eliminación Eficiente](04-gestion-ramas.md)

**Volver al:** [Índice Principal](../README.md)