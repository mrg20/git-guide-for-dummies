# Pautas a seguir para git

En este documento encontrarás comandos de git, junto con su udo y explicación. Los comandos están agrupados en tres niveles, ya que no todos son iguales de entender o igual de siemples o importantes. Con los dos primeros niveles, tendremos suficiente para poder trabajar con el repositorio.

## Índice
### Parte 1 (Local)
* [Estructura de git](#estructura-de-git)
* [Git add](#git-add)
* [Git commit](#git-commit)
* [Git status](#git-status)
* [Git branch](#git-branch)
* [Git checkout](#git-checkout)
* [Git merge](#git-merge)

### Parte 2 (Origin part 1)
* [Git clone](#git-clone)
* [Origin](#origin)
* [Git pull origin branca](#git-pull-origin-branca)
* [Git push origin branca](#git-push-origin-branca)

### Parte 3 (Origin part 2)
* [Git fetch](#git-fetch)
* [Git pull](#git-pull)
* [Git revert](#git-revert)

## Parte 1
En este nivel, no se entra en detalle en ninguno de los conceptos epxlicados, solo se da una pequeña explicación por encima, par saber qué hacen.

### Estructura de git
Git es, en esencia, un árbol. Este árbol está formado por dos conceptos generales que se deben tener claros: ramas y commits.

**Commit:**
Un commit es una versión de nuestro programa, que hemos guardado.

*Ejemplo*
```
|____ i18n  <= Un commit
|      |___ fichero 'olla.js' modificado
|      |___ fichero 'index.html' eliminado
|____ a32c  <= Otro commit
       |___ fichero 'router.ts' creado
```

**Rama:**
Una rama es una cadena de commits. En el caso anterior era la rama master.

*Ejemplo*
```
master
|____ i18n  <= Un commit
|      |___ fichero 'olla.js' modificado
|      |___ fichero 'index.html' eliminado
|____ a32c  <= Otro commit
       |___ fichero 'router.ts' creado
```

**Nosotros:**
Nosotros siempre estamos en alguna rama. Por defecto siempre miramos el último commit que hemos hecho con las cosas nuevas que vamos vamos escribiendo (también podemos navegar por los commits de una rama, de momento no lo miraremos porque solo complica las cosas y casi nunca hace falta hacerlo).

*Ejemplo*
```
        i18n              a32c
---------|-----------------|-------------- master
                            \
                             |------------ development <=Nosotros
                            c43k
```

En este último ejemplo vemos una nueva subrama, que se llama "development". Nosotros estamos apuntando allí y podemos volver a la rama master si queremos con un simple comando (más tarde lo veremos).

Esta nueva subrama, en el momento que se ha creado (sí, hay un comando para hacerlo), pasa a ser independiente de la rama master y los commits que se hagan en esta nueva rama no los tendrá la otra. Lo podemos ver como si caminésemos por un camino que se llama master, y de golpe nos encontramos una biifurcación. Master continua hacia la izquierda y a la derecha tenemos un camino que se llama development. Todo lo que pase en development, no pasará en máster y todo lo que pase en master (a partir de la bifurcación), no pasará en development.


### Git add

Cuando modificamos un fichero, git sabe que se ha modificado, pero no sabe si hay que guardar est anueva modificación en el siguiente commit. Por tanto, utilizando el comando git add nombreDelFichero le decimos a git que en el próximo commit, las acciones realizadas sobre ese fichero, se guardarán.

*Ejemplo*
```
Fichero modificar.js se le modifica una línia y queremos guardarlo en el siguiente commit.

> git add modificar.js
```

### Git commit

Cuando ya le hemos dicho a git todos los ficheros que queremos que se guarden en el siguiente commit, es hora de hacerlo. Solo hace falta hacer git commit -m "Frase explicando los cambios".

*Ejemplo*
```
> git commit -m "Realizada una modificacio en el tratamiento de tablas sobre las granjas modificadas"
```

Es MUY importante que los mensajes de commit sean descriptivos, indicando qué contiende ese commit (así si alguna vez queremos ver una modificación, sabremos a dónde ir solo por este mensaje).

### Git status

Esta es probablemente una de los mejores comandos de git. Cuando no recordamos qué ficheros hemos modificado o sobre qué ficheros hemos hecho add, este comando indicará toda la información necesaria.

*Ejemplo*
```
>git status

  On branch redactant_git
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

          modified:   instruccionesGit.md

  no changes added to commit (use "git add" and/or "git commit -a")
```

Como vemos, el fichero instruccionesGit.md se ha modificado, pero no se ha hecho git add aún.

### Git branch

Antes hemos visto que de la rama master, salía la subrama development (si no lo has leído, hazlo). Esta rama se ha creado cuando estábamos dentro de master, haciendo git branch development.

*Exemple*
```
        i18n              a32c
---------|-----------------|-------------- master  <=Nosotros
                            \ <=  >git branch development
                             \ development
```

### Git checkout

Cuando tenemos varias ramas, como por ejemplo master, development, funcionalidad1, funcionalidad2, etc.... Tenemos que poder movernos entre estas ramas de alguna forma. Para ello utilizaremos git checkout [nombre de la rama].

*Ejemplo*
```
        i18n              a32c
---------|-----------------|-------------- master <=Nosotros
                            \
                             |------------ development
                            c43k

> git checkout development

        i18n              a32c
---------|-----------------|-------------- master
                            \
                             |------------ development <=Nosotros
                            c43k

```

### Git merge

Este es el comando más dificil del nivel 1. Normalmente, en el proyecto se hará solo merge una vez cada vez que se acaba cada funcionalidad, hacia development. Siempre hay una persona encargada de hacer merge de todas las ramas a development, por tanto, está prohibido utlizar este comando hacia desarrollo sin permiso. 

Cuando nosotros tenemos dos ramas diferentes y con conflictos entre ellas, quizás nos interesa juntar código de una rama con el código de otra. Esto lo haremos con el comando git merge [nombre de la rama que queremos coger].

*Ejemplo*
```
a32c  i23s 3fs1
|-----|----|----- master <=Nosotros
\
 |------|---|---- development
u42a   n42m h21k
```

Nos interesa hacer que master gane las funcionalidades implementadas en development, por tanto haremos git merge development. ATENCIÓN: si hacemos git merge master desde la rama development, será development quien ganará las funcionalidades de master y no al revés.

```
>git merge development
a32c  i23s 3fs1
|-----|----|------|- master <=Nosotros
\                / >git merge development
 |------|---|---|---development
u42a   n42m h21k
```

Cuando utilizamos este comando,  pueden pasar dos cosas. La primera es que no pase nada y que master tenga las funcionalidades que queríamos. Esta es la mejor posibilidad. El problema, es si los commit entre las ramas contienen modificaciónes sobre los mismos ficheros, y aquí aparece las segunda opción, conflictos.

**Conflictos**

Siempre que aparecen conflictos en un merge, git nos lo comunica con un mensaje por la terminal, indicando en qué fichero hay conflictos.

Si entramos en el fichero, tendremos una estructura así:

*Ejemplo*
```
<<<<<<< HEAD
// Aquí está el codigo que tenemos
=======
// Aqueí está el código nuevo
>>>>>>> Commit de la rama que nos queremos quedar
```

Aquí hay que decidir con qué código nos quedamos y con cual no, o qué modificaciones hay que hacer según el código. Por tanto, modificamos el fichero arreglando el conflicto y borramos el código que no queremos juntamente con los mensajes que ha creado git.

## Part 2
En el nivel anterior se ha comentado el uso de git guardando la información localmente, en el propio ordenador. El ogjetivo de este nivel es explicar el uso de git remoto, en un repositorio de internet.

### Git clone
Si queremos trabajar sobre el repositorio d einternet, debemos utilizar el comando git clone [enlace http o ssh que nos proporciona el repositorio].
Un ejemplo seria > git clone https://github.com/exemple/directori.git
Este comando descarga la rama master, las otras ramas se eligen con otros comandos que ya veremos.
### Origin
Cuando se trabaja con git, se puede guardar información en local o en remoto (conocido como origin). Origin es la información conectada con el repositorio de internet.

Decimos que la información de los commits/proyectos, en total, se puede guardar de tres formas:

*Ejemplo*
```
Repositorio
---|-----|------|--
 j23k   l12j   k32g
__________
Origin
---|-----|--
 j23k   l12j
__________
Local
---|-----|----
 j23k  a34f
```

Como podemos ver, tenemos la versión del repositorio, la versión de origin y nuetsra versión, la local.
La versión del repositorio tienes tres commits, la versión de origin tiene dos de ellas, y finalmente la rama local tiene uno de ellos, más uno que hemos hecho nosotros, que no tiene nadie más.

Tenemos muchas posibilidades en esta situación, podemos actualizar el origin, podemos actualizar el local sin actualizar el origin, podemos actualizar origin y local a la vez, etc.

### Git pull origin rama

Cuando nosotros estamos en local y queremos descargar información del repositorio, hay que seguir unos pasos:

Primero, hay que tener una rama con el mismo nombre que la rama que queremos descargar o actualizar. Por tanto, si queremos descargar la rama development, y no la tenemos creada, debemos de ejecutar git branch development y git checkout development.

Cuando tenemos la rama con el mismo nombre en el repositorio, ya podemos utilizar el comando git pull origin [nombre de la rama]. Este comando cogerá la información del repositorio y actualizarña origin y la rama local (la rama desde la que hemos ejecutado el comando).

Atención, si tenemos algunos commits en la rama local, que no tiene repositorio, y tocamos la misma parte de código que los commits que descargamos, habrá conflictos y habrá que solucionarlos.

*Ejemplo*
```
> git pull origin development
Repositorio
---|-----|------|-- <= development
 j23k   l12j   k32g
__________      \
Origin           \
---|-----|--------|- <= development
 j23k   l12j     k32g
_________\        \
Local     \        \
---|-----|-|--------|- <= development
 j23k  a34f        k32g
          l12j
```

### Git push origin rama

Cuando nuestra verisón (local) del proyecto tiene todos los commits del repositorio, y a demás tiene commits que hemos hehco nosotros, nos interesa actualizar el repositorio. Para hacerlo, utilizaremos el comando git push origin [nombre d ela rama]. Este comando coge todos los commits que tenemos en nuetsra rama y los envía al repositorio remoto, actualizándolo.

*Ejemplo*
```
Repositorio                   a34f
---|-----|------|-------------|- <= development
 j23k   l12j   k32g          /
__________                  /
Origin                a34f /
---|-----|------|---------|- <= development
 j23k   l12j   k32g      /
__________              /
Local                  /
---|-----|------|-----|--- <= development
 j23k   l12j   k32g  a34f
```

También podemos hacer git push -u origin [nombre de la rama], así las siguiente veces solo hará falta utilizar el comando git push, y git ya sabrá a qué localización hacer push.

## Parte 3

En este apartado veremos más a fondo los comandos que trabajan sobre origin y el repositorio, a parte de comandos más complejos.

### Git fetch
Si nuestra intención es actualizar el origin descargando los datos del repositorio, debemos actualizar con git fetch.

*Ejemplo*
```
> git pull origin development
Repositorio
---|-----|------|-- <= development
 j23k   l12j   k32g
__________      \   <= >git fetch
Origin           \
---|-----|--------|- <= development
 j23k   l12j     k32g
```

### Git pull

Si lo que queremos es actualizar el origin y actualizar la rama en la que trabajamos, utilizaremos git pull. Este comando es una convinación de git fetch y git merge [rama en las uqe estamos], es decir:

*Ejemplo*
```
>git pull == >git fetch => >git merge

Repositori
---|-----|------|-- <= development
 j23k   l12j   k32g
__________      \   <= git fetch
Origin           \
---|-----|--------|- <= development
 j23k   l12j     k32g
_________\        \ <= git merge "rama en la que estamos"
Local     \        \
---|-----|-|--------|- <= development
 j23k  a34f        k32g
          l12j
```

¿Qué significa esto? Este comando es muy útil, pero a veces peligroso si estamos trabajando sobre una rama y haciendo modificaciones que no tiene el repositorio, este comando puede ocasionar conflictos (en la parte del merge).

### Git revert

Este comando es simple, pero muy peligroso. Si hemos hecho un merge con una rama equivocada, o un error que el commit anterior no tenía, este comando es el indicado, pero si nuestro objetivo es tirar más de un commit atrás, hay que vigilar mucho con este comando, ya que puede provocar muchos errores.

La intención del comando es hacer un nuevo commit con el uso de un commit anterior. ¿Qué quiere decir esto? Podriamos decir que es un backup de información anterior.

*Ejemplo*
```
--------|---------|-------|-- <= popUp
      g45o      i15g    a31o

> git revert i15g

                   ____________
                  /            \
--------|---------|-------|-----| <= popUp
      g45o      i15g    a31o   w32v (anterior commit i15g)
```

<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>
