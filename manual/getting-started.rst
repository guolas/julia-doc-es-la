.. _man-getting-started:

***********
 Comenzando  
***********

La instalación de Julia es directa, ya sea utilizando archivos binários pre-compilados, o
compilando el código fuente. Baje e instale Julia siguiendo las instrucciones (en inglés)
en `http://julialang.org/downloads/ <http://julialang.org/downloads/>`_.

La manera más fácil de aprender y experimentar con Julia es iniciando una sesión
interactiva (también conocida como *read-eval-print loop* o *"repl"*)::

    $ julia
                   _
       _       _ _(_)_     |
      (_)     | (_) (_)    |  A fresh approach to technical computing.
       _ _   _| |_  __ _   |
      | | | | | | |/ _` |  |  Version 0 (pre-release)
      | | |_| | | | (_| |  |  Commit 61847c5aa7 (2011-08-20 06:11:31)*
     _/ |\__'_|_|_|\__'_|  |
    |__/                   |

    julia> 1 + 2
    3

    julia> ans
    3


Para cerrar la sesión interactiva, se ha de pulsar ``^D``- la tecla *Ctrl* 
junto a la tecla ``d`` - o escribir ``quit()`` o ``exit()``. Cuando se utiliza 
Julia en modo interativo, ``julia`` muestra un *banner* y espera a 
que el usuario introduzca un comando. Una vez que el usuario ha introducido un comando,
como ``1 + 2``, y presionado *enter*, la sesión interativa evalúa la 
expresión y muestra el resultado. Si, en una sesión interactiva, una expresión se
finaliza con un punto y coma, la expresión se evaluará, pero el resultado no será
mostrado por pantalla. La varible ``ans`` almacena el resultado 
de la última expresión evaluada, se haya mostrado o no el resultado.

Para calcular expresiones escritas en un archivo ``file.jl``, se ha de utilizar
el comando ``include("file.jl")``.

Para ejecutar el código contenido en un archivo de forma no interactiva, el
nombre del archivo puede pasarse como primer argumento al ejecutar ``julia``::

    $ julia script.jl arg1 arg2...

Como sugiere el ejemplo, los siguientes argumentos se utilizan como los argumentos
para el programa ``script.jl``, pasados en la constante global ARGS. El valor de la
constante ARGS también se inicializa cuando el código a ejecutar se introduce
utilizando la opción ``-e`` en la línea de comandos (vea la salida de la ayuda de
``julia`` más abajo). Por ejemplo, para imprimir los argumentos pasados a un
*script*, se puede hacer lo siguiente::

    $ julia -e 'for x in ARGS; println(x); end' foo bar
    foo
    bar

O se puede introducir este código en un *script* y ejecutarlo::

    $ echo 'for x in ARGS; println(x); end' > script.jl
    $ julia script.jl foo bar
    foo
    bar

Si se require que cierto código se ejecute cada vez que ``julia`` inicie, se
puede incluir en el fichero ``~\.juliarc.jl``::

	$ echo 'println("Greetings! 你好! 안녕하세요?")' > ~/.juliarc.jl
	$ julia
	Greetings! 你好! 안녕하세요?
	
	...

Existen múltiples formas de ejecutar código en Julia, y múltiples opciones. De
forma similar a como se hace en ``perl`` o ``ruby``, la opción ``--help``
permite mostrar estas opciones::

    $ julia --help
    julia [options] [program] [args...]
     -v --version             Display version information
     -q --quiet               Quiet startup without banner
     -H --home=<dir>          Load files relative to <dir>
     -T --tab=<size>          Set REPL tab width to <size>

     -e --eval=<expr>         Evaluate <expr>
     -E --print=<expr>        Evaluate and show <expr>
     -P --post-boot=<expr>    Evaluate <expr> right after boot
     -L --load=file           Load <file> right after boot
     -J --sysimage=file       Start up with the given system image file

     -p n                     Run n local processes
     --machinefile file       Run processes on hosts listed in file

     --no-history             Don't load or save history
     -f --no-startup          Don't load ~/.juliarc.jl
     -F                       Load ~/.juliarc.jl, then handle remaining inputs

     -h --help                Print this message


Tutoriales
----------

Se pueden encontrar guías paso a paso online:

- `Forio Julia Tutorials (en inglés) <http://forio.com/julia/tutorials-list>`_
- `Clase sobre análisis numérico de Homer Reid (en inglés) <http://homerreid.ath.cx/teaching/18.330/JuliaProgramming.shtml#SimplePrograms>`_
- `Vídeos de los tutoriales de Julia en el MIT <http://julialang.org/blog/2013/03/julia-tutorial-MIT/>`_

Diferencias notables con MATLAB
-------------------------------

Los usuarios de MATLAB encontrarán familiar la sintaxis de Julia. Sin embargo,
Julia no es, de ningún modo, un clón de MATLAB, ya que existen importantes
diferencias sintácticas y funcionales. Las siguientes son algunas de las diferencias
más notables, que pueden confundir a usuarios acostumbrados al uso de MATLAB::

- Los *arrays* son indexados usando corchetes, ``A[i,j]``.

- La unidad imaginaria ``sqrt(-1)`` se representa en Julia con ``im``.

- La devolución y asignación de múltiples valores de forma simultánea se realiza
  con paréntesis, ``return (a, b)`` y ``(a, b) = f(x)``.

- Los valores se pasan por referencia. Si una variable de entrada de una función
  se modifica en el interior, los cambios serán visibles desde donde se invocó la
  función.

- Julia tiene *arrays* unidimensionales. Los vectores columna son de tamaño ``N``
  y no ``Nx1``. Por ejemplo, ``randn(N)`` crea un *array* unidimensional.

- La concatenación de escalares y *arrays* utilizando la sintaxis ``[x, y, z]``
  concatena en la primera dimensión ("verticalmente"). Para que la concatenación
  sea en la segunda dimensión ("horizontalmente"), se ha de utilizar espacios,
  ``[x y z]``. Para construir matrices por bloques (mediante la concatenación en
  las dos primeras dimensiones), se utiliza la sintaxis ``[a b; c d]`` para
  evitar confusiones.

- Los dos puntos en ``a:b`` y en ``a:b:c`` crean objectos del tipo ``Range``.
  Para construir un *array*, se ha de utilizar ``linspace`` o "concatenar" el
  rango escribiéndolo entre corchetes, ``[a:b]``.

- Las funciones devuelven valores mediante la palabra reservada ``return``, en
  vez de indicando los nombres de las variables a devolver en la definición de
  la función (ver :ref:`man-return-keyword` para más información).

- Un archivo puede contener cualquier número de funciones, y todas ellas serán
  accesibles una vez se haya cargado el archivo.

- Reducciones como ``sum``, ``prod`` y ``max`` se realizan sobre todos y cada
  uno de los elementos de un *array* cuando se invocan con único argumento,
  como en ``sum(A)``.

- Funciones como ``sort``, que operan por columnas por defecto (``sort(A)``
  equivale a ``sort(A, 1)``), no tienen ningún comportamiento especial para 
  arrays de dimensiones ``1xN``; el argumento se devuelve sin modificaciones,
  puesto que ejecuta ``sort(A, 1)``. Para que funcione en un *array* de
  dimensiones ``1xN`` como en un vector de ``Nx1``, se ha de ejecutar como
  ``sort(A, 2)``.

- Los paréntesis se han de utilizar incluso para invocar una función sin
  parámetros, como es el caso de ``tic()`` y ``toc()``.

- El punto y coma no es necesario al final de cada cláusula. El resultado
  de una cláusula no se muestra (excepto cuando Julia se ejecuta en modo
  interactivo), y las líneas de código no tienen por qué acabarse con un punto
  y coma. Se puede emplear la función ``println`` para imprimir un valor
  seguido de un salto de línea.

- Si ``A`` y ``B`` son *arrays*, ``A == B`` no devuelve un *array* de booleanos.
  Para conseguir este funcionamiento, se ha de emplear ``A .== B``. Otros
  operadores booleanos, como ``<``, ``>``, ``!=``, se comportan de la misma
  manera.

- Los elementos de una colección pueden pasarse como parámetros a una función
  utilizando ``...``, como en ``xs=[1,2]; f(xs...)``.

- La función ``svd`` en Julia devuelve los valores singulares como un vector
  en vez de como una matriz diagonal.

Diferencias notables con R
--------------------------

Uno de los objetivos de Julia es proporcionar un lenguaje efectivo para el análisis
de datos y la programación estadística. Para usuarios que vengan a Julia desde R,
éstas son algunas de las diferencias más importantes::

- Julia usa ``=`` para la asignación. Julia no proporciona ningún otro operador
  como ``<-`` o ``<<-``.

- Julia construye los vectores utilizando corchetes. ``[1, 2, 3]`` en Julia es
  equivalente a ``c(1, 2, 3)`` en R.

- Las operaciones con matrices de Julia son más parecidas a la notación matricial
  tradicional que a la de R. Si ``A`` y ``B`` son matrices, entonces ``A * B``
  representa la multiplicación de matrices equivalente a ``A %*% B`` en R. En R,
  esta notación realizaría una multiplicación elemento a elemento, o producto
  de Hadamard. Para obtener la multiplicación elemento a elemento en Julia, se
  ha de hacer ``A .* B``.

- La trasposición de matrices en Julia se realiza mediante el operador ``'``.
  ``A'`` en Julia equivale, por tanto, a ``t(A)`` en R.

- Julia no necesita los paréntesis en las cláusulas ``if`` o en los bucles
  ``for``: se ha de usar ``for i in [1, 2, 3]`` en lugar de ``for (i in c(1, 2, 3))``
  e ``if i == 1`` en vez de ``if (i == 1)``.

- Julia no trata los números ``0`` y ``1`` como booleanos. No se puede escribir
  ``if (1)`` en Julia, porque la cláusula ``if`` sólo acepta valores lógicos. En
  su lugar, se puede utilizar ``if true``.

- Julia no ofrece funciones ``nrow`` o ``ncol``. Se ha de emplear ``size(M, 1)``
  en lugar de ``nrow(M)`` y ``size(M, 2)`` en lugar de ``ncol(M)``.

- La SVD en Julia no es reducida, por defecto, a diferencia de en R. Para obtener
  resultados similares a los que proporciona R, en general habrá que ejecutar
  ``svd(X, true)`` en una matriz ``X``.

- Julia es un lenguaje muy estricto en la distinción entre escalares, vectores
  y matrices. En R, ``1`` y ``c(1)`` son lo mismo. En Julia esto no es así. Esto
  puede dar lugar a confusiones como que ``x' * y``, cuando ``x`` e ``y`` son
  vectores, da como resultado un vector con un único elemento, y no un escalar. Para
  obtener un escalar sería necesario ejecutar ``dot(x, y)``.

- Las funciones ``diag()`` y ``diagm()`` en Julia no son lo mismo que en R.

- No se puede asignar al resultado de una función en el término a la izquierda
  del igual en una asignación: no se puede escribir ``diag(M) = ones(n)``.
   
- Julia desaconseja ocupar el *namespace* principal con funciones. La mayoría de
  las funciones estadísticas en Julia se pueden encontrar en `paquetes <http://docs.julialang.org/en/latest/packages/packagelist/>`_
  como DataFrames y Distributions::

  - Funciones de distribución se pueden encontrar en el `paquete Distributions <https://github.com/JuliaStats/Distributions.jl>`_
	
  - El `paquete DataFrames <https://github.com/HarlanH/DataFrames.jl>`_ proporciona *data frames*.

  - Fórmulas para GLM se deben de escapar: se ha de utilizar  ``:(y ~ x)`` en lugar de ``y ~ x``.

- Julia proporciona tuplas y tablas *hash* reales, pero no las listas de R. Para
  devolver múltiples elementos, típicamente se ha de utilizar una tupla: en vez
  de ``list(a = 1, b = 2)`` se ha de utilizar ``(1, 2)``.

- Julia promueve que todos los usuarios definan sus propios tipos. Los tipos
  en Julia son mucho más sencillos de utilizar que los objetos S3 o S4 en R. El
  sistema de *multiple dispatch* de Julia permite que ``table(x::TypeA)`` y ``table(x::TypeB)``
  sean equivalentes a ``table.TypeA(x)`` y ``table.TypeB(x)`` en R.

- En Julia los valores se pasan y se asignan por referencia. Si una variable de
  entrada de una función se modifica en el interior, los cambios serán visibles
  desde donde se invocó la función. Esto es muy diferente a cómo funciona R, y
  permite que nuevas funciones operen con estructuras de datos de gran tamaño de
  forma eficiente.

- La concatenación de vectores y matrices se realiza con ``hcat`` y ``vcat``, y
  no con ``c``, ``rbind`` y ``cbind``.

- Un objecto ``Range`` ``a:b`` en Julia no es un atajo para crear un vector,
  como en R, sino que es un tipo de objecto especial que se emplea para realizar
  iteraciones sin requerir mucha memoria. Para convertir un ``Range`` en un
  vector, es preciso escribirlo entre corchetes ``[a:b]``.
  
- Julia tiene algunas funciones que son capaces de modificar los argumentos. Por
  ejemplo existe ``sort(v)`` y ``sort!(v)``.

- En R, eficiencia requiere vectorización. En Julia, casi lo contrario es cierto:
  en general el código desempeña mejor cuando utiliza bucles no vectorizados.

- A diferencia de R, no hay ejecución retardada en Julia. Para la mayor parte de
  los usuarios esto significa qeu hay muy pocas expresiones o nombres de columnas
  sin comillas.

- Julia no soporta el tipo ``NULL``.

- En Julia no hay equivalente a las funciones ``assign`` o ``get`` en R.
