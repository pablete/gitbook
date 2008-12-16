## El Modelo de Objetos de Git ##

### El SHA ###

Toda la información necesaria para reprensentar la historia de un projecto 
esta almacenada en ficheros referenciados por un "nombre de objeto" de 
40-digitos parecido a algo como esto:
    
    6ff87c4664981e4397625791c8ea3bbb5f2279a3
    
Veras estas cadenas de 40-caracteres por todos lados en Git.
En cada caso el nombre es calculado tomando el hash SHA1 del contenido del 
objeto. El hash SHA1 es una función hash cryptográfica.
Esto, para nosotros, significa que es virtualmente imposible encontrar dos 
objetos diferentes con el mismo nombre. Esto tiene un número de ventajas; 
entre otras:

- Git puede determinar rápidamente si dos objetos son idénticos o no, sólo   
  comparando sus nombres.
- Como los nombres de objetos son calculados de la misma manera en cada 
  repositorio, el mismo contenido almacenado en dos repositorios estará 
  siempre almacenado bajo el mismo nombre.
- Git puede detectar errores cuando lee un objeto, con sólo chequear que el 
  nombre del objeto sigue siendo el hash SHA1 de su contenido.

### Los Objectos ###

Cada objeto consiste de tres cosas - un **tipo**, un **tamaño** y un **contenido**.
El _tamaño_ es simplemente el tamaño del contenido, el contenido depende del tipo de objeto que es, y existen cuatro diferentes tipos de objetos:
"blob", "tree", "commit" y "tag". 

- Un **"blob"** es usado para almacenar ficheros de datos - es generalmente un fichero.
- Un **"tree"** es basicamente un directorio - referencia a un puñado de otros 
     trees y/o blobs (por ejemplo: ficheros y sub-directorios)
- Un **"commit"** apunta a un solo tree, marcando cómo el projecto lucia in un    
     cierto punto en el tiempo. Contiene meta-información acerca de ese punto 
     en el tiempo, como la fecha y hora, el autor de los cambios desde el   
     ultimo commit, un puntero a los commits anteriores, etc.
- Un **"tag"** es una manera de marcar un commit particular como especial de 
     alguna manera. Es usado normalmente para marcar ciertos commits como    
     releases especificas, o algo similar en esta línea.

Casi todo Git esta construido manipulando esta simple estructura de cuatro 
tipos de objeto difrentes. Es como una especie de pequeño sistema de archivos 
funcinando por encima del sistema de archivos de tu maquina.

### Diferente de SVN ###

Es importante remarcar que esto es muy diferente de la mayoria de los VCS con 
los que puede ser que estes familiarizado. Subversion, CVS, Perforce, 
Mercurial y similares usan todos un sistema de _Almacenamiento de Deltas_
guardan las diferencias entre un commit y el siguiente. Git no hace esto 
último, sino que almacena una foto instantánea de como lucían los archivos en 
esta estructura de árbol cada vez que haces un commit. Esto es un concepto muy 
importante de entender cuando uses Git.