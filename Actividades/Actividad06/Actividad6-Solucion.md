## **Actividad: Rebase, Cherry-Pick y CI/CD en un entorno ágil**

### Objetivo de aprendizaje:  

Aprender a usar los comandos `git rebase` y `git cherry-pick` para mantener un historial de commits limpio y manejable en proyectos colaborativos.  También explorarás cuándo y por qué utilizar estos comandos en lugar de los merges regulares.

#### **Parte 1: git rebase para mantener un historial lineal**

1. **Introducción a Rebase:**

   El rebase mueve tus commits a una nueva base, dándote un historial lineal y limpio. En lugar de fusionar ramas y mostrar un "commit de merge", el rebase integra los cambios aplicándolos en la parte superior de otra rama.

   - **Caso de uso**: Simplifica la depuración y facilita la comprensión del historial de commits.

2. **Escenario de ejemplo:**

   - Crea un nuevo repositorio Git y dos ramas, main y new-feature:
     ```bash
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ mkdir prueba-git-rebase
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ cd prueba-git-rebase
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git init
      Inicializado repositorio Git vacío en /home/hoperrs/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase/.git/
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ echo "# Mi Proyecto de Rebase" > README.md
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git add README.md
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git commit -m "Commit inicial en main"
      [main (commit-raíz) 27b5c7a] Commit inicial en main
      1 file changed, 1 insertion(+)
      create mode 100644 README.md
     ```

   - Crea y cambia a la rama new-feature:
     ```bash
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git checkout -b new-feature
      Cambiado a nueva rama 'new-feature'
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ echo "Esta es una nueva característica." > NewFeature.md
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git add NewFeature.md
      hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git commit -m "Agregar nueva característica"
      [new-feature e881ab4] Agregar nueva característica
      1 file changed, 1 insertion(+)
      create mode 100644 NewFeature.md
     ```

   **Pregunta:** Presenta el historial de ramas obtenida hasta el momento.

   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git log --oneline --graph
   * e881ab4 (HEAD -> new-feature) Agregar nueva característica
   * 27b5c7a (main) Commit inicial en main
   ```

   Ahora, digamos que se han agregado nuevos commits a main mientras trabajabas en new-feature:

   ```bash
   # Cambiar de nuevo a 'main' y agregar nuevos commits
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git checkout main
   Cambiado a rama 'main'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ echo "Updates to the project." >> Updates.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git add Updates.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git commit -m "Update main"
   [main 1130244] Update main
   1 file changed, 1 insertion(+)
   create mode 100644 Updates.md
   ```

   Tu gráfico de commits ahora diverge (comprueba esto)

   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git log --oneline --graph
   * 1130244 (HEAD -> main) Update main
   * 27b5c7a Commit inicial en main
   ```

   > **Tarea**: Realiza el rebase de `new-feature` sobre `main` con los siguientes comandos:
   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git checkout new-feature 
   Cambiado a rama 'new-feature'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git rebase main
   Rebase aplicado satisfactoriamente y actualizado refs/heads/new-feature.
   ```

3. **Revisión:**

   Después de realizar el rebase, visualiza el historial de commits con:
   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git log --graph --oneline
   * f5feb93 (HEAD -> new-feature) Agregar nueva característica
   * 1130244 (main) Update main
   * 27b5c7a Commit inicial en main
   ```

4. **Momento de fusionar y completar el proceso de git rebase:**
   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git checkout main
   Cambiado a rama 'main'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git merge new-feature
   Actualizando 1130244..f5feb93
   Fast-forward
   NewFeature.md | 1 +
   1 file changed, 1 insertion(+)
   create mode 100644 NewFeature.md
   ```

   Cuando se realiza una fusión *fast-forward*, las HEADs de las ramas main y new-feature serán los commits correspondientes.

   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-git-rebase$ git log --oneline --graph
   * f5feb93 (HEAD -> main, new-feature) Agregar nueva característica
   * 1130244 Update main
   * 27b5c7a Commit inicial en main
   ```

#### Parte 2: **git cherry-pick para la integración selectiva de commit**

1. **Introducción a Cherry-pick:**

   `git cherry-pick` te permite seleccionar commits individuales de una rama y aplicarlos en otra. Esto es útil cuando necesitas integrar una característica o corrección sin hacer merge de toda la rama.

   Imagina que tienes dos ramas, main y feature. Te das cuenta de que uno o dos commits de la rama feature deberían moverse a main, pero no estás listo para fusionar toda la rama. El comando `git cherry-pick` te permite hacer precisamente eso.

   Puedes hacer cherry-pick de los cambios de un commit específico en la rama feature y aplicarlos en la rama main.
   Esta acción creará un nuevo commit en la rama main.


3. **Escenario de ejemplo:**

   ```bash
   # Inicializar un nuevo repositorio
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ mkdir prueba-cherry-pick
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ cd prueba-cherry-pick
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git init
   Inicializado repositorio Git vacío en /home/hoperrs/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick/.git/

   # Agregar y commitear README.md inicial a main
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ echo "# Mi Projecto" > README.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git add README.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git commit -m "Commit inicial"
   [main (commit-raíz) fa5f85e] Commit inicial
   1 file changed, 1 insertion(+)
   create mode 100644 README.md

   # Crear y cambiar a una nueva rama 'add-base-documents'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git checkout -b add-base-documents
   Cambiado a nueva rama 'add-base-documents'

   # Hacer cambios y commitearlos
   # Agregar CONTRIBUTING.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ echo "# CONTRIBUTING" >> CONTRIBUTING.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git add CONTRIBUTING.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git commit -m "Se agrega CONTRIBUTING.md"
   [add-base-documents e07d3e3] Se agrega CONTRIBUTING.md
   1 file changed, 1 insertion(+)
   create mode 100644 CONTRIBUTING.md

   # Agregar LICENSE.txt
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ echo "LICENSE" >> LICENSE.txt
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git add LICENSE.txt
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git commit -m "Agrega LICENSE.txt"
   [add-base-documents 07bfeba] Agrega LICENSE.txt
   1 file changed, 1 insertion(+)
   create mode 100644 LICENSE.txt

   # Echa un vistazo al log de la rama 'add-base-documents'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git log add-base-documents --graph --oneline
   * 07bfeba (HEAD -> add-base-documents) Agrega LICENSE.txt
   * e07d3e3 Se agrega CONTRIBUTING.md
   * fa5f85e (main) Commit inicial
   ```

    **Pregunta:** Muestra un diagrama de como se ven las ramas en este paso.

   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git log --oneline --graph
   * 07bfeba (HEAD -> add-base-documents) Agrega LICENSE.txt
   * e07d3e3 Se agrega CONTRIBUTING.md
   * fa5f85e (main) Commit inicial
   ```

4. **Tarea: Haz cherry-pick de un commit de add-base-documents a main:**
   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git checkout main
   Cambiado a rama 'main'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git cherry-pick e07d3e3
   [main ba80f31] Se agrega CONTRIBUTING.md
   Date: Sat Apr 19 18:28:08 2025 -0500
   1 file changed, 1 insertion(+)
   create mode 100644 CONTRIBUTING.md
   ```

5. **Revisión:**  
   Revisa el historial nuevamente:
   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/prueba-cherry-pick$ git log --graph --oneline
   * ba80f31 (HEAD -> main) Se agrega CONTRIBUTING.md
   * fa5f85e Commit inicial
   ```
   Después de que hayas realizado con éxito el cherry-pick del commit, se agregará un nuevo commit a tu rama actual (main en este ejemplo) y contendrá los cambios del commit cherry-picked.  

   Ten en cuenta que el nuevo commit tiene los mismos cambios pero un valor de hash de commit diferente. !Comprueba esto!.


##### **Preguntas de discusión:**

1. ¿Por qué se considera que rebase es más útil para mantener un historial de proyecto lineal en comparación con merge?

   Porque produce un historial lineal más limpio al evitar los commits de fusión y reorganiza los commits en una secuencia cronológica clara.

2. ¿Qué problemas potenciales podrían surgir si haces rebase en una rama compartida con otros miembros del equipo?

   Realizar rebase en ramas compartidas puede causar: Conflictos de historial porque rebase reescribe el historial de commits generando diferencias para quienes ya tienen una versión anterior. Problemas en push/pull ya que los colaboradores tendrán errores al querer sincronizar y requerirán usar `--force`. Y  perdida de contexto al perder la cronología real del proyecto y cuándo se integraron ciertos cambios.

3. ¿En qué se diferencia cherry-pick de merge, y en qué situaciones preferirías uno sobre el otro?

   Cherry-pick aplica commits específicos seleccionados individualmente, crea nuevos commits con distinto hash pero con los mismos cambios sin tener que traer toda la rama. Cherry-pick es útil para aplicar un fix urgente o una funcionalidad importante sin tener que aplicar todos los cambios de la rama. Merge integra todos los cambios de una rama completa a través de un commit de fusión y preserva el historial completo y la relación entre las ramas. Merge es útil cuando se quiere integrar completamente el trabajo de una feature y mantener el historial completo con su trazabilidad.

4. ¿Por qué es importante evitar hacer rebase en ramas públicas?

   Porque dificulta el rastreo de cambios, rompe la sincronización, cambia los hashes de los commits creando versiones incompatibles del historial, esto crea conflictos para los colaboradores y requiere push forzado solucionarlo.



#### **Ejercicios teóricos**

1. **Diferencias entre git merge y git rebase**  
   **Pregunta**: Explica la diferencia entre git merge y git rebase y describe en qué escenarios sería más adecuado utilizar cada uno en un equipo de desarrollo ágil que sigue las prácticas de Scrum.

   Git merge crea un nuevo commit que une dos ramas y preserva el historial y cronología de los commits. Git rebase traslada los commits de una rama a otra modificando los hashes de los commits originales. Se recomienda usar git merge cuando se busca integrar una rama feature completa a main y se quiere preservar el contexto. Y se recomienda usar git rebase cuando se quiere actualizar las rama feature con los últimos cambios de main, mantiene un historial de commits limpio y lineal.

2. **Relación entre git rebase y DevOps**  
   **Pregunta**: ¿Cómo crees que el uso de git rebase ayuda a mejorar las prácticas de DevOps, especialmente en la implementación continua (CI/CD)? Discute los beneficios de mantener un historial lineal en el contexto de una entrega continua de código y la automatización de pipelines.

   En el contexto de CI/CD, rebase ayuda a mantener un flujo de trabajo donde cada cambio puede ser probrado totalmente, validado y desplegado de manera independiente desde la rama feature, esto es fundamental para la entrega continua y confiable de software. 

3. **Impacto del git cherry-pick en un equipo Scrum**  
   **Pregunta**: Un equipo Scrum ha finalizado un sprint, pero durante la integración final a la rama principal (main) descubren que solo algunos commits específicos de la rama de una funcionalidad deben aplicarse a producción. ¿Cómo podría ayudar git cherry-pick en este caso? Explica los beneficios y posibles complicaciones.

   Cherry-pick permite seleccionar solo los commits necesarios para ser agregados a la rama main, esto es ideal para el escenario propuesto. Entre sus complicaciones está que crea commits duplicados si la rama completa se fusiona después y se puede perder el seguimiento de los cambios que están en producción.

#### **Ejercicios prácticos**

1. **Simulación de un flujo de trabajo Scrum con git rebase y git merge**

   **Contexto:**  
   Tienes una rama `main` y una rama `feature` en la que trabajas. Durante el desarrollo del sprint, se han realizado commits tanto en `main` como en `feature`.  

   Tu objetivo es integrar los cambios de la rama `feature` en `main` manteniendo un historial limpio.

   **Instrucciones:**

   - Crea un repositorio y haz algunos commits en la rama main.
   - Crea una rama feature, agrega nuevos commits, y luego realiza algunos commits adicionales en main.
   - Realiza un rebase de feature sobre main.
   - Finalmente, realiza una fusión fast-forward de feature con main.

   **Comandos:**
   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ mkdir scrum-workflow
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ cd scrum-workflow
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git init
   Inicializado repositorio Git vacío en /home/hoperrs/Escritorio/DS/Actividades/Actividad06/scrum-workflow/.git/
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ echo "Commit inicial en main" > mainfile.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git add mainfile.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git commit -m "Commit inicial en main"
   [main (commit-raíz) a95da7f] Commit inicial en main
   1 file changed, 1 insertion(+)
   create mode 100644 mainfile.md

   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git checkout -b feature
   Cambiado a nueva rama 'feature'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ echo "Nueva característica en feature" > featurefile.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git add featurefile.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git commit -m "Commit en feature"
   [feature 819fb6a] Commit en feature
   1 file changed, 1 insertion(+)
   create mode 100644 featurefile.md

   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git checkout main
   Cambiado a rama 'main'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ echo "Actualización en main" >> mainfile.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git add mainfile.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git commit -m "Actualización en main"
   [main 10dfff6] Actualización en main
   1 file changed, 1 insertion(+)

   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git checkout feature
   Cambiado a rama 'feature'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git rebase main
   Rebase aplicado satisfactoriamente y actualizado refs/heads/feature.

   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git checkout main
   Cambiado a rama 'main'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-workflow$ git merge feature --ff-only
   Actualizando 10dfff6..95bf806
   Fast-forward
   featurefile.md | 1 +
   1 file changed, 1 insertion(+)
   create mode 100644 featurefile.md
   ```

   **Preguntas:**

   - ¿Qué sucede con el historial de commits después del rebase?

      En la rama feature se aplican los commits de la rama main pero con diferente hash.

   - ¿En qué situación aplicarías una fusión fast-forward en un proyecto ágil?

      Cuando previamente hice un rebase en una rama feature y quiero hacer un merge -ff para mantener el historial de commits lineal y limpio.


2. **Cherry-pick para integración selectiva en un pipeline CI/CD**

   **Contexto:**  
   Durante el desarrollo de una funcionalidad, te das cuenta de que solo ciertos cambios deben ser integrados en la rama de producción, ya que el resto aún está en desarrollo. Para evitar fusionar toda la rama, decides hacer cherry-pick de los commits que ya están listos para producción.

   **Instrucciones:**

   - Crea un repositorio con una rama main y una rama feature.
   - Haz varios commits en la rama feature, pero solo selecciona uno o dos commits específicos que consideres listos para producción.
   - Realiza un cherry-pick de esos commits desde feature a main.
   - Verifica que los commits cherry-picked aparezcan en main.

   **Comandos:**
   ```bash
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ mkdir ci-cd-workflow
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ cd ci-cd-workflow
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git init
   Inicializado repositorio Git vacío en /home/hoperrs/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow/.git/
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ echo "Commit inicial en main" > main.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git add main.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git commit -m "Commit inicial en main"
   [main (commit-raíz) c4cdc54] Commit inicial en main
   1 file changed, 1 insertion(+)
   create mode 100644 main.md

   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git checkout -b feature
   Cambiado a nueva rama 'feature'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ echo "Primera característica" > feature1.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git add feature1.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git commit -m "Agregar primera característica"
   [feature 19fa8b8] Agregar primera característica
   1 file changed, 1 insertion(+)
   create mode 100644 feature1.md

   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ echo "Segunda característica" > feature2.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git add feature2.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git commit -m "Agregar segunda característica"
   [feature 94431a6] Agregar segunda característica
   1 file changed, 1 insertion(+)
   create mode 100644 feature2.md

   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git checkout main
   Cambiado a rama 'main'
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git cherry-pick 19fa8b8
   [main c0904c9] Agregar primera característica
   Date: Sun Apr 20 16:27:41 2025 -0500
   1 file changed, 1 insertion(+)
   create mode 100644 feature1.md
   hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/ci-cd-workflow$ git cherry-pick 94431a6
   [main ceceb0b] Agregar segunda característica
   Date: Sun Apr 20 16:28:24 2025 -0500
   1 file changed, 1 insertion(+)
   create mode 100644 feature2.md
   ```

   **Preguntas:**

   - ¿Cómo utilizarías cherry-pick en un pipeline de CI/CD para mover solo ciertos cambios listos a producción?

      Revisaría el historial de commits en la rama feature que me interesa para seleccionar el commit que se aplicará a la rama main con el comando cherry-pick.

   - ¿Qué ventajas ofrece cherry-pick en un flujo de trabajo de DevOps?

      Me permite aplicar commits específicos de la rama feature que requieran pronta implementación para CI/CD, y me permite seguir trabajando en el rama feature sin aplicar todos los cambios en main hasta estar completamente listo.

---

#### **Git, Scrum y Sprints**

#### **Fase 1: Planificación del sprint (sprint planning)**

**Ejercicio 1: Crear ramas de funcionalidades (feature branches)**

En esta fase del sprint, los equipos Scrum deciden qué historias de usuario van a trabajar. Cada historia de usuario puede representarse como una rama de funcionalidad.

**Objetivo:** Crear ramas para cada historia de usuario y asegurar que el trabajo se mantenga aislado.

**Instrucciones:**

1. Crea un repositorio en Git.
2. Crea una rama `main` donde estará el código base.
3. Crea una rama por cada historia de usuario asignada al sprint, partiendo de la rama `main`.

**Comandos:**
```bash
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ mkdir scrum-project
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06$ cd scrum-project
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git init
Inicializado repositorio Git vacío en /home/hoperrs/Escritorio/DS/Actividades/Actividad06/scrum-project/.git/
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ echo "# Proyecto Scrum" > README.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git add README.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git commit -m "Commit inicial en main"
[main (commit-raíz) 346cbca] Commit inicial en main
 1 file changed, 1 insertion(+)
 create mode 100644 README.md

# Crear ramas de historias de usuario
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git checkout -b feature-user-story-1
Cambiado a nueva rama 'feature-user-story-1'
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git checkout -b feature-user-story-2
Cambiado a nueva rama 'feature-user-story-2'
```

**Pregunta:** ¿Por qué es importante trabajar en ramas de funcionalidades separadas durante un sprint?

   Porque permite el aislamiento de cambios, estabilidad y una experimentación segura. También facilita la revisión de codigo e integración continua al permitir las pruebas automatizadas por funcionalidad.


#### **Fase 2: Desarrollo del sprint (sprint execution)**

**Ejercicio 2: Integración continua con git rebase**

A medida que los desarrolladores trabajan en sus respectivas historias de usuario, pueden ocurrir cambios en main. Para mantener un historial lineal y evitar conflictos más adelante, se usa `git rebase` para integrar los últimos cambios de main en las ramas de funcionalidad antes de finalizar el sprint.

**Objetivo:** Mantener el código de la rama de funcionalidad actualizado con los últimos cambios de main durante el sprint.

**Instrucciones:**

1. Haz algunos commits en main.
2. Realiza un rebase de la rama `feature-user-story-1` para actualizar su base con los últimos cambios de main.

**Comandos:**
```bash
# Simula cambios en la rama main
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git checkout main
Cambiado a rama 'main'
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ echo "Actualización en main" > updates.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git add updates.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git commit -m "Actualizar main con nuevas funcionalidades"
[main 9d08cbe] Actualizar main con nuevas funcionalidades
 1 file changed, 1 insertion(+)
 create mode 100644 updates.md

# Rebase de la rama feature-user-story-1 sobre main
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git checkout feature-user-story-1
Cambiado a rama 'feature-user-story-1'
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git rebase main
Rebase aplicado satisfactoriamente y actualizado refs/heads/feature-user-story-1.
```

**Pregunta:** ¿Qué ventajas proporciona el rebase durante el desarrollo de un sprint en términos de integración continua?

Rebase permite una sincronización frecuente al mantener las ramas feature actualizadas con últimos cambios aplicados en main. También reduce los posibles conflictos al actualizarse regularmente y produce tests más confiables con código actualizado.

#### **Fase 3: Revisión del sprint (sprint review)**

**Ejercicio 3: Integración selectiva con git cherry-pick**

En esta fase, es posible que algunas funcionalidades estén listas para ser mostradas a los stakeholders, pero otras aún no están completamente implementadas. Usar `git cherry-pick` puede permitirte seleccionar commits específicos para mostrar las funcionalidades listas, sin hacer merge de ramas incompletas.

**Objetivo:** Mover commits seleccionados de una rama de funcionalidad (`feature-user-story-2`) a `main` sin integrar todos los cambios.

**Instrucciones:**

1. Realiza algunos commits en `feature-user-story-2`.
2. Haz cherry-pick de los commits que estén listos para mostrarse a los stakeholders durante la revisión del sprint.

**Comandos:**
```bash
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git checkout feature-user-story-2
Cambiado a rama 'feature-user-story-2'
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ echo "Funcionalidad lista" > feature2.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git add feature2.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git commit -m "Funcionalidad lista para revisión"
[feature-user-story-2 7112b99] Funcionalidad lista para revisión
 1 file changed, 1 insertion(+)
 create mode 100644 feature2.md

hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ echo "Funcionalidad en progreso" > progress.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git add progress.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git commit -m "Funcionalidad aún en progreso"
[feature-user-story-2 b3d422b] Funcionalidad aún en progreso
 1 file changed, 1 insertion(+)
 create mode 100644 progress.md

# Ahora selecciona solo el commit que esté listo
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git checkout main
Cambiado a rama 'main'
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git cherry-pick 7112b99
[main 896bbde] Funcionalidad lista para revisión
 Date: Sun Apr 20 18:22:39 2025 -0500
 1 file changed, 1 insertion(+)
 create mode 100644 feature2.md
```

**Pregunta:** ¿Cómo ayuda `git cherry-pick` a mostrar avances de forma selectiva en un sprint review?

Cherry-pick permite aplicar un commit específico a la rama main o a otra rama que se use para la revisión y aprobación, nos facilita el mostrar los avances de los commits que ya tienen funcionalidades finalizadas.

#### **Fase 4: Retrospectiva del sprint (sprint retrospective)**

**Ejercicio 4: Revisión de conflictos y resolución**

Durante un sprint, pueden surgir conflictos al intentar integrar diferentes ramas de funcionalidades. Es importante aprender cómo resolver estos conflictos y discutirlos en la retrospectiva.

**Objetivo:** Identificar y resolver conflictos de fusión con `git merge` al intentar integrar varias ramas de funcionalidades al final del sprint.

**Instrucciones:**

1. Realiza cambios en `feature-user-story-1` y `feature-user-story-2` que resulten en conflictos.
2. Intenta hacer merge de ambas ramas con main y resuelve los conflictos.

**Comandos:**
```bash
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git checkout feature-user-story-1
Cambiado a rama 'feature-user-story-1'
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ echo "Cambio en la misma línea" > conflicted-file.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git add conflicted-file.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git commit -m "Cambio en feature 1"
[feature-user-story-1 a29ff44] Cambio en feature 1
 1 file changed, 1 insertion(+)
 create mode 100644 conflicted-file.md

hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git checkout feature-user-story-2
Cambiado a rama 'feature-user-story-2'
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ echo "Cambio diferente en la misma línea" > conflicted-file.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git add conflicted-file.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git commit -m "Cambio en feature 2"
[feature-user-story-2 4576016] Cambio en feature 2
 1 file changed, 1 insertion(+)
 create mode 100644 conflicted-file.md

# Intentar hacer merge en main
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git checkout main
Cambiado a rama 'main'
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git merge feature-user-story-1
Merge made by the 'ort' strategy.
 conflicted-file.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 conflicted-file.md
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git merge feature-user-story-2
Auto-fusionando conflicted-file.md
CONFLICTO (agregar/agregar): Conflicto de fusión en conflicted-file.md
Fusión automática falló; arregle los conflictos y luego realice un commit con el resultado.
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git add conflicted-file.md 
hoperrs@hoperrs:~/Escritorio/DS/Actividades/Actividad06/scrum-project$ git commit
[main c9d3b3a] Merge branch 'feature-user-story-2'
```

**Pregunta**: ¿Cómo manejas los conflictos de fusión al final de un sprint? ¿Cómo puede el equipo mejorar la comunicación para evitar conflictos grandes?

Inspeccionando los archivos que presentan conflicos, y seleccionar los cambios a preservar después de debatir con el equipo. Se recomienda mejorar la comunicación al definir los cambios y funcionalidad únicas que se asignarán a cada colaborar a fin de que 2 o más personas no modifiquen el mismo archivo al mismo tiempo y generen conflictos al querer integrarlos.

#### **Fase 5: Fase de desarrollo, automatización de integración continua (CI) con git rebase**

**Ejercicio 5: Automatización de rebase con hooks de Git**

En un entorno CI, es común automatizar ciertas operaciones de Git para asegurar que el código se mantenga limpio antes de que pase a la siguiente fase del pipeline. Usa los hooks de Git para automatizar el rebase cada vez que se haga un push a una rama de funcionalidad.

**Objetivo:** Implementar un hook que haga automáticamente un rebase de `main` antes de hacer push en una rama de funcionalidad, asegurando que el historial se mantenga limpio.

**Instrucciones:**

1. Configura un hook `pre-push` que haga un rebase automático de la rama `main` sobre la rama de funcionalidad antes de que el push sea exitoso.
2. Prueba el hook haciendo push de algunos cambios en la rama `feature-user-story-1`.

**Comandos:**
```bash
# Dentro de tu proyecto, crea un hook pre-push
$ nano .git/hooks/pre-push

# Agrega el siguiente script para automatizar el rebase
#!/bin/bash
git fetch origin main
git rebase origin/main

# Haz el archivo ejecutable
$ chmod +x .git/hooks/pre-push

# Simula cambios y haz push
$ git checkout feature-user-story-1
$ echo "Cambios importantes" > feature1.md
$ git add feature1.md
$ git commit -m "Cambios importantes en feature 1"
$ git push origin feature-user-story-1
```

**Pregunta**: ¿Qué ventajas y desventajas observas al automatizar el rebase en un entorno de CI/CD?

---

### **Navegando conflictos y versionado en un entorno devOps**

**Objetivo:**  
Gestionar conflictos en Git, realizar fusiones complejas, utilizar herramientas para comparar y resolver conflictos, aplicar buenas prácticas en el manejo del historial de versiones  y usar el versionado semántico en un entorno de integración continua (CI).

**Herramientas:**

- Git  
- Un entorno de desarrollo (Visual Studio Code, terminal, etc.)  
- Un repositorio en GitHub o GitLab (opcional, puede ser local)

**Contexto:**  
En un entorno de desarrollo colaborativo, los conflictos son inevitables cuando varios desarrolladores trabajan en la misma base de código. Resolver estos conflictos es crucial para mantener un flujo de trabajo eficiente en DevOps.

Los conflictos ocurren cuando dos ramas modifican la misma línea de un archivo y luego se intenta fusionarlas. Git no puede decidir qué cambio priorizar, por lo que la resolución manual es necesaria.


#### **Cómo fusionar conflictos en Git:**

1. **Identificar conflictos**: Usa `git status` para ver los archivos en conflicto.
2. **Examinar los archivos**: Busca los marcadores de conflicto (`<<<<<<<`, `=======`, `>>>>>>`) en los archivos.
3. **Resolver los conflictos**: Elige qué cambios conservar (rama actual o fusionada) o mezcla ambos.
4. **Commit de los archivos resueltos**: Después de resolver, añade los archivos al staging y realiza el commit.

#### **Comandos para resolver conflictos**

- `git checkout --ours <file-path>`: Conserva los cambios de tu rama.  
- `git checkout --theirs <file-path>`: Conserva los cambios de la rama fusionada.


#### **Herramientas para gestionar fusiones**

- `git diff`: Compara las diferencias entre dos ramas o commits, ayudando a identificar conflictos:
  ```bash
  $ git diff feature-branch..main
  ```

- `git merge --no-commit --no-ff`: Simula una fusión sin realizar el commit para ver los cambios:
  ```bash
  $ git merge --no-commit --no-ff feature-branch
  $ git diff --cached
  ```
  Si no es lo que esperas, puedes abortar la fusión:
  ```bash
  $ git merge --abort
  ```

- `git mergetool`: Usa herramientas gráficas para resolver conflictos de manera visual. Configura tu herramienta preferida:
  ```bash
  $ git config --global merge.tool vimdiff
  $ git mergetool
  ```

##### **Comandos para organizar tu entorno de trabajo**

- **git reset**: Este comando permite retroceder en el historial de commits. Existen tres tipos:

  1. **Soft Reset**: Mueve el HEAD sin cambiar los archivos:
     ```bash
     $ git reset --soft <commit>
     ```
  2. **Mixed Reset**: Mueve el HEAD y quita archivos del staging, pero mantiene los cambios:
     ```bash
     $ git reset --mixed <commit>
     ```
  3. **Hard Reset**: Elimina todos los cambios no guardados y resetea el directorio de trabajo:
     ```bash
     $ git reset --hard <commit>
     ```

- **git revert**: Deshace cambios sin modificar el historial de commits, creando un nuevo commit:
  ```bash
  $ git revert <commit_hash>
  ```

- **git checkout**: Además de cambiar de ramas, este comando te permite restaurar archivos específicos:
  ```bash
  $ git checkout -- <file_name>
  ```

##### **Herramientas para depurar**

- **git blame**: Muestra qué usuario hizo cambios en una línea específica de un archivo:
  ```bash
  $ git blame file.txt
  ```

- **git bisect**: Realiza una búsqueda binaria para encontrar el commit que introdujo un error:
  ```bash
  $ git bisect start
  $ git bisect bad
  $ git bisect good <commit>
  $ git bisect reset
  ```


##### **git clean y stash**

1. `git clean`: Elimina archivos y directorios no rastreados.
   ```bash
   $ git clean -fd
   ```

2. `git stash`: Guarda cambios sin hacer commit, útil para multitasking.
   ```bash
   $ git stash
   $ git stash apply stash@{0}
   ```

##### **.gitignore**

El archivo `.gitignore` te permite especificar qué archivos y carpetas deben ignorarse durante un `git add`, asegurando que permanezcan exclusivos de tu entorno local.

```bash
# Ignorar todos los archivos de log
.log

# Ignorar archivos de configuración personal
config/personal/
```

##### **Versioning en Git**

Usa versioning semántico para gestionar versiones del software de manera clara:
```bash
$ git tag -a v1.0 -m "Initial stable release"
$ git tag v2.4.4 <commit>
```

---

#### **Ejemplo:**

1. **Inicialización del proyecto y creación de ramas**

   - **Paso 1**: Crea un nuevo proyecto en tu máquina local.
     ```bash
     $ mkdir proyecto-colaborativo
     $ cd proyecto-colaborativo
     ```
   - **Paso 2**: Inicializa Git en tu proyecto.
     ```bash
     $ git init
     ```
   - **Paso 3**: Crea un archivo de texto llamado `archivo_colaborativo.txt` y agrega algún contenido inicial.
     ```bash
     $ echo "Este es el contenido inicial del proyecto" > archivo_colaborativo.txt
     ```
   - **Paso 4**: Agrega el archivo al área de staging y haz el primer commit.
     ```bash
     $ git add .
     $ git commit -m "Commit inicial con contenido base"
     ```
   - **Paso 5**: Crea dos ramas activas: main y feature-branch.
     ```bash
     $ git branch feature-branch  # Crear una nueva rama
     ```
   - **Paso 6**: Haz checkout a la rama feature-branch y realiza un cambio en el archivo `archivo_colaborativo.txt`.
     ```bash
     $ git checkout feature-branch
     $ echo "Este es un cambio en la feature-branch" >> archivo_colaborativo.txt
     $ git add .
     $ git commit -m "Cambios en feature-branch"
     ```
   - **Paso 7**: Regresa a la rama main y realiza otro cambio en la misma línea del archivo `archivo_colaborativo.txt`.
     ```bash
     $ git checkout main
     $ echo "Este es un cambio en la rama main" >> archivo_colaborativo.txt
     $ git add .
     $ git commit -m "Cambios en main"
     ```

2. **Fusión y resolución de conflictos**

   - **Paso 1**: Intenta fusionar feature-branch en main. Se espera que surjan conflictos de fusión.
     ```bash
     $ git merge feature-branch
     ```
   - **Paso 2**: Usa `git status` para identificar los archivos en conflicto. Examina los archivos afectados y resuelve manualmente los conflictos, conservando las líneas de código más relevantes para el proyecto.
     ```bash
     $ git status
     $ git checkout --theirs <archivo>  # Si decides aceptar los cambios de feature-branch
     $ git checkout --ours <archivo>    # Si decides aceptar los cambios de main
     ```
   - **Paso 3**: Una vez resueltos los conflictos, commitea los archivos y termina la fusión
     ```bash
     $ git add .
     $ git commit -m "Conflictos resueltos"
     ```

3. **Simulación de fusiones y uso de git diff**

   - **Paso 1**: Simula una fusión usando `git merge --no-commit --no-ff` para ver cómo se comportarían los cambios antes de realizar el commit.
     ```bash
     $ git merge --no-commit --no-ff feature-branch
     $ git diff --cached  # Ver los cambios en el área de staging
     $ git merge --abort  # Abortar la fusión si no es lo que se esperaba
     ```

4. **Uso de git mergetool**

   - **Paso 1**: Configura git mergetool con una herramienta de fusión visual (puedes usar meld, vimdiff, o Visual Studio Code).
     ```bash
     $ git config --global merge.tool <nombre-herramienta>
     $ git mergetool
     ```
   - **Paso 2**: Usa la herramienta gráfica para resolver un conflicto de fusión.

5. **Uso de git revert y git reset**

   - **Paso 1**: Simula la necesidad de revertir un commit en main debido a un error. Usa `git revert` para crear un commit que deshaga los cambios.
     ```bash
     $ git revert <commit_hash>
     ```
   - **Paso 2**: Realiza una prueba con `git reset --mixed` para entender cómo reestructurar el historial de commits sin perder los cambios no commiteados.
     ```bash
     $ git reset --mixed <commit_hash>
     ```

6. **Versionado semántico y etiquetado**

   - **Paso 1**: Aplica versionado semántico al proyecto utilizando tags para marcar versiones importantes.
     ```bash
     $ git tag -a v1.0.0 -m "Primera versión estable"
     $ git push origin v1.0.0
     ```

7. **Aplicación de git bisect para depuración**

   - **Paso 1**: Usa `git bisect` para identificar el commit que introdujo un error en el código.
     ```bash
     $ git bisect start
     $ git bisect bad   # Indica que la versión actual tiene un error
     $ git bisect good <último_commit_bueno>
     # Continúa marcando como "good" o "bad" hasta encontrar el commit que introdujo el error
     $ git bisect reset  # Salir del modo bisect
     ```

8. **Documentación y reflexión**

   - **Paso 1**: Documenta todos los comandos usados y los resultados obtenidos en cada paso.
   - **Paso 2**: Reflexiona sobre la utilidad de cada comando en un flujo de trabajo de DevOps.


#### **Preguntas**

1. **Ejercicio para git checkout --ours y git checkout --theirs**

   **Contexto**: En un sprint ágil, dos equipos están trabajando en diferentes ramas. Se produce un conflicto de fusión en un archivo de configuración crucial. El equipo A quiere mantener sus cambios mientras el equipo B solo quiere conservar los suyos. El proceso de entrega continua está detenido debido a este conflicto.

   **Pregunta**:  
   ¿Cómo utilizarías los comandos `git checkout --ours` y `git checkout --theirs` para resolver este conflicto de manera rápida y eficiente? Explica cuándo preferirías usar cada uno de estos comandos y cómo impacta en la pipeline de CI/CD. ¿Cómo te asegurarías de que la resolución elegida no comprometa la calidad del código?

2. **Ejercicio para git diff**

   **Contexto**: Durante una revisión de código en un entorno ágil, se observa que un pull request tiene una gran cantidad de cambios, muchos de los cuales no están relacionados con la funcionalidad principal. Estos cambios podrían generar conflictos con otras ramas en la pipeline de CI/CD.

   **Pregunta**:  
   Utilizando el comando `git diff`, ¿cómo compararías los cambios entre ramas para identificar diferencias específicas en archivos críticos? Explica cómo podrías utilizar `git diff feature-branch..main` para detectar posibles conflictos antes de realizar una fusión y cómo esto contribuye a mantener la estabilidad en un entorno ágil con CI/CD.

3. **Ejercicio para git merge --no-commit --no-ff**

   **Contexto**: En un proyecto ágil con CI/CD, tu equipo quiere simular una fusión entre una rama de desarrollo y la rama principal para ver cómo se comporta el código sin comprometerlo inmediatamente en el repositorio. Esto es útil para identificar posibles problemas antes de completar la fusión.

   **Pregunta**:  
   Describe cómo usarías el comando `git merge --no-commit --no-ff` para simular una fusión en tu rama local. ¿Qué ventajas tiene esta práctica en un flujo de trabajo ágil con CI/CD, y cómo ayuda a minimizar errores antes de hacer commits definitivos? ¿Cómo automatizarías este paso dentro de una pipeline CI/CD?

4. **Ejercicio para git mergetool**

   **Contexto**: Tu equipo de desarrollo utiliza herramientas gráficas para resolver conflictos de manera colaborativa. Algunos desarrolladores prefieren herramientas como vimdiff o Visual Studio Code. En medio de un sprint, varios archivos están en conflicto y los desarrolladores prefieren trabajar en un entorno visual para resolverlos.

   **Pregunta**:  
   Explica cómo configurarías y utilizarías `git mergetool` en tu equipo para integrar herramientas gráficas que faciliten la resolución de conflictos. ¿Qué impacto tiene el uso de `git mergetool` en un entorno de trabajo ágil con CI/CD, y cómo aseguras que todos los miembros del equipo mantengan consistencia en las resoluciones?

5. **Ejercicio para git reset**

   **Contexto**: En un proyecto ágil, un desarrollador ha hecho un commit que rompe la pipeline de CI/CD. Se debe revertir el commit, pero se necesita hacerlo de manera que se mantenga el código en el directorio de trabajo sin deshacer los cambios.

   **Pregunta**:  
   Explica las diferencias entre `git reset --soft`, `git reset --mixed` y `git reset --hard`. ¿En qué escenarios dentro de un flujo de trabajo ágil con CI/CD utilizarías cada uno? Describe un caso en el que usarías `git reset --mixed` para corregir un commit sin perder los cambios no commiteados y cómo afecta esto a la pipeline.

6. **Ejercicio para git revert**

   **Contexto**: En un entorno de CI/CD, tu equipo ha desplegado una característica a producción, pero se ha detectado un bug crítico. La rama principal debe revertirse para restaurar la estabilidad, pero no puedes modificar el historial de commits debido a las políticas del equipo.

   **Pregunta**:  
   Explica cómo utilizarías `git revert` para deshacer los cambios sin modificar el historial de commits. ¿Cómo te aseguras de que esta acción no afecte la pipeline de CI/CD y permita una rápida recuperación del sistema? Proporciona un ejemplo detallado de cómo revertirías varios commits consecutivos.

7. **Ejercicio para git stash**

   **Contexto**: En un entorno ágil, tu equipo está trabajando en una corrección de errores urgente mientras tienes cambios no guardados en tu directorio de trabajo que aún no están listos para ser committeados. Sin embargo, necesitas cambiar rápidamente a una rama de hotfix para trabajar en la corrección.

   **Pregunta**:  
   Explica cómo utilizarías `git stash` para guardar temporalmente tus cambios y volver a ellos después de haber terminado el hotfix. ¿Qué impacto tiene el uso de `git stash` en un flujo de trabajo ágil con CI/CD cuando trabajas en múltiples tareas? ¿Cómo podrías automatizar el proceso de *stashing* dentro de una pipeline CI/CD?

8. **Ejercicio para .gitignore**

   **Contexto**: Tu equipo de desarrollo ágil está trabajando en varios entornos locales con configuraciones diferentes (archivos de logs, configuraciones personales). Estos archivos no deberían ser parte del control de versiones para evitar confusiones en la pipeline de CI/CD.

   **Pregunta**:  
   Diseña un archivo `.gitignore` que excluya archivos innecesarios en un entorno ágil de desarrollo. Explica por qué es importante mantener este archivo actualizado en un equipo colaborativo que utiliza CI/CD y cómo afecta la calidad y limpieza del código compartido en el repositorio.

---

#### **Ejercicios adicionales**

##### **Ejercicio 1: Resolución de conflictos en un entorno ágil**

**Contexto:**  
Estás trabajando en un proyecto ágil donde múltiples desarrolladores están enviando cambios a la rama principal cada día. Durante una integración continua, se detectan conflictos de fusión entre las ramas de dos equipos que están trabajando en dos funcionalidades críticas. Ambos equipos han modificado el mismo archivo de configuración del proyecto.

**Pregunta:**  
- ¿Cómo gestionarías la resolución de este conflicto de manera eficiente utilizando Git y manteniendo la entrega continua sin interrupciones? ¿Qué pasos seguirías para minimizar el impacto en la CI/CD y asegurar que el código final sea estable?


##### **Ejercicio 2: Rebase vs. Merge en integraciones ágiles**

**Contexto:**  
En tu equipo de desarrollo ágil, cada sprint incluye la integración de varias ramas de características. Algunos miembros del equipo prefieren realizar merge para mantener el historial completo de commits, mientras que otros prefieren rebase para mantener un historial lineal.

**Pregunta:**  
- ¿Qué ventajas y desventajas presenta cada enfoque (merge vs. rebase) en el contexto de la metodología ágil? ¿Cómo impacta esto en la revisión de código, CI/CD, y en la identificación rápida de errores?


##### **Ejercicio 3: Git Hooks en un flujo de trabajo CI/CD ágil**

**Contexto:**  
Tu equipo está utilizando Git y una pipeline de CI/CD que incluye tests unitarios, integración continua y despliegues automatizados. Sin embargo, algunos desarrolladores accidentalmente comiten código que no pasa los tests locales o no sigue las convenciones de estilo definidas por el equipo.

**Pregunta:**  
- Diseña un conjunto de Git Hooks que ayudaría a mitigar estos problemas, integrando validaciones de estilo y tests automáticos antes de permitir los commits. Explica qué tipo de validaciones implementarías y cómo se relaciona esto con la calidad del código y la entrega continua en un entorno ágil.

##### **Ejercicio 4: Estrategias de branching en metodologías ágiles**

**Contexto:**  
Tu equipo de desarrollo sigue una metodología ágil y está utilizando Git Flow para gestionar el ciclo de vida de las ramas. Sin embargo, a medida que el equipo ha crecido, la gestión de las ramas se ha vuelto más compleja, lo que ha provocado retrasos en la integración y conflictos de fusión frecuentes.

**Pregunta:**  
- Explica cómo adaptarías o modificarías la estrategia de branching para optimizar el flujo de trabajo del equipo en un entorno ágil y con integración continua. Considera cómo podrías integrar feature branches, release branches y hotfix branches de manera que apoyen la entrega continua y minimicen conflictos.


##### **Ejercicio 5: Automatización de reversiones con git en CI/CD**

**Contexto:**  
Durante una integración continua en tu pipeline de CI/CD, se detecta un bug crítico después de haber fusionado varios commits a la rama principal. El equipo necesita revertir los cambios rápidamente para mantener la estabilidad del sistema.

**Pregunta:**  
- ¿Cómo diseñarías un proceso automatizado con Git y CI/CD que permita revertir cambios de manera eficiente y segura? Describe cómo podrías integrar comandos como `git revert` o `git reset` en la pipeline y cuáles serían los pasos para garantizar que los bugs se reviertan sin afectar el desarrollo en curso.

--- 
**Entrega:**  
- Al finalizar, debes hacer push a su repositorio remoto con los cambios realizados y etiquetar el commit final.

**Evaluación:**  
- El dominio de los comandos Git será evaluado, junto con la correcta resolución de conflictos, uso de herramientas de fusión, y comprensión de versionado semántico.
