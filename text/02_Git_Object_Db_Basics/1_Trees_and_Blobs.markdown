### El Objeto Blob ###

Un blob generalmente almacena el contenido de un archivo.

[fig:object-blob]

Puedes usar linkgit:git-show[1] para examinar el contenido de cualquier blob.
Asumiendo que tenemos el SHA1 de un blob, podemos examinar su contenido así:

    $ git show 6ff87c4664

     Note that the only valid version of the GPL as far as this project
     is concerned is _this_ particular version of the license (ie v2, not
     v2.2 or v3.x or whatever), unless explicitly otherwise stated.
    ...

Un objeto "blob" no es más que un fragmento de datos binarios. No se refiere
a nada mas, ni tiene atributos de ningun tipo, ni siquiera un nombre de archivo.

Ya que el blob esta enteramente definido por sus datos, si dos archivos en un
árbol de directorio (o en múltiples diferentes versiones de un repositorio)
tienen el mismo contenido, compartirán el mismo objeto blob. Este objeto es
totalmente independiente de su ubicación en el árbol de directorio, y 
renombrar un archivo no cambiará el objeto al cual ese fichero esté asociado.

### Objeto árbol ###

Un árbol es un objeto simple que tiene un puñado de punteros hacia blobs y
hacia otros árboles - generalmente representa el contenido de un directorio o 
un subdirectorio.

[fig:object-tree]

El siempre-versatil comando linkgit:git-show[1] también puede ser usado para 
examinar objetos árbol, pero linkgit:git-ls-tree[1] te dara mas detalles.
Asumiendo que tenemos el SHA1 de un árbol, podemos examinarlo de esta manera:

    $ git ls-tree fb3a8bdd0ce
    100644 blob 63c918c667fa005ff12ad89437f2fdc80926e21c    .gitignore
    100644 blob 5529b198e8d14decbe4ad99db3f7fb632de0439d    .mailmap
    100644 blob 6ff87c4664981e4397625791c8ea3bbb5f2279a3    COPYING
    040000 tree 2fb783e477100ce076f6bf57e4a6f026013dc745    Documentation
    100755 blob 3c0032cec592a765692234f1cba47dfdcc3a9200    GIT-VERSION-GEN
    100644 blob 289b046a443c0647624607d471289b2c7dcd470b    INSTALL
    100644 blob 4eb463797adc693dc168b926b6932ff53f17d0b1    Makefile
    100644 blob 548142c327a6790ff8821d67c2ee1eff7a656b52    README
    ...

Como puedes ver, un objecto árbol contiene una lista de entradas, cada una con 
un modo, tipo de objeto, nombre SHA1 y nombre, ordenado por nombre. Representa 
el contenido de un árbol de directorio en particular.

Un objeto referenciado por un árbol puede ser un blob, representando el 
contenido de un archivo, u otro árbol, representando el contenido de un 
subdirectorio. Siendo que los árboles y los blobs, como todos los demas 
objetos, son llamados por el hash SHA1 de su contenido, dos árboles tienen el 
mismo nombre SHA1 si y solo si su contenido (incluyendo, recursivamente, el 
contenido de todos sus subdirectorios) es idéntico.
Esto permite a git rápidamente determinar las diferencias entre dos árboles 
relacionados, ya que puede ignorar cualquier entrada con nombres de objeto 
idénticos.

(Nota: en precencia de submódulos, los árboles tambien pueden tener commits 
como entradas. Ver la seccion **Submodulos**.)

Notarás que todos los archivos tienen modo 644 o 755: git en realidad 
sólo presta atención al bit ejecutable.

