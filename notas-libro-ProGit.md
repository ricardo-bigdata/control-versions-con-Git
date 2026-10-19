Notas do primeiro tema do libro *Pro Git* (Inicio - Sobre o Control de Versións):

### 1. Evolución dos Sistemas de Control de Versións (VCS)
Un VCS rexistra os cambios nos ficheiros ao longo do tempo para poder recuperar versións anteriores. Hai tres tipos:
*   **Locais:** Gardábanse copias noutros directorios ou bases de datos sinxelas (como RCS). Son propensos a erros.
*   **Centralizados (CVCS):** Un único servidor contén todos os ficheiros e os clientes descargan as versións (como CVS ou Subversion). *Vantaxe:* todos saben que fai o resto. *Desvantaxe:* o servidor é un **punto único de fallo**.
*   **Distribuídos (DVCS):** Cada cliente ten unha **copia completa** do repositorio (como Git). Se o servidor falla, calquera cliente pode restauralo.

### 2. Breve historia de Git
Git naceu en 2005, creado por Linus Torvalds (creador de Linux) despois de que a comunidade de Linux deixase de usar o sistema propietario BitKeeper. Os seus obxectivos eran: **velocidade, deseño sinxelo, soporte para miles de ramas (desenvolvemento non lineal), completamente distribuído** e capaz de xestionar grandes proxectos.

### 3. Fundamentos de Git (Como funciona)
*   **Instantáneas (Snapshots), non diferenzas:** A diferenza doutros sistemas que gardan os cambios (deltas) ficheiro por ficheiro, Git fai unha “foto” do estado de todos os ficheiros en cada confirmación. Se un ficheiro non cambiou, fai unha ligazón ao anterior para aforrar espazo.
*   **Operacións locais:** Case todas as accións (ver historial, facer commits) fanse en local, de xeito instantáneo e sen necesidade de conexión a internet.
*   **Integridade (SHA-1):** Todo se verifica mediante unha suma de comprobación (hash SHA-1). É imposible cambiar un ficheiro sen que Git o detecte.
*   **Só engade información:** É moi difícil facer que Git borre datos de xeito accidental unha vez confirmados.

### 4. Os Tres Estados (Moi importante)
Os ficheiros en Git poden estar en tres estados:
1.  **Confirmado (Committed):** Os datos están gardados de xeito seguro no directorio de Git (base de datos local).
2.  **Modificado (Modified):** Cambiaches o ficheiro pero non o confirmaches.
3.  **Preparado (Staged):** Marcaches o ficheiro modificado para que forme parte da próxima confirmación.

Isto corresponde a tres áreas do proxecto: **Directorio de traballo** (onde editas), **Área de preparación** (staging area, onde pos o que queres gardar) e **Directorio de Git** (onde se garda o historial).

**Fluxo de traballo básico:** Modificar ficheiros \(\rightarrow\) Preparalos (Stage) \(\rightarrow\) Confirmalos (Commit).

### 5. Instalación e Configuración
*   **Liña de comandos:** É a única maneira de executar *todos* os comandos de Git.
*   **Configuración inicial:** Cómpre facelo unha soa vez.
    *   Identidade: `git config --global user.name "Nome"` e `git config --global user.email "correo@exemplo.com"`.
    *   Editor por defecto: `git config --global core.editor emacs` (ou o que prefiras).
*   **Axuda:** Podes consultar o manual de calquera comando con `git help <comando>` ou `git <comando> --help`.
