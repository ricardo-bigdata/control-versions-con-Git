# Sincronizar meu fork con nova branch do repositorio orixinal

## 1. Consulta-la lista de repositorios remotos configurados

Antes de nada necesitamos saber se o noso git local xa ten rexistrado o repositorio pai (orixinal); para elo debemos consulta-la lista de **remotos** configurados no noso proxecto:

```bash
git remote -v

origin	git@gitlab.com:ricardo-bigdata/edu-git-platega-202610-examen.git (fetch)
origin	git@gitlab.com:ricardo-bigdata/edu-git-platega-202610-examen.git (push)
```

Se o noso Git local **xa sabe** onde está o repositorio pai, deberiamos ver na saída catro liñas en total: dúas para `origin` (o noso fork) e outras dúas para `upstream` (o repositorio pai). 

> ***upstream*** pode traducirse por "augas arriba" ou "corrente arriba"; no contexto de Git, emprégase para referirnos ó **repositorio orixinal** ou repositorio fonte do cal parte un proxecto.

O resultado debería ser moi semellante a este:

```bash
origin	git@gitlab.com:ricardo-bigdata/edu-git-platega-202610-examen.git (fetch)
origin	git@gitlab.com:ricardo-bigdata/edu-git-platega-202610-examen.git (push)
upstream	git@gitlab.com:edu-git-platega-2026oct/edu-git-platega-202610-examen.git (fetch)
upstream	git@gitlab.com:edu-git-platega-2026oct/edu-git-platega-202610-examen.git (push)

```

O comando `git remote -v` amosa as **conexións remotas configuradas** no teu repositorio local xunto cos seus enderezos URL completos. Esta saída indica que o este proxecto está conectado a dous repositorios en GitLab, como é típico de proxectos nos que o fluxo de traballo parten dun *fork* (bifurcación):

- Nomes dos remotos (***origin*** e ***upstream***): son os alias curtos que usa Git para referirse ós servidores externos sen ter que escribi-lo enderezo completo cada vez.
  - ***origin*** é simplemente o nome que Git pon por defecto ó clonar un repositorio. 
- Nomes de pólas (***main***, ***master***): son etiquetas que apuntan a un *commit*. Antes de 2020, Git creaba por defecto unha póla chamada ***master***, pero actualmente, plataformas como ***GitHub*** e ***GitLab*** usan ***main*** por defecto, pero pódese renomear a *principal*, *producion* ou calquera outro nome válido.

> Tanto o nome dos remotos (*origin*, *upstream*) coma o nome da póla principal (*main*, *master*) son simples convencións que se poden cambiar en calquera momento.

### Que pasa se non aparece "upstream"?
Se ó executa-lo comando só vemo-las liñas de `origin`, significa que o noso Git local **aínda non coñece o repositorio pai**. Nese caso, teriamos que executa-lo comando para engadilo:

```bash
git remote add upstream git@gitlab.com:edu-git-platega-2026oct/edu-git-platega-202610-examen.git
```

> En caso necesario pódese borrar con `git remote remove` calquera remoto, como p.e. este mesmo `upstream`; en realidade só elimina a "ligazón" ou o acceso directo que o noso Git local ten cara a ese servidor, non borra ningún ficheiro do noso ordenador nin afecta ó noso código.
>
```bash
git remote remove upstream
```

## 2. Sincronizar meu fork coa nova póla "tests/1" creada no repositorio orixinal.

Para sincroniza-lo meu fork coa nova póla `tests/1` creada no repositorio orixinal, hai que facer que o meu Git local coñeza ese repositorio orixinal, descarga-los seus cambios e despois subilos ó meu propio fork en GitLab. 


### 1. Engadi-lo repositorio orixinal como remoto (`upstream`)

Se aínda non o fixemos, debemos indicarlle ó noso Git local onde está o repositorio "pai" ou orixinal. O nome estándar para este remoto é `upstream`:

```bash
git remote add upstream git@gitlab.com:edu-git-platega-2026oct/edu-git-platega-202610-examen.git
```

### 2. Descarga-las novidades do repositorio orixinal

Executamos un `fetch` apuntando a `upstream` para que o noso ordenador se descargue a información de tódalas pólas novas (incluída `tests/1`):

```bash
git fetch upstream
```
**Ó facer isto, o noso Git local creará alomenos unha póla de rastrexo remoto chamada `upstream/tests/1`.**

```bash
git branch -a
* develop
  main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/rb/javierConde/develop
  remotes/origin/rb/ricardoVeiga/develop
  remotes/upstream/main
  remotes/upstream/rb/javierConde/develop
  remotes/upstream/tests/1
```

> Cando executamo-lo comando `git fetch upstream`, estamos a dicirlle a Git: *"Conéctate ó repositorio pai e descárgate **toda** a información sobre o estado actual das súas pólas"*.
>
> Git non descarga só unha póla específica, senón que actualiza o mapa completo dese servidor no noso ordenador. Por iso agora vemos novas pólas co prefixo `remotes/upstream/`:
> * `remotes/upstream/main`: a póla principal do repositorio pai.
> * `remotes/upstream/rb/javierConde/develop`: a póla de desenvolvemento do noso compañeiro no repositorio pai.
> * **`remotes/upstream/tests/1`**: a nova póla que queriamos sincronizar.

Estas pólas que vemos con `remotes/upstream/...` son de **só lectura** (son só "fotos" do servidor). Para poder traballar na de tests e subila ó teu propio fork, hai que completa dous últimos pasos.

### 3. Trae-la póla ó noso contorno local

Agora situámonos nesa nova póla que acabamos de descubrir. O seguinte comando crea unha póla local `tests/1` idéntica á do repositorio orixinal:

```bash
git checkout -b tests/1 upstream/tests/1

tests/1
branch 'tests/1' set up to track 'upstream/tests/1'.
Switched to a new branch 'tests/1'
```

```bash
git branch -v
  develop 3e35b6a Engadida dúbida sobre parámetro -u en git push
  main    6505497 Extensiones recomendadas y nuevas notas FIXME
* tests/1 f29f44a Primer tests
```

```bash
git status
On branch tests/1
Your branch is up to date with 'upstream/tests/1'.

nothing to commit, working tree clean
```

```bash
git branch -a
  develop
  main
* tests/1
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/rb/javierConde/develop
  remotes/origin/rb/ricardoVeiga/develop
  remotes/upstream/main
  remotes/upstream/rb/javierConde/develop
  remotes/upstream/tests/1
```

### 4. Subi-la nova póla ó noso fork (`origin`)

Por último, para que esta póla apareza tamén na interface web do noso fork en GitLab, debemos enviala ó noso remoto por defecto (`origin`):

```bash
git push -u origin tests/1

Enumerating objects: 7, feito.
Counting objects: 100% (7/7), feito.
Delta compression using up to 12 threads
Compressing objects: 100% (4/4), feito.
Writing objects: 100% (5/5), 1.77 KiB | 908.00 KiB/s, feito.
Total 5 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
remote: 
remote: To create a merge request for tests/1, visit:
remote:   https://gitlab.com/ricardo-bigdata/edu-git-platega-202610-examen/-/merge_requests/new?merge_request%5Bsource_branch%5D=tests%2F1
remote: 
To gitlab.com:ricardo-bigdata/edu-git-platega-202610-examen.git
 * [new branch]      tests/1 -> tests/1
branch 'tests/1' set up to track 'origin/tests/1'.
```

A partir deste momento, a póla `tests/1` xa formará parte do noso fork e estará perfectamente visible en GitLab.

####  Por que usar `git push -u origin tests/1` a primeira vez, en vez de `git push`?

Neste momento un simple `git push` daría un erro ou non faría nada**, dependendo de como teñamos configurado Git por defecto.

A razón é que esa póla local `tests/1` **acaba de nacer no noso ordenador** (creámola a partir de `upstream`). O noso servidor propio (`origin`, o noso fork de GitLab) aínda non sabe que esa póla existe nin ten ningunha liña á que conectala.

O comando completo con `-u origin tests/1` dálle a Git 3 instrucións fundamentais:

1. **`origin`**: indica explicitamente a que servidor queres enviar o código (ó noso fork, non ó repositorio pai).
2. **`tests/1`**: indica o nome que vai te-la póla en GitLab.
3. **`-u` (ou `--set-upstream`)**: é o parámetro fundamental. **Crea o enlace permanente** entre a nosa póla local `tests/1` e a nova póla remota `origin/tests/1`.

Poderemos usar simplemente `git push`**a partir do segundo push**; unha vez que executamo-lo comando co `-u` por primeira vez, o enlace xa queda gardado na configuración de Git. 

Nas seguintes ocasións, cando fagamos máis commits nesa póla, abondará con escribir:
```bash
git push
```

Git lembrará automaticamente que a póla `tests/1` do noso ordenador ten que envia-los cambios a `origin/tests/1`.

