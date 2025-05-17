# Fundamentos de Git: Configuración Inicial y Comandos Básicos

Una vez comprendida la importancia del control de versiones, el siguiente paso es asegurarnos de que Git esté correctamente instalado y configurado en nuestro sistema. Además, veremos cómo iniciar nuestro primer proyecto gestionado por Git. Trabajar con Git desde la terminal es una habilidad fundamental para cualquier desarrollador.

## 1. Verificación de la Instalación de Git

Antes de empezar, es crucial confirmar si Git ya se encuentra instalado en tu sistema operativo (Linux, macOS o Windows a través de WSL).

Para hacerlo:
1.  Abre tu terminal (o consola de comandos).
2.  Escribe el siguiente comando y presiona Enter:
    ```bash
    git --version
    ```
3.  **Resultado esperado:** Si Git está instalado, verás un mensaje con la versión de Git, por ejemplo: `git version 2.30.1`.
4.  **Si no aparece la versión:** Significa que Git no está instalado o no está correctamente configurado en el PATH de tu sistema. En este caso, necesitarás instalarlo.
    * Si necesitas instalar Git, puedes consultar **[esta guía de instalación oficial](https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Instalaci%C3%B3n-de-Git)**

## 2. Creación y Preparación del Primer Proyecto con Git

Con Git instalado, vamos a preparar el espacio para nuestro primer proyecto:

1.  **Limpiar la Terminal (Opcional pero Recomendado):** Para una mejor visibilidad, puedes limpiar la pantalla de la terminal.
    *   En Linux/macOS: `clear`
    *   En Windows (cmd/PowerShell): `cls`
2.  **Crear una Carpeta para el Proyecto:** Utiliza el comando `mkdir` (make directory) seguido del nombre que desees para tu proyecto.
    ```bash
    mkdir mi-primer-proyecto-git
    ```
3.  **Navegar a la Carpeta del Proyecto:** Ingresa a la carpeta recién creada usando el comando `cd` (change directory).
    ```bash
    cd mi-primer-proyecto-git
    ```
    Ahora, cualquier comando de Git que ejecutemos (relacionado con un repositorio) se aplicará dentro de esta carpeta.

## 3. Inicialización de un Repositorio Git

Una vez dentro de la carpeta del proyecto, el siguiente paso es decirle a Git que comience a rastrear esta carpeta. Esto se hace inicializando un repositorio.

*   Ejecuta el comando:
    ```bash
    git init
    ```
    Este comando crea una subcarpeta oculta llamada `.git` dentro de `mi-primer-proyecto-git`. Esta carpeta `.git` es donde Git almacena toda la información del historial, configuración y metadatos del repositorio.
    Al ejecutar `git init`, por defecto, se crea una rama principal. Tradicionalmente, esta rama se llamaba `master`.

### Cambiar el Nombre de la Rama Principal a `main` (Práctica Recomendada)

Actualmente, es una práctica común y recomendada nombrar la rama principal como `main` en lugar de `master` por razones de inclusividad.

*   **Para configurar `main` como la rama por defecto para todos los nuevos repositorios que crees globalmente:**
    ```bash
    git config --global init.defaultBranch main
    ```
    Después de ejecutar esto, cualquier nuevo `git init` creará la rama `main` por defecto.

*   **Si ya inicializaste el repositorio y se creó como `master`, puedes renombrarla a `main` para el proyecto actual:**
    ```bash
    git branch -m main
    ```
    Este comando (`-m` de move/rename) renombra la rama actual a `main`.

## 4. Configuración de Usuario en Git (¡Muy Importante!)

Git registra quién realiza cada cambio (commit). Para ello, necesitas configurar tu nombre de usuario y tu dirección de correo electrónico. Esta información se asociará con cada commit que realices.

*   **Configurar tu Nombre de Usuario:**
    ```bash
    git config --global user.name "Tu Nombre Completo o Apodo"
    ```
    Reemplaza `"Tu Nombre Completo o Apodo"` con tu información real.

*   **Configurar tu Correo Electrónico:**
    ```bash
    git config --global user.email "tu.correo@ejemplo.com"
    ```
    Reemplaza `"tu.correo@ejemplo.com"` con el correo electrónico que usas para desarrollo (idealmente el mismo que usarás en GitHub).

La opción `--global` indica que esta configuración se aplicará a todos tus repositorios Git en tu sistema. Si deseas una configuración diferente para un proyecto específico, puedes omitir `--global` y ejecutar los comandos dentro de ese repositorio.

**Tip para la Terminal:** Si cometes un error al escribir un comando, en la mayoría de las terminales puedes presionar la tecla de **flecha hacia arriba (↑)** para recuperar el último comando ejecutado y editarlo.

## 5. Confirmación de la Configuración de Git

Para verificar que tu nombre, correo y otras configuraciones (como la rama por defecto) se hayan guardado correctamente, puedes listar todas las configuraciones de Git:

```bash
git config --list
```

Busca las líneas user.name, user.email e init.defaultbranch para confirmar tus datos. También puedes ver configuraciones específicas:

```
git config user.name
git config user.email
```
6. ¿Qué Hacer si Olvidas un Comando de Git?

Git tiene una extensa documentación y una útil función de ayuda integrada.

Si necesitas un recordatorio general de los comandos más comunes, escribe:
```
git help
```

Si necesitas ayuda sobre un comando específico (por ejemplo, commit), puedes escribir:

```
git help commit
```
o

```
git commit --help
```

Esto abrirá la página del manual para ese comando, ofreciendo una descripción detallada y sus opciones.

**Siguiente Tema:** [Control de Versiones con Git: Comandos Básicos y Flujo de Trabajo](../03-control-versiones-comandos-flujo.md)

**Volver al:** [Índice Principal](../README.md)