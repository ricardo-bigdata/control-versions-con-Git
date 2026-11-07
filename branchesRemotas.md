## 1. Como solucionar o estado "HEAD detached"

```bash
git status
HEAD detached at origin/rb/ricardoVeiga/develop
nothing to commit, working tree clean
```

```bash
git branch -a
* (HEAD detached at origin/rb/ricardoVeiga/develop)
  main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/rb/javierConde/develop
  remotes/origin/rb/ricardoVeiga/develop
```


Para saír do estado **HEAD detached** e poder traballar na túa rama de desenvolvemento sen problemas, o ideal é que crees unha **rama local** que estea conectada directamente coa rama remota do servidor.

Mirando o teu `git branch -a`, a túa rama remota en GitLab chámase `origin/rb/ricardoVeiga/develop`. 

### O comando para solucionalo

Para crear a túa rama local (chamada `develop` ou co mesmo nome) e ligala á remota, executa o seguinte comando na túa terminal:

```bash
git checkout -b develop origin/rb/ricardoVeiga/develop
branch 'develop' set up to track 'origin/rb/ricardoVeiga/develop'.
Switched to a new branch 'develop'
```

*(En versións modernas de Git tamén podes usar: `git switch -c develop origin/rb/ricardoVeiga/develop`)*

### Que fai exactamente este comando?
1. **`-b develop`:** Crea unha nova rama local no teu ordenador chamada `develop`.
2. **`origin/rb/ricardoVeiga/develop`:** Dille a Git que descargue a historia desa rama remota e a use como punto de partida.
3. **Establece o rastrexo (*upstream*):** Conecta ambas as dúas ramas. A partir deste momento, cando fagas un `git status`, Git indicarache se vas por diante ou por detrás do servidor, e poderás facer `git push` ou `git pull` sen engadir máis parámetros.

Se fas agora un `git branch -a`, verás o asterisco `*` colocado de forma segura sobre a túa nova rama local `develop`.

```bash
git status
On branch develop
Your branch is up to date with 'origin/rb/ricardoVeiga/develop'.

nothing to commit, working tree clean
```

```bash
git branch -a
* develop
  main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/rb/javierConde/develop
  remotes/origin/rb/ricardoVeiga/develop
```


## 2. Por que existe unha rama remota pero non en local
Isto é algo moi común en Git e débese a un principio fundamental: **Git é un sistema de control de versións distribuído**. O teu ordenador (local) e o servidor (GitLab) teñen bases de datos independentes que non se sincronizan de forma automática.

Aquí tes as razóns de por que ocorre isto e como funciona por dentro:

### 1. Desconexión por deseño
Cando un compañeiro sube unha rama a GitLab (ou ti mesmo a creas directamente desde a interface web), esa rama existe **únicamente no servidor**. 
O teu Git local non sabe nada dela ata que ti llo pides explicitamente.

### 2. A "Zona de Tránsito" (Ramificións Remotas)
Cando executas o comando **`git fetch`**, o teu ordenador conéctase a GitLab e descárgase a información de todo o novo que hai alí. Pero Git non che crea as ramas locais directamente para non ensuciar o teu espazo de traballo. 

No seu lugar, gárdaas como **ramas de rastrexo remoto** (as que viches en vermello ou co prefixo `remotes/origin/...`). 
* `origin/rb/ricardoVeiga/develop` é só un "indicador" ou unha copia de leitura que che di onde está esa rama en GitLab nese preciso instante. Non podes traballar directamente sobre ela, e por iso se che "descolgaba" o HEAD (`HEAD detached`).

### Como se adoitan crear as ramas? Os dous camiños inversos:

* **Camiño A (De Local a Remoto):** Creas unha rama no teu ordenador (`git checkout -b nova-rama`), traballas nela e logo fáselle un `git push origin nova-rama` para subila a GitLab. Aquí a rama nace en local.
* **Camiño B (De Remoto a Local):** Alguén crea a rama en GitLab (ou fáiselle un fork). O teu ordenador despois do `git fetch` descobre que existe en `origin/...`, pero **tes que crearte ti a túa propia copia local** (co comando `git checkout -b` que vimos antes) para poder poñerte a escribir código e crear commits nela.

> Na saída do comando `git branch -a`, na liña `remotes/origin/HEAD -> origin/main` o indicador `->` representa un **punteiro por defecto dentro do propio servidor remoto (GitLab)**. Significa que se alguén clona ese repositorio por primeira vez sen especificar unha rama, Git descargará automaticamente a rama `main` por ser a rama principal predeterminada do proxecto.


## 3. Como ver que rama local apunta a que rama remota?

Para comprobar se a túa rama local `develop` está correctamente ligada (*tracking*) á rama remota `origin/rb/ricardoVeiga/develop`, o comando `git branch -a` non che abonda. Tes que usar o parámetro **`-vv` (moi verboso)**:

```bash
git branch -vv
```

Ó executar este comando, verás unha saída semellante a esta:

```bash
* develop 3e35b6a [origin/rb/ricardoVeiga/develop] Engadida dúbida sobre parámetro -u en git push
  main    6505497 [origin/main] Extensiones recomendadas y nuevas notas FIXME
```

### Que significa esa información?
* **`* develop`**: indica a rama local na que estás situado.
* **`3e35b6a`**: é o identificador do último commit local nesa rama.
* **`[origin/rb/ricardoVeiga/develop]`**: indica explicitamente que a túa rama local está conectada e rastreando esa rama remota concreta. 

Tamén o podes comprobar de forma moi sinxela executando simplemente **`git status`** mentres estás na rama `develop`. O propio Git indicarache na primeira liña se a túa rama está ao día, por diante ou por detrás da de remoto.

```bash
git status
On branch develop
Your branch is up to date with 'origin/rb/ricardoVeiga/develop'.

nothing to commit, working tree clean
```



En resumo: as ramas remotas son "fotos" do que hai no servidor, e as ramas locais son os teus espazos de traballo interactivos no teu ordenador.
