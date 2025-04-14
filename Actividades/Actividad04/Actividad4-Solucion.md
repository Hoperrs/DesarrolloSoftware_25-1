## Actividad: Introducción a Git - conceptos básicos y operaciones esenciales

### Preguntas

- #### ¿Cómo te ha ayudado Git a mantener un historial claro y organizado de tus cambios?

    Git me ha facilitado enormemente el mantener mi código organizado. Puedo ver todos los cambios que hice con `git log`, volver a versiones anteriores si algo falla y trabajar en diferentes funcionalidades con ramas.

- #### ¿Qué beneficios ves en el uso de ramas para desarrollar nuevas características o corregir errores?

    Git me permite crear ramas para poder trabajar en nuevas funcionalidades sin afectar mi código principal. Así puedo experimentar nuevas características, corregir errores en paralelo y luego integrarlo todo cuando esté listo.


### Ejercicios

- #### Ejercicio 1: Manejo avanzado de ramas y resolución de conflictos

    1. <u>Crear una nueva rama para una característica:</u>

        Se crea la rama `feature/advanced-feature` desde la rama `main`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git branch
        feature/another-new-feature
        feature/another-new-feature2
        * main
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git checkout -b feature/advanced-feature
        Cambiado a nueva rama 'feature/advanced-feature'
        ```

    2. <u>Modificar archivos en la nueva rama:</u>

        Se edita el archivo `main.py` para incluir una función adicional.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ cat main.py 
        print('Hello World')

        def greet():
            print('Hello from advanced feature branch')

        greet()
        ```

        Se añade y confirma los cambios en la rama `feature/advanced-feature`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add main.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "Add greet function in advanced feature"
        [feature/advanced-feature 57055f9] Add greet function in advanced feature
        1 file changed, 5 insertions(+)
        ```

    3. <u>Simular un desarrollo paralelo en la rama main:</u>

        Se cambia de nuevo a la rama `main`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git checkout main
        Cambiado a rama 'main'
        ```

        Se edita el archivo `main.py`.

        ```python
        print('Hello World - updated in main')
        ```

        Se añade y confirma los cambios en la rama `main`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add main.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "Updated main.py message in main branch"
        [main fb4e021] Updated main.py message in main branch
        1 file changed, 1 insertion(+), 1 deletion(-)
        ```

    4. <u>Intentar fusionar la rama feature/advanced-feature en main:</u>

        Se fusiona la rama `feature/advanced-feature` en `main`:

        ```bash
        operrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git merge feature/advanced-feature 
        Auto-fusionando main.py
        CONFLICTO (contenido): Conflicto de fusión en main.py
        Fusión automática falló; arregle los conflictos y luego realice un commit con el resultado.
        ```

    5. <u>Resolver el conflicto de fusión:</u>

        Se genera un conflicto en `main.py`.

        ```python
        <<<<<<< HEAD
        print('Hello World - updated in main')
        =======
        print('Hello World')

        def greet():
            print('Hello from advanced feature branch')

        greet()
        >>>>>>> feature/advanced-feature
        ```

        Se resuelve el conflico manualmente.

        ```python
        print('Hello World - updated in main')

        def greet():
            print('Hello from advanced feature branch')

        greet()
        ```

        Se añade el archivo resuelto y completa la fusión.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add main.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "Resolve merge conflict between main and feature/advanced-feature"
        [main f175f37] Resolve merge conflict between main and feature/advanced-feature
        ```

    6. <u>Eliminar la rama fusionada:</u>

        Después de la fusión con éxito, se elimina la rama `feature/advanced-feature`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git branch -d feature/advanced-feature 
        Eliminada la rama feature/advanced-feature (era 57055f9).
        ```


- #### Ejercicio 2: Exploración y manipulación del historial de commits

    1. <u>Ver el historial detallado de commits:</u>

        Se usa el comando `git log` para explorar el historial de commits. Con el parámetro `-p` se pueden observar las diferencias y cambios realizados en cada commit.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git log -p
        commit f175f37f0fcfb501477c75da11558500538a2560 (HEAD -> main)
        Merge: fb4e021 57055f9
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 08:59:27 2025 -0500

            Resolve merge conflict between main and feature/advanced-feature

        commit fb4e02105bec87ce2ef8f78b970975c18680ca93
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 08:36:58 2025 -0500

            Updated main.py message in main branch

        diff --git a/main.py b/main.py
        index df1dc68..971e8f4 100644
        --- a/main.py
        +++ b/main.py
        @@ -1 +1 @@
        -print('Hello World')
        +print('Hello World - updated in main')

        commit 57055f9c9d48348c6c48ef88e798e1143b2b23f9
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 08:30:22 2025 -0500

            Add greet function in advanced feature

        diff --git a/main.py b/main.py
        index df1dc68..1b4fc93 100644
        --- a/main.py
        +++ b/main.py
        @@ -1 +1,6 @@
        print('Hello World')
        +
        +def greet():
        +    print('Hello from advanced feature branch')
        +
        +greet()

        commit 4943b41ed6bd7d4963457eb7de18f12b82d10115 (feature/another-new-feature2, feature/another-new-feature)
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 07:32:15 2025 -0500

            Add main.py

        diff --git a/main.py b/main.py
        new file mode 100644
        index 0000000..df1dc68
        --- /dev/null
        +++ b/main.py
        @@ -0,0 +1 @@
        +print('Hello World')

        commit 6fad0ba3652e0cc963eb407995374d7e18002d1a
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 07:28:15 2025 -0500

            Set up the repository base documentation

        diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
        new file mode 100644
        index 0000000..2e8cc63
        --- /dev/null
        +++ b/CONTRIBUTING.md
        @@ -0,0 +1 @@
        + CONTRIBUTING
        diff --git a/README.md b/README.md
        index 2772834..69aa3ec 100644
        --- a/README.md
        +++ b/README.md
        @@ -1 +1 @@
        - README
        + README\n\nWelcome to the project

        commit 323754f88d5fd42cbc6e21a66b5be6815be115d4
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 07:19:20 2025 -0500

            Initial commit with README.md

        diff --git a/README.md b/README.md
        new file mode 100644
        index 0000000..2772834
        --- /dev/null
        +++ b/README.md
        @@ -0,0 +1 @@
        + README
        (END)
        ```

    2. <u>Filtrar commits por autor:</u>

        Se usa el parámetro `--author=<nombre>` para filtrar los commits realizados por un autor específico.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git log --author="hoperrs"
        commit f175f37f0fcfb501477c75da11558500538a2560 (HEAD -> main)
        Merge: fb4e021 57055f9
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 08:59:27 2025 -0500

            Resolve merge conflict between main and feature/advanced-feature

        commit fb4e02105bec87ce2ef8f78b970975c18680ca93
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 08:36:58 2025 -0500

            Updated main.py message in main branch

        commit 57055f9c9d48348c6c48ef88e798e1143b2b23f9
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 08:30:22 2025 -0500

            Add greet function in advanced feature

        commit 4943b41ed6bd7d4963457eb7de18f12b82d10115 (feature/another-new-feature2, feature/another-new-feature)
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 07:32:15 2025 -0500

            Add main.py

        commit 6fad0ba3652e0cc963eb407995374d7e18002d1a
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 07:28:15 2025 -0500

            Set up the repository base documentation

        commit 323754f88d5fd42cbc6e21a66b5be6815be115d4
        Author: hoperrs <kchavezc19@gmail.com>
        Date:   Sat Apr 12 07:19:20 2025 -0500

            Initial commit with README.md
        (END)
        ```

    3. <u>Revertir un commit:</u>

        En el caso de que el commit más reciente en `main.py` no debería haberse hecho. Se usa `git revert` para revertir el commit.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git revert HEAD
        error: el commit f175f37f0fcfb501477c75da11558500538a2560 es una fusión pero no se proporcionó la opción -m.
        fatal: falló al revertir
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git revert -m 1 HEAD
        [main c87edf5] Revert "Resolve merge conflict between main and feature/advanced-feature"
        1 file changed, 5 deletions(-)
        ```

        Se usa el parámetro `-m 1` para conservar los cambios solo del primer commit que se usó en el merge.

    4. <u>Rebase interactivo:</u>

        Se realiza un rebase interactivo para combinar varios commits en uno solo. Se usa `git rebase -i HEAD~3` para combinar los últimos trescommits en uno solo  usando la opción `squash`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git rebase -i HEAD~3
        Auto-fusionando main.py
        CONFLICTO (contenido): Conflicto de fusión en main.py
        error: no se pudo aplicar 57055f9... Add greet function in advanced feature
        hint: Resolve all conflicts manually, mark them as resolved with
        hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
        hint: You can instead skip this commit: run "git rebase --skip".
        hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
        hint: Disable this message with "git config set advice.mergeConflict false"
        No se pudo aplicar 57055f9... Add greet function in advanced feature
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add main.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git rebase --continue
        [HEAD desacoplado d4f3cf2] Add greet function in advanced feature
        1 file changed, 5 insertions(+)
        Rebase aplicado satisfactoriamente y actualizado refs/heads/main.
        ```

    5. <u>Visualización gráfica del historial:</u>

        Se visualiza una representación gráfica del historial de commits.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git log --graph --oneline --all
        * 39a0fac (HEAD -> main) Revert "Resolve merge conflict between main and feature/advanced-feature"
        * d4f3cf2 Add greet function in advanced feature
        * fb4e021 Updated main.py message in main branch
        * 4943b41 (feature/another-new-feature2, feature/another-new-feature) Add main.py
        * 6fad0ba Set up the repository base documentation
        * 323754f Initial commit with README.md
        ```


- #### Ejercicio 3: Creación y gestión de ramas desde commits específicos

    1. <u>Crear una nueva rama desde un commit específico:</u>

        Se usa `git log --oneline` para ver el historial de commits e identificar un commit antigup desde el cual crear una nueva rama.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git log --oneline
        39a0fac (HEAD -> main) Revert "Resolve merge conflict between main and feature/advanced-feature"
        d4f3cf2 Add greet function in advanced feature
        fb4e021 Updated main.py message in main branch
        4943b41 (feature/another-new-feature2, feature/another-new-feature) Add main.py
        6fad0ba Set up the repository base documentation
        323754f Initial commit with README.md
        ```

        Se crea una nueva rama `bugfix/rollback-feature` desde ese commit.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git checkout -b bugfix/rollback-feature d4f3cf2
        Cambiado a nueva rama 'bugfix/rollback-feature'
        ```  

    2. <u>Modificar y confirmar cambios en la nueva rama:</u>

        Se realiza algunas modificaciones en `main.py` que simulen una correción de errores.

        ```python
        print('Hello World - updated in main')

        def greet():
            print('Fixed bug in feature')

        greet()
        ```

        Se añaden y confirman los cambios en la nueva rama.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add main.py
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "Fix bug in rollback feature"
        [bugfix/rollback-feature df9cf27] Fix bug in rollback feature
        1 file changed, 1 insertion(+), 1 deletion(-)
        ```

    3. <u>Fusionar los cambios en la rama principal:</u>

        Se cambia de nuevo a la rama `main` y se fusiona la rama `bugfix/rollback-feature`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git checkout main
        Cambiado a rama 'main'
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git merge bugfix/rollback-feature 
        Auto-fusionando main.py
        CONFLICTO (contenido): Conflicto de fusión en main.py
        Fusión automática falló; arregle los conflictos y luego realice un commit con el resultado.
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add main.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "Merge de main y bugfix/rollback-feature"
        [main 0f88e11] Merge de main y bugfix/rollback-feature
        ```

    4. <u>Explorar el historial después de la fusión:</u>

        Se usar `git log --graph` para ver cómo se ha integrado en el historial de commits.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git log --graph --oneline
        *   0f88e11 (HEAD -> main) Merge de main y bugfix/rollback-feature
        |\  
        | * df9cf27 (bugfix/rollback-feature) Fix bug in rollback feature
        * | 39a0fac Revert "Resolve merge conflict between main and feature/advanced-feature"
        |/  
        * d4f3cf2 Add greet function in advanced feature
        * fb4e021 Updated main.py message in main branch
        * 4943b41 (feature/another-new-feature2, feature/another-new-feature) Add main.py
        * 6fad0ba Set up the repository base documentation
        * 323754f Initial commit with README.md
        ```

    5. <u>Eliminar la rama bugfix/rollback-feature:</u>

        Luego de fusionados los cambios, se elimina la rama `bugfix/rollback-feature`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git branch -d bugfix/rollback-feature 
        Eliminada la rama bugfix/rollback-feature (era df9cf27).
        ```


- #### Ejercicio 4: Manipulación y restauración de commits con git reset y git restore

    1. <u>Hacer cambios en el archivo main.py:</u>

        Se edita el archivo `main.py` para introducir un nuevo cambio.

        ```python
        print('Hello World - updated in main')

        def greet():
            print('Fixed bug in feature')

        greet()

        print('This change will be reset')
        ```

        Se añade y confirma los cambios.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add main.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "Introduce a change to be reset"
        [main 610fe73] Introduce a change to be reset
        1 file changed, 2 insertions(+), 3 deletions(-)
        ```

    2. <u>Usar git reset para deshacer el commit:</u>

        Se deshace el commit utilizando `git reset` para volver al estado anterior.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git reset --hard HEAD~1
        HEAD está ahora en 0f88e11 Merge de main y bugfix/rollback-feature
        ```

    3. <u>Usar git restore para deshacer cambios no confirmados:</u>

        Se realiza un cambio en `README.md` sin confirmar.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ echo "Another line in README" >> README.md
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git status
        En la rama main
        Cambios no rastreados para el commit:
        (usa "git add <archivo>..." para actualizar lo que será confirmado)
        (usa "git restore <archivo>..." para descartar los cambios en el directorio de trabajo)
                modificados:     README.md
                modificados:     main.py

        sin cambios agregados al commit (usa "git add" y/o "git commit -a")
        ```

        Se usa `git restore` para deshacer el cambio no confirmado. Y se verifica que el cambio no confirmado ha sido revertido.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git restore README.md 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ cat README.md 
        README\n\nWelcome to the project
        ```


- #### Ejercicio 5: Trabajo colaborativo y manejo de Pull Requests

    1. <u>Crear un nuevo repositorio remoto:</u>

        Se usa GitHub para crear un nuevo repositorio remoto y clonarlo localmente.

        ```bash
        hoperrs@hoperrs:~/Escritorio$ git clone git@github.com:Hoperrs/DesarrolloSoftware_25-1.git
        Clonando en 'DesarrolloSoftware_25-1'...
        remote: Enumerating objects: 14, done.
        remote: Counting objects: 100% (14/14), done.
        remote: Compressing objects: 100% (9/9), done.
        remote: Total 14 (delta 0), reused 14 (delta 0), pack-reused 0 (from 0)
        Recibiendo objetos: 100% (14/14), 25.41 KiB | 4.23 MiB/s, listo.
        ```

    2. <u>Crear una nueva rama para desarrollo de una característica:</u>

        En el repositorio local, se crea una nueva rama `feature/team-feature`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DesarrolloSoftware_25-1$ git checkout -b feature/team-feature
        Cambiado a nueva rama 'feature/team-feature'
        ```

    3. <u>Realizar cambios y enviar la rama al repositorio remoto:</u>

        Se realizan cambios en los archivos del proyecto y se confirman.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DesarrolloSoftware_25-1$ echo 'print("Collaboration is key!")' > ./Actividades/Actividad04/collaboration.py
        hoperrs@hoperrs:~/Escritorio/DesarrolloSoftware_25-1$ git add .
        hoperrs@hoperrs:~/Escritorio/DesarrolloSoftware_25-1$ git commit -m "Add collaboration script"
        [feature/team-feature e29f18a] Add collaboration script
        1 file changed, 1 insertion(+)
        create mode 100644 Actividades/Actividad04/collaboration.py
        ```

        Se envía la rama al repositorio remoto.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DesarrolloSoftware_25-1$ git push origin feature/team-feature 
        Enumerando objetos: 8, listo.
        Contando objetos: 100% (8/8), listo.
        Compresión delta usando hasta 12 hilos
        Comprimiendo objetos: 100% (3/3), listo.
        Escribiendo objetos: 100% (5/5), 425 bytes | 425.00 KiB/s, listo.
        Total 5 (delta 2), reused 0 (delta 0), pack-reused 0 (from 0)
        remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
        remote: 
        remote: Create a pull request for 'feature/team-feature' on GitHub by visiting:
        remote:      https://github.com/Hoperrs/DesarrolloSoftware_25-1/pull/new/feature/team-feature
        remote: 
        To github.com:Hoperrs/DesarrolloSoftware_25-1.git
        * [new branch]      feature/team-feature -> feature/team-feature
        ```

    4. <u>Abrir un Pull Request:</u>

        Se abre un Pull Request (PR) en el repositorio remoto de GitHub para fusionar `feature/team-feature` con la rama `main`.

        <div style="text-align:center">
            <img src="./img/01-.png" style="width:auto; max-width:60%">
        </div>

        Se añade una descripción detallada del PR explicando los cambios y su propósito.

        <div style="text-align:center">
            <img src="./img/02-.png" style="width:auto; max-width:60%">
        </div>

    5. <u>Revisar y fusionar el Pull Request:</u>

        Se revisa y una vez aprobado, se fusiona el PR en la rama `main`.

        <div style="text-align:center">
            <img src="./img/03-.png" style="width:auto; max-width:60%">
        </div>

        <div style="text-align:center">
            <img src="./img/04-.png" style="width:auto; max-width:60%">
        </div>

    6. <u>Eliminar la rama remota y local:</u>

        Después de la fusión, se elimina la rama local y remota.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DesarrolloSoftware_25-1$ git branch -D feature/team-feature 
        Eliminada la rama feature/team-feature (era e29f18a).
        hoperrs@hoperrs:~/Escritorio/DesarrolloSoftware_25-1$ git push origin --delete feature/team-feature
        To github.com:Hoperrs/DesarrolloSoftware_25-1.git
        - [deleted]         feature/team-feature
        ```


- #### Ejercicio 6: Cherry-Picking y Git Stash

    1. <u>Hacer cambios en main.py y confirmarlos:</u>

        Se realiza y confirma varios cambios de `main.py` en la rama `main`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ echo 'print("Cherry pick this!")' >> cherry.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add cherry.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "Add cherry-pick example"
        [main ff42aa3] Add cherry-pick example
        1 file changed, 1 insertion(+)
        create mode 100644 cherry.py
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ echo 'print("Second change")' >> cherry.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add cherry.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "Segundo cambio"
        [main 4b35ee8] Segundo cambio
        1 file changed, 1 insertion(+)
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ echo 'print("Thrid change")' >> cherry.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add cherry.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "Tercer cambio"
        [main 823e9f5] Tercer cambio
        1 file changed, 1 insertion(+)
        ```

    2. <u>Crear una nueva rama y aplicar el commit específico:</u>

        Se crea una nueva rama `feature/cherry-pick` y se aplica el commit específico.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git checkout -b feature/cherry-pick
        Cambiado a nueva rama 'feature/cherry-pick'
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git cherry-pick 4b35ee8
        Auto-fusionando cherry.py
        CONFLICTO (contenido): Conflicto de fusión en cherry.py
        error: no se pudo aplicar 4b35ee8... Segundo cambio
        hint: Luego de resolver los conflictos, márquelos con
        hint: "git add/rm <pathspec>", luego ejecute
        hint: "git cherry-pick --continue".
        hint: O puede saltar este commit con "git cherry-pick --skip".
        hint: Para abortar y regresar al estado anterior a "git cherry-pick",
        hint: ejecute "git cherry-pick --abort".
        hint: Disable this message with "git config set advice.mergeConflict false"
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add cherry.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git cherry-pick --continue
        [feature/cherry-pick c196c58] Segundo cambio
        Date: Sun Apr 13 18:23:15 2025 -0500
        1 file changed, 1 deletion(-)
        ```

    3. <u>Guardar temporalmente cambios no confirmados:</u>

        Se realiza algunos cambios en `main.py` pero sin confirmar.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ echo "This change is stashed" >> main.py
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git status
        En la rama feature/cherry-pick
        Cambios no rastreados para el commit:
        (usa "git add <archivo>..." para actualizar lo que será confirmado)
        (usa "git restore <archivo>..." para descartar los cambios en el directorio de trabajo)
                modificados:     main.py

        sin cambios agregados al commit (usa "git add" y/o "git commit -a")
        ```

        Se guarda temporalmente este cambio utilizando `git stash`.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git stash
        Directorio de trabajo y estado de índice WIP on feature/cherry-pick: c196c58 Segundo cambio guardados
        ```

    4. <u>Aplicar los cambios guardados:</u>

        Se realiza otro cambio y se confirma.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ echo "Otro cambio" >> main.py
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git add main.py 
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git commit -m "agregando otro cambio"
        [feature/cherry-pick 36afc7f] agregando otro cambio
        1 file changed, 1 insertion(+)
        ```

        Luego se recupera los cambios guardados anteriormente.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git stash pop
        Auto-fusionando main.py
        CONFLICTO (contenido): Conflicto de fusión en main.py
        En la rama feature/cherry-pick
        Rutas no fusionadas:
        (usa "git restore --staged <archivo>..." para sacar del área de stage)
        (usa "git add <archivo>..." para marcar una resolución)
                modificados por ambos:  main.py

        sin cambios agregados al commit (usa "git add" y/o "git commit -a")
        La entrada de stash se guardó en caso de ser necesario nuevamente.
        ```

        Visualizando el cambio recuperado.

        ```python
        print('Hello World - updated in main')

        def greet():
            print('Fixed bug in feature')

        greet()
        <<<<<<< Updated upstream
        Otro cambio
        =======
        This change is stashed
        >>>>>>> Stashed changes
        ```

    5. <u>Revisar el historial y confirmar la correcta aplicación de los cambios:</u>

        Se usa `git log` para revisar el historial de commits y verificar que todos los cambios se han aplicado correctamente.

        ```bash
        hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad04/hoperrs-repo$ git log --oneline --graph
        * 36afc7f (HEAD -> feature/cherry-pick) agregando otro cambio
        * c196c58 Segundo cambio
        * 823e9f5 (main) Tercer cambio
        * 4b35ee8 Segundo cambio
        * ff42aa3 Add cherry-pick example
        * 6dbb3f3 Confirmando cambios
        *   0f88e11 Merge de main y bugfix/rollback-feature
        |\  
        | * df9cf27 Fix bug in rollback feature
        * | 39a0fac Revert "Resolve merge conflict between main and feature/advanced-feature"
        |/  
        * d4f3cf2 Add greet function in advanced feature
        * fb4e021 Updated main.py message in main branch
        * 4943b41 Add main.py
        * 6fad0ba Set up the repository base documentation
        * 323754f Initial commit with README.md
        ```

---