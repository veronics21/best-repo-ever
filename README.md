# CURSO BÁSICO GIT

Pasos básicos(según yo.. y lo que he entendidio de la documentación) a seguir para git salesforce.

1. [Teoría Git](#id1)
    - [Clonar un repo ](#id2)
    - [El flujo de los cambios: ( local a origin ) ](#id3)
    - [Sincronizar repo y traer cambios desde el origin ( origin a local ) ](#id4)
    - [Ramas, merge, pull request y conflictos](#id5)
2. [Pasos según documentación](#id6)

</br>
</br>
</br>

<div id='id1' />

## Teoría Git 
----

**Checkear / instalar git.**

Abrimos un terminal y comprobamos

```git --version```


En ubuntu viene preinstalado pero en Windows y posiblemente MacOs habrá que instalarlo [Guia de instalación](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git);
 
</br>
</br>
</br>
<div id='id2' />

## Clonar un repo 
----
Esto nos creará una carpeta con el nombre del directorio (como haría un mkdir) pero con los ficheros actualizados de la rama principal del repositorio (main).

Esta carpeta ya tendrá su sincronización con git y podremos empezar a trabajar con git sobre ella directamente.

[Video Clonar Repo](ClonarComandVSC.mp4)

**Mediante Comando.**

Cuando quieres crearte una copia de un repositorio en Github ya existente, podemos hacerlo con el siguiente comando:

`git clone URL_AL_ORIGIN`

**Mediante VSC.**

Siguendo los pasos que te da el VSC.


[Configurar VSC bitbucket y jira](https://support.atlassian.com/bitbucket-cloud/docs/get-started-with-vs-code/);


</br>
</br>
</br>
<div id='id3' />

## El flujo de los cambios: ( local a origin )
--------

Desde que hacemos cambios y guardamos un fichero hasta que este se encuentra en Github, tiene que pasar por las siguientes fases:

0. **Working area**: Aquí se añaden automáticamente todos los ficheros que git detecta que han sufrido cambios

1. **Staging area**: Aquí deberemos añadir aquellos ficheros que vayamos a subir con el próximo commit

2. **Commited**: Una vez tengamos listos nuestros cambios, debemos confirmarlos (commit) para que queden reflejados como un hito dentro del histórico de cambios (un punto en la rama).Podemos dividir nuestros cambios en diferentes commits tanto como queramos, pero hay que tener en cuenta que hasta aquí **SOLO ESTAMOS USANDO GIT EN LOCAL**. Si queremos que otros participantes del proyecto puedan bajarse nuestros cambios, **debemos subir nuestros commits**

3. **"Pusheado"**: Una vez hayamos "empujado" nuestros commits hacia el origin, al checkear con `git status` nos dirá "your branch is up to date with origin".Con esto sabremos que efectivamente nuestros cambios están ya en Github

![El flujo de los cambios de local a origin](/resumenEstados.png)

</br>
</br>


### **1. Añadir un fichero al Staging Area** <div id='add' />
Una vez hayamos hecho cambios en un fichero lo tendremos en el **Working area**. Para pasarlo al **Staging area**:

- Ejemplo: Añade "mifichero.txt" a starting area.  `git add mifichero.txt`

Si quisieramos pasar todo lo que tenemos en el working area al staging, tenemos un atajo:

- Añadir todos los cambios: `git add .`

</br>
</br>

### **2. Confirmar (commitear) los cambios**
Con esto confirmarmos todos los cambios que tenemos en el **Staging area** para crear un "commit"
 que formará ya parte del histórico de git en nuestro local.
Requiere un mensaje que deberá describir que tipo de cambios estamos haciendo:

- Commitear: `git commit -m "Mi mensaje de cambios"`

Este comando no funcionará si no tenemos al menos un fichero en el Staging area

</br>
</br>

### **3. Empujar (pushear) los cambios hacia el origin (Github)** <div id='push' />
Podemos realizar tantos commits seguidos como queramos, pero no estarán subidos hasta que los "pusheemos" hacia el origin:

- Empujar cambios: `git push`

Esto hará que nuestra rama `main` local (que estaba más "adelantada", es decir, tenía más commits)
 se sincronice con nuestra rama `origin/main` (su equivalente en Github)


### **4. Ver todos los commits**
Podemos usar el comando git log con algunos parámetros para tener una visualización simplificada:
`git log --graph --oneline --decorate --all`

La visualización por terminal de las ramas suele ser compleja, pueden usarse herramientas de UI de git como la extensión de VSCode `Git Graph`
o la aplicaciónes como [`GitKraken`](https://www.gitkraken.com/), [`Sourcetree`](https://www.sourcetreeapp.com/).

#### **Revert**
Esto nos permite deshacer los cambios de un commit. Revert hará un nuevo commit eliminando los cambios que hayamos hecho (por ejemplo, si en el commit introdujimos una línea de código, el revert lo que hará será borrarla).

- `git revert HEAD` hará un revert del último commit.
- `git revert HEAD~3` hará un revert con los cambios los tres últimos commits.

> Hay otras opciones, como "git reset --hard" que nos permitrá restablecer la rama en un punto concreto o "git drop" que nos permitirá borrar commits
> Estas opciones destruyen histórico y deben ser manejadas con cuidado cuando 

</br>
</br>
</br>
<div id='id4' />


##  Sincronizar repo y traer cambios desde el origin ( origin a local )
---
¿Y si otro desarrollador ha subido cambios? Nosotros no los veremos hasta que ejecutemos algunos comandos:

![fetch-vs-pull-vs-merge](/fetch-vs-pull-vs-merge.png)

### **Fetch** <div id='fetch' />
`git fetch` nos permitirá bajarnos los cambios para que `git status` nos de información actualizada. 
Ojo!, fetch no nos hace disponer inmediatamente de los cambios que hay en la rama, pues disponer de ellos se realiza en una operación aparte

Si tras el fetch se han realizado cambios en la rama por parte de otro desarrollador, veremos este mensaje al ejecutar `git status`:
> Your branch is behind 'origin/main'

Esto quiere decir que en `origin/main` hay commits que no tienes en tu rama `main` de local

### **Pull** <div id='pull' />
`git pull` nos permite traer a local esos commits adicionales que tenemos en el origin.
Una vez lo ejecutemos podremos ver que nuestros ficheros han cambiado con lo más actual que hay en el origin

</br>
</br>
</br>
<div id='id5' />

##  Ramas, merge, pull request y conflictos
---

Todo lo que hemos hecho hasta ahora sería utilizando la rama `main` y su rama en el origin,
pero todas las nuevas funcionalidades y parches de soporte deben desarrollarse en las ramas **ISO_features**.

>Git nos permite abrir `branches` o ramas, es decir, líneas de commits alternativas a la rama principal.  Estas ramas nos permiten crear commits sin afectar a la rama principal.


![](https://wac-cdn.atlassian.com/dam/jcr:7afd8460-b7bf-4c42-b997-4f5cf24f21e8/01%20Branch-2%20kopiera.png?cdnVersion=309)

### **Cambiar de rama** <div id='checkout' />
``git checkout`` - Cambiar ramas o restaurar archivos de árbol de trabajo
</br>
</br>

### **Crear una rama** <div id='checkoutB' />
Podemos usar el comando `git checkout -b my-branch` para crear una nueva rama `my-branch` en nuestro repo local.
Esta rama partirá desde donde la hayamos creado `main` en este caso. Es importante por tanto si queremos estar actualizados
con los últimos cambios que existan en la rama `main` que primero hagamos un fetch y un pull de esa rama antes de crear la nuestra.
La opción `-b` comprueba la rama, se crea si no existe o, si existe error. 

> Una manera de cambiar el punto de partida de una rama a posteriori es con el comando [git rebase](https://www.atlassian.com/es/git/tutorials/rewriting-history/git-rebase)

</br>
</br>

### **Ver todas las ramas que hay en local y el origin**
Podemos usar para esto `git branch -a`. Si queremos información lo más actualizada posible del origin, 
es conveniente primero ejecutar un fetch para tener los últimos cambios que haya sobre el origin.

</br>
</br>

### **Cambiar nuestra rama de trabajo**
Sólo podemos estar trabajando en una única rama al mismo tiempo en nuestro local.
Con el comando `git checkout  main` podremos movernos de nuevo a `main` o a cualquier otra rama que queramos.
Hay que tener en cuenta que no podremos movernos si tenemos cambios en el working/staging area, así que debemos primero
descartarlos o commitearlos antes de tratar de movernos

> Una opción para guardar los cambios temporalmente es los stash. Los stash son commit temporales para guardar cambios en ficheros temporalmente
>  [git stash](https://www.atlassian.com/es/git/tutorials/saving-changes/git-stash).

</br>
</br>

### **Integrar cambios de una rama en otra**
El objetivo último de todas las ramas que no sean la principal es terminar siendo integradas en esta, a esto se le llama **merge**

Esto llevará todos los cambios que hayamos hechos en los commits de la rama alternativa (a un nuevo commit en la rama de destino ). 

![](https://wac-cdn.atlassian.com/dam/jcr:c6db91c1-1343-4d45-8c93-bdba910b9506/02%20Branch-1%20kopiera.png?cdnVersion=309)

Hay varias formas de provocar un merge. La más sencilla y utilizando comandos ya vistos es con `git pull`:

0. Cerciorarnos de que la rama que queremos integrar la tenemos actualizada (fetch, pull, status)

1. Situarnos con `git checkout` en la rama de destino del merge, por ejemplo: `main`;
2. Realizar un pull, pero indicando la rama que queremos integrar: `git pull my-branch`.
3. `git push` para que el merge quede subido al origin

Esto habrá traído los cambios de `my-branch` en `main`.

</br>
</br>


### **Merge contra ramas protegidas: Pull request**
En los proyectos es habitual que la rama principal `main` o `master` esté **protegida**.
Esto quiere decir que no podrán mergearse otras ramas sobre la principal por el método convencional 
porque requiere un proceso de validación previo manual 


De esta forma, el método de merge "estándar" queda normalmente relegado a traer cambios desde la rama principal
`main` a nuestra rama de trabajo cuando `main` ha sufrido alguna actualización y queremos tener los últimos cambios.

Para solicitar estos cambios se suele utilizar el método de `Pull request` o `Merge request`. Esto se suele realizar
desde la interfaz del servicio de repos que usemos (Github, Gitlab, Bitbucket) y puede variar, pero en general suele 
ser el siguiente:

1. Pulsar el botón "Pull request" en la web del respositorio
2. Indicar qué rama queremos mergear en qué otra rama (A -> B)
3. Revisar cómo el código cambia en la rama principal e introducir comentarios
4. Que la persona con permisos le de al botón "Merge" para mergearlo

</br>
</br>

### **Conflictos**
Puede darse el caso de que desde que abrimos en nuestra rama hayamos hecho cambios sobre líneas de código 
que hayan sufrido cambios por parte de otro desarrollador. 

Lo más habitual es que esta situación se dé de la siguiente forma:

1. Un compañero abre una rama desde `main` y nosotros abrimos otra.
2. Tanto como el compañero como nosotros efectuamos cambios sobre las mismas líneas de código
3. El compañero abre una Pull Request y se mergea mientras nosotros seguimos trabajando en cambios
4. Al actualizar nuestra rama (hacer un merge de main -> nuestra rama) git nos indica que hay líneas con conflictos.

Git nos abrirá el editor de texto que tengamos por defecto (normalmente 'Vim') y nos envolverá las líneas de código
con unos guiones, tanto la nuestra como la de nuestro compañero. Podemos editar el código desde aquí para dejarlo en el estado
que queramos

En estos casos lo mejor es hablar con el compañero y tratar de resolverlos entre los dos, pues la resolución debe hacerse manualmente.


</br>
</br>
</br>
<div id='id6' />

##  Pasos según documentación
---
### **Mover a la rama**
COMANDO: 
Comando para  [mueverte de rama ](#checkout)  a "origin/release" : 
`$ git checkout origin/release`

VSC:
[mueve a la rama ](/CambioRamaVSC.jpg)

### **Actualizar Repositorio local**
COMANDO:
[Bajarnos los cambios](#fetch)  al repositorio local(no al entorno de desarrollo): `$ git fetch origen`
[Bajarnos los cambios](#pull)  al entorno de desarrollo (fetch + merge): 
`$ git pull`

VSC:
[Actualizar](/pushVSC.png)

### **CREAR RAMA**
NOMENGRATURA:
Se deben de crear en 
COMANDO:
 [Crea la rama](#checkoutB) "NOMBRE_RAMA" . 
`$ git checkout -b ISO_feature_pr/sp_myfeature`

 [Sube la rama al repositorio ](#push) "ISO_feature_pr/sp_myfeature" . 
Establece la rama "ISO_feature_pr/sp_myfeature" en el repositorio remoto como la rama de seguimiento ascendente para la rama local actual. Esto significa que, en el futuro, puedes usar simplemente git push o git pull sin especificar la rama remota, ya que se utilizará automáticamente la rama de seguimiento ascendente que acabas de establecer.
`$ git push --set-upstream origin ISO_feature_pr/sp_myfeature`

</br>

### **Desarrollo**
Desarrollar de forma normal, en DevSupport:
Desarrolle la nueva funcionalidad utilizando Visual Studio Code (o su IDE preferido) en conexión con el entorno **DEV_SP/PR**.

Obtener la última versión de Salesforce  ``SFDX: Retrieve Source From Org``, modificar y 
despliégue en la organización ``SFDX: Deploy Source to Org``.

</br>

### **Subir los cambios a Bitbucket**
Es necesario subir **sólo** *los cambios relacionados con este nuevo desarrollo* y **no el resto** de posibles cambios que se desplieguen en el entorno DEV_PR/SP.

1. Asegúrese de obtener la rama correcta mediante el comando branch: ``$ git branch``

>Si se ejecuta sin argumentos adicionales, ``git branch`` simplemente listará todas las ramas locales en tu repositorio y resaltará la rama actual 

2. Añadir todos los cambios de working area a  [starting area (local)](#add) `` $ git add .``.