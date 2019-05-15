# UD8 ShellScripts

- [UD8 ShellScripts](#ud8-shellscripts)
- [Introducción](#introducci%C3%B3n)
- [Variables](#variables)
  - [Nombres de las variables](#nombres-de-las-variables)
  - [Operaciones con variables](#operaciones-con-variables)
    - [Operaciones básicas: sumar, restar, dividir y multiplicar](#operaciones-b%C3%A1sicas-sumar-restar-dividir-y-multiplicar)
    - [Incrementar valor variables](#incrementar-valor-variables)
    - [Operaciones con decimales](#operaciones-con-decimales)
  - [Leer variables desde teclado](#leer-variables-desde-teclado)
    - [Solitar dato y leer respuesta](#solitar-dato-y-leer-respuesta)
    - [Leer más de una variable](#leer-m%C3%A1s-de-una-variable)
    - [Lectura con timeout](#lectura-con-timeout)
    - [Lectura de contraseñas](#lectura-de-contrase%C3%B1as)
    - [Lectura de teclado carácter a carácter](#lectura-de-teclado-car%C3%A1cter-a-car%C3%A1cter)
- [Control de flujo](#control-de-flujo)
  - [Comparaciones de cadenas alfanuméricas](#comparaciones-de-cadenas-alfanum%C3%A9ricas)
  - [Comparaciones de cadenas numéricas](#comparaciones-de-cadenas-num%C3%A9ricas)
  - [Comparaciones de atributos de fichero](#comparaciones-de-atributos-de-fichero)
  - [Condicionales](#condicionales)
    - [if/else](#ifelse)
  - [Bucles](#bucles)
  - [for](#for)
  - [while](#while)
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

En esta unidad didáctica vamos a aprender a hacer algunos scripts que nos permitirán introducirnos en la programación de sistemas operativos. Un script no es más que un archivo que contiene un conjunto de órdenes para realizar una acción.

# Introducción

Vamos a crear nuestro primer script, para ello en un editor de texto como [Nano] escribimos lo siguiente y lo guardaremos con el nombre de `hola.sh`

```bash
#!/bin/bash
# Este es nuestro primer programa
echo Hola mundo
```
La primera línea de nuestro script le indica al sistema que tiene que usar la shell `bash`. 
La segunda línea es un comentario para el programador, todas las líneas que comiencen por `#` son ignorados por el intérprete de shell, a excepción de la primera línea. En la tercera línea tenemos el comando `echo` que muestra el texto que sigue por pantalla.

# Variables

Para guardar información en el ordenador, que más adelante nos puede hacer falta, necesitamos de variables que almacenen dicha información. Podemos hacer la similitud que una variable es una caja en la cual podemos guardar palabras o números.

Para asignar el valor a una variable debemos hacer lo siguiente:

```bash
# Almacenar un número entero
let variable=0
# Almacenar un texto
variable=casa
```

**Es muy importante no dejar espacios ni antes ni después del `=`**

Para usar el valor almacenado dentro de la variable solo tenemos que anteponer el símbolo del dolar `$` antes del nombre de la variable

```bash
# Muestra por pantalla el valor almacenado en variable
echo $variable
```

A lo largo de un script podemos asignarles diferentes valores a una misma variable

```bash
#!/bin/bash
variable=Hola Mundo
echo $variable
let variable=5.5
echo $variable
```
## Nombres de las variables

Las variables pueden tomar prácticamente cualquier nombre, sin embargo, existen algunas restricciones:

* Sólo puede contener caracteres alfanuméricos y guiones bajos
* El primer carácter debe ser una letra del alfabeto o `_` (este último caso se suele reservar para casos especiales).
* No pueden contener espacios.
* Las mayúsculas y las minúsculas importan, “a” es distinto de “A”.
* Algunos nombres son usados como variables de entorno y no los debemos utilizar para evitar sobrescribirlas (e.j.,PATH).
 
De manera general, y para evitar problemas con las variables de entorno que siempre están escritas en mayúscula, deberemos escribir el nombre de las variables en minúscula.

Además, aunque esto no es una regla que deba obedecerse obligatoriamente, es conveniente que demos a las variables nombres que más tarde podamos recordar. Si abrimos un script tres meses después de haberlo escrito y nos encontramos con la expresión `let m=3.5` nos será difícil entender que hace el programa. Habría sido mucho más claro nombrar la variable como `let media=3.5`.

## Operaciones con variables

### Operaciones básicas: sumar, restar, dividir y multiplicar

Para realizar cálculos en [Bash] podemos emplear alguna de las siguientes opciones:

1- Encerrar la operación matemática dentro de `$((operacion))`

```bash
var1=3
var2=10
echo $(($var1/$var2))
```

2- Emplear la herramienta `let`

```bash
let a=4*9+5
echo $a
```

Para anidar operaciones, podemos emplear variables intermedias

```bash
let a=2
let b=6
let c=a*b
echo $c
```

### Incrementar valor variables

Para incrementar o decrementar el valor de una variable en [Bash] podemos emplear los siguientes métodos:

```bash
#Incrementar
var=$((var+1))
((var=var+1))
((var+=1))
((var++))
let "var=var+1"
let "var+=1"
let "var++"
```

```bash
#Decrementar
var=$((var-1))
((var=var-1))
((var-=1))
((var--))
let "var=var-1"
let "var-=1"
let "var--"
```

### Operaciones con decimales

[Bash] no permite operar con decimales, sin embargo podemos emplear la herramienta [bc]. 

```bash
#!/bin/bash
let operador1=3
let operador2=2
division=$(echo "scale=2; $operador1/$operador2" | bc)
echo "El cociente de "$operador1" / "$operador2" es "$division
```

El valor de `scale=2` nos indica el número de decimales qué queremos mostrar. En nuestro caso `2`

## Leer variables desde teclado

En ocasiones será necesario que realicemos scripts interactivos para que el usuario escoja alguna opción.

Tenemos diversas formas de hacerlo, veamos ejemplos de cada una de ellas:

### Solitar dato y leer respuesta

El comando `read` se encarga de leer una línea introducida por el usuario y asignársela a una variable. Casi siempre se utiliza con el modificador `-p` (prompt), para presentar en pantalla un texto en el que se solicita el dato.

```bash
#!/bin/bash
read -p "Nombre del fichero: " nombre_fichero
echo "El fichero a procesar es: $nombre_fichero"
```

### Leer más de una variable

El comando `read` admite especificar más de una variable a ser leída. En este caso, el script divide la información introducida por el usuario utilizando como separador el carácter `IFS` que por defecto es un espacio en blanco.

```bash
#!/usr/bin/bash
read -p "Introduzca varios números: " num1 num2 
echo "El primer número es $num1"
echo "El segundo número es $num2"
```

La ejecución del script anterior genera la siguiente salida:

```bash
$ ./lectura_de_teclado.sh
Introduzca varios números: uno dos
El primer número es uno
El segundo número es dos

$ ./lectura_de_teclado.sh
Introduzca varios números: uno dos tres cuatro
El primer número es uno
El segundo número es dos tres cuatro
```

En el segundo caso se han introducido más datos de los que espera el script, y el resultado es que la última variable `$num2` se le asignan los datos restantes.

### Lectura con timeout

Para quedar a la espera de que el usuario introduzca el dato que se le solicita durante un tiempo limitado, el comando `read` dispone del modificador `-t`, que permite especificar el número máximo de segundos antes de que el script continue su ejecución.

Si se alcanza el tiempo límite, el valor de las variables a leer queda en blanco, y el comando `read` devuelve un código de status distinto de cero:

```bash
#!/bin/bash
if read -t 60 -p "Nombre del fichero: " nombre_fichero; then
    echo "El nombre del fichero es: $nombre_fichero"
else
    echo "Se ha alcanzado el límite de tiempo. Código de salida: $?"
fi
```

### Lectura de contraseñas

Si el script necesita solicitar una contraseña al usuario, es interesante hacerlo de tal modo que lo que teclea el usuario no se presente en pantalla. El modificador `-s` del comando `read` implementa esta funcionalidad:

```bash
#!/bin/bash
read -s -p "Clave de acceso : " mi_clave
echo
echo "La clave que has introducido es: - $mi_clave"
```

### Lectura de teclado carácter a carácter

Por defecto, el comando `read` espera a que el usuario introduzca una línea completa, finalizando con la tecla `Intro`

Pero en ocasiones, nos puede interesar que el comando lea cualquier tecla que haya sido pulsada, sin esperar a que finalice la línea.

Esto se consigue utilizando el modificador `-n`, para especificar el número de caracteres que se desea leer. En particular, con `-n 1` indicamos al comando `read` que finalize en cuanto el usuario haya pulsado una tecla:

```bash
#!/bin/bash
read -n 1 -p "Pulsa una tecla : " mi_caracter
echo
echo "La tecla pulsada es: - $mi_caracter"
```

# Control de flujo

Como hemos visto los scripts se ejecutan línea a línea hasta llegar al final, sin embargo, muchas veces nos interesará modificar ese comportamiento de manera que el programa pueda responder de un modo u otro dependiendo de las cirscunstancias o pueda repetir trozos de código.

Estas construcciones nos ayudan a controlar la ejecución del script y se basan en su mayoría en la comparación de cadenas numéricas, alfanuméricas o de atributo de ficheros.

Veamos como se definen:

## Comparaciones de cadenas alfanuméricas


| Operador           | Verdad si...                                            |
| ------------------ | ------------------------------------------------------- |
| cadena1 = cadena2  | cadena1 es igual a cadena2                              |
| cadena1 != cadena2 | cadena1 no es igual a cadena2                           |
| cadena1 < cadena2  | cadena1 es menor que cadena2                            |
| cadena1 > cadena 2 | cadena1 es mayor que cadena 2                           |
| -n cadena1         | cadena1 no es igual al valor nulo (longitud mayor que 0) |
| -z cadena1         | cadena1 tiene un valor nulo (longitud 0)                |

## Comparaciones de cadenas numéricas

| Operador | Verdad si...          |
| -------- | --------------------- |
| x -lt y  | x menor que y         |
| x -le y  | x menor o igual que y |
| x -eq y  | x igual que y         |
| x -ge y  | x mayor o igual que y |
| x -gt y  | x mayor que y         |
| x -ne y  | x no igual que y      |

## Comparaciones de atributos de fichero

|Operador | Verdad si...|
|---------|-------------|
-d fichero | fichero existe y es un directorio
-e fichero | fichero existe
-f fichero | fichero existe y es un fichero regular (no un directorio, u otro tipo de fichero especial)
-r fichero | Tienes permiso de lectura en fichero
-s fichero | fichero existe y no esta vacio
-w fichero | Tienes permiso de escritura en fichero
-x fichero | Tienes permiso de ejecucion en fichero (o de busqueda si es un directorio)
-O fichero | Eres el dueño del fichero
-G fichero | El grupo del fichero es igual al tuyo.
fichero1 -nt fichero2 | fichero1 es mas reciente que fichero2
fichero1 -ot fichero2 | fichero1 es mas antiguo que fichero2

## Condicionales

Se pueden combinar varias condiciones con los símbolos `&&` y `||`, y negar una condición con `!`. En los ejemplos de los apartados siguientes se verán cómo se emplean con mayor claridad.

### if/else

Comprueba si la `[CONDICIÓN]` se cumple. La sintaxis básica es la siguiente:

```bash
if [ CONDICIÓN ];
then
    Se ejecuta la instrucción si se cumple la condición
fi
```

También podemos especificar qué hacer si la condición no se cumple

```bash
if [ CONDICIÓN ];
then
    Se ejecuta la instrucción si se cumple la condición
else
    Se ejecuta la instrucción si NO se cumple la condición
fi
```

Se pueden añadir más condiciones concatenando más `if`:

```bash
if [ CONDICIÓN1 ];
then
    Se ejecuta la instrucción si se cumple la condición1
elif [ CONDICION2 ]
    Se ejecuta la instrucción si se cumple la condición2
else
    Se ejecuta la instrucción si NO se cumplen las condiciones
fi
```

## Bucles

## for

## while

[Nano]: https://www.nano-editor.org/dist/v2.2/nano.html
[Bash]: https://es.wikipedia.org/wiki/Bash
[bc]: https://ss64.com/bash/bc.html