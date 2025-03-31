**Paso 1: Lo Más Básico - `echo` y Salida de Texto**

* **Concepto:** Mostrar mensajes en la pantalla. Es el comando fundamental para ver qué está haciendo un script o para mostrar información al usuario.
* **Script (`paso1_saludo.sh`):**

```bash
#!/bin/bash

# Este es un comentario. El script empieza aquí.
# Muestra un mensaje simple en la terminal.

echo "¡Hola, futuros expertos en Bash!"
echo "Este es nuestro primer script."
echo "Podemos mostrar texto,"
echo "números como 1, 2, 3,"
echo "y caracteres especiales si los 'escapamos' con \ o usamos comillas:"
echo "Por ejemplo, una barra invertida \\ o un símbolo de dólar \$"
echo "O usando comillas simples: 'Todo aquí dentro es literal, $HOSTNAME no se expande'"
```

* **Explicación:**
    * `#!/bin/bash`: El "shebang", indica qué intérprete usar.
    * `#`: Comentarios, ignorados por Bash.
    * `echo`: El comando para imprimir texto en la salida estándar (la terminal, por defecto).
    * Se muestra cómo las comillas dobles (`"`) permiten la expansión de variables (aunque no usamos ninguna aquí aún) y secuencias de escape (`\`), mientras que las comillas simples (`'`) tratan todo de forma literal.



**Paso 2: Introducción a las Variables**

* **Concepto:** Guardar información (texto, números) en "contenedores" con nombre para usarla más tarde.
* **Script (`paso2_variables.sh`):**

```bash
#!/bin/bash

# Definimos algunas variables
nombre_curso="Scripting en Bash"
numero_alumnos=15
mensaje_bienvenida="Bienvenidos al curso de ${nombre_curso}" # {} son opcionales aquí, pero buena práctica

# Mostramos el contenido de las variables
echo $mensaje_bienvenida
echo "Actualmente hay $numero_alumnos alumnos inscritos." # Dentro de "" se expanden
echo 'Con comillas simples, $numero_alumnos no se expande.'

# Podemos cambiar el valor de una variable
numero_alumnos=20
echo "¡Corrección! Ahora hay $numero_alumnos alumnos."

# Variables especiales (predefinidas por el sistema)
echo "Este script se llama: $0"
echo "Estás ejecutando esto como el usuario: $USER"
echo "Tu directorio 'home' es: $HOME"
echo "El nombre de esta máquina es: $HOSTNAME"
```

* **Explicación:**
    * `variable="valor"`: Así se asigna un valor a una variable (sin espacios alrededor del `=`).
    * `$variable` o `${variable}`: Así se accede al valor de la variable. Las comillas dobles son importantes para que se expanda correctamente, especialmente si el valor contiene espacios.
    * Se muestran algunas variables predefinidas útiles.



**Paso 3: Interactuando con el Usuario - `read`**

* **Concepto:** Pedir información al usuario mientras el script se ejecuta y guardarla en una variable.
* **Script (`paso3_interaccion.sh`):**

```bash
#!/bin/bash

# Preguntar el nombre al usuario
echo "Por favor, introduce tu nombre:"
read nombre_usuario # Lee la entrada y la guarda en la variable nombre_usuario

# Preguntar la edad (con el prompt en la misma línea usando -p)
read -p "Ahora, dime tu edad: " edad_usuario

# Mostrar un mensaje personalizado
echo "¡Hola, $nombre_usuario! Es un placer conocerte."
echo "Así que tienes $edad_usuario años. ¡Interesante!"

# Pequeño cálculo (Bash básico sólo maneja enteros)
edad_proxima=$((edad_usuario + 1))
echo "El año que viene tendrás $edad_proxima años."

```

* **Explicación:**
    * `read variable`: Pausa el script, espera a que el usuario escriba algo y presione Enter, y guarda la entrada en `variable`.
    * `read -p "Prompt: " variable`: Muestra el texto "Prompt: " antes de esperar la entrada, sin necesidad de un `echo` previo.
    * `$((expresion))`: Sintaxis para realizar operaciones aritméticas con enteros en Bash.



**Paso 4: Tomando Decisiones - Condicionales `if`, `else`, `elif`**

* **Concepto:** Ejecutar diferentes bloques de código dependiendo de si una condición es verdadera o falsa.
* **Script (`paso4_condicionales.sh`):**

```bash
#!/bin/bash

read -p "Introduce un número entero: " numero

# Comprobación básica: ¿es mayor que 10?
echo " Comprobación simple "
if [ "$numero" -gt 10 ]; then
  echo "El número $numero es mayor que 10."
fi # Siempre se cierra un 'if' con 'fi' (if al revés)

# Comprobación con alternativa: ¿es positivo o no?
echo " Comprobación con else "
if [ "$numero" -ge 0 ]; then
  echo "El número $numero es positivo o cero."
else
  echo "El número $numero es negativo."
fi

# Comprobación con múltiples condiciones: ¿positivo, negativo o cero?
echo " Comprobación con elif "
if [ "$numero" -gt 0 ]; then
  echo "$numero es positivo."
elif [ "$numero" -lt 0 ]; then
  echo "$numero es negativo."
else
  echo "$numero es exactamente cero."
fi

# Comprobando texto
read -p "Escribe 'si' o 'no': " respuesta
respuesta=$(echo "$respuesta" | tr '[:upper:]' '[:lower:]') # Convertir a minúsculas

if [ "$respuesta" = "si" ]; then
  echo "Has elegido 'sí'."
elif [ "$respuesta" == "no" ]; then # == es un alias de = en bash dentro de [ ], más claro para comparar strings
  echo "Has elegido 'no'."
else
  echo "Respuesta no reconocida: '$respuesta'"
fi

# Comprobando si un archivo existe
archivo="paso1_saludo.sh"
echo " Comprobando archivo "
if [ -f "$archivo" ]; then
  echo "El archivo '$archivo' existe en el directorio actual."
else
  echo "El archivo '$archivo' NO existe aquí."
fi

```

* **Explicación:**
    * `if [ condicion ]; then ... fi`: Estructura básica. El espacio después de `[` y antes de `]` es **obligatorio**.
    * `[ "$variable" operador valor ]`: Formato de la condición. **¡Siempre pon las variables entre comillas dobles dentro de `[]`** para evitar errores si están vacías o contienen espacios!
    * Operadores numéricos comunes: `-eq` (igual), `-ne` (no igual), `-gt` (mayor que), `-ge` (mayor o igual), `-lt` (menor que), `-le` (menor o igual).
    * Operadores de cadena: `=` o `==` (igual), `!=` (no igual). `-z` (cadena vacía), `-n` (cadena no vacía).
    * Operadores de archivo: `-f` (es un archivo regular), `-d` (es un directorio), `-e` (existe, sea archivo o dir).
    * `else`: Bloque que se ejecuta si la condición `if` es falsa.
    * `elif`: Permite encadenar múltiples condiciones. Se ejecuta si el `if` anterior es falso, pero su propia condición es verdadera.
    * `$(comando)`: Sustitución de comando. Ejecuta el comando y reemplaza `$(comando)` por su salida. Útil para procesar texto como en la conversión a minúsculas con `tr`.



**Paso 5: Repitiendo Tareas - Bucles `for` y `while`**

* **Concepto:** Ejecutar un bloque de código múltiples veces. `for` se usa a menudo para iterar sobre una lista de elementos o un rango de números. `while` se usa cuando la repetición depende de que una condición siga siendo verdadera.
* **Script (`paso5_bucles.sh`):**

```bash
#!/bin/bash

#  Bucle FOR: Iterar sobre una lista de elementos 
echo " Bucle FOR con lista "
colores="rojo verde azul amarillo"
for color in $colores; do # Itera sobre cada palabra en la variable 'colores'
  echo "Color actual: $color"
done
echo # Línea en blanco para separar

#  Bucle FOR: Iterar sobre archivos 
echo " Bucle FOR con archivos (scripts .sh) "
for archivo_script in *.sh; do # El * es un comodín (wildcard)
  # Comprobamos si realmente es un archivo antes de imprimir
  if [ -f "$archivo_script" ]; then
    echo "Script encontrado: $archivo_script"
  fi
done
echo

#  Bucle FOR: Rango de números (estilo C) 
echo " Bucle FOR con rango numérico "
read -p "Contar hasta qué número (ej: 5): " limite
if [[ "$limite" =~ ^[0-9]+$ ]]; then # Usamos [[ ]] y =~ para expresiones regulares (validar que es número)
  for (( i=1; i<=$limite; i++ )); do
    echo "Contando: $i"
  done
else
    echo "'$limite' no parece ser un número entero positivo."
fi
echo

#  Bucle WHILE: Repetir mientras una condición sea cierta 
echo " Bucle WHILE "
contador=0
umbral=3
while [ "$contador" -lt "$umbral" ]; do
  echo "Dentro del while, contador = $contador (umbral = $umbral)"
  # ¡Importante! Modificar la variable de control dentro del bucle para evitar bucles infinitos
  contador=$((contador + 1))
done
echo "Bucle while terminado. Contador final: $contador"
echo

#  Bucle WHILE: Leer un archivo línea por línea 
# (Asegúrate de que tienes el archivo paso1_saludo.sh)
echo " WHILE leyendo archivo paso1_saludo.sh "
archivo_a_leer="paso1_saludo.sh"
if [ -f "$archivo_a_leer" ]; then
  numero_linea=1
  while IFS= read -r linea || [[ -n "$linea" ]]; do # Forma segura de leer líneas
      echo "Línea $numero_linea: $linea"
      numero_linea=$((numero_linea + 1))
  done < "$archivo_a_leer" # Redirige el contenido del archivo a la entrada del while
else
    echo "No se encontró el archivo '$archivo_a_leer' para leerlo."
fi

```

* **Explicación:**
    * `for variable in lista; do ... done`: Itera asignando cada elemento de la `lista` a `variable` en cada pasada.
    * `*.sh`: Comodín que representa todos los archivos que terminan en `.sh`.
    * `for (( expr1; expr2; expr3 )); do ... done`: Bucle estilo C, útil para rangos numéricos. `i++` incrementa `i`.
    * `[[ condicion ]]`: Una versión mejorada de `[ condicion ]`, especialmente útil con comparaciones de cadenas y expresiones regulares (`=~`).
    * `while [ condicion ]; do ... done`: Ejecuta el bloque `do...done` mientras la `condicion` sea verdadera (salga con código 0). Es crucial que algo dentro del bucle eventualmente haga que la condición sea falsa.
    * `while IFS= read -r linea ... < archivo`: Es la forma canónica y segura de leer un archivo línea por línea en Bash, preservando espacios y caracteres especiales. `IFS=` evita que se recorten espacios al inicio/final, `read -r` evita que se interpreten las barras invertidas. `< "$archivo_a_leer"` redirige el contenido del archivo a la entrada estándar del bucle `while`.



**Paso 6: Organizando el Código - Funciones**

* **Concepto:** Agrupar bloques de código reutilizables bajo un nombre. Mejora la legibilidad, organización y evita repetir código. Las funciones pueden aceptar argumentos (parámetros).
* **Script (`paso6_funciones.sh`):**

```bash
#!/bin/bash

#  Definición de Funciones 

# Función simple sin argumentos
mostrar_bienvenida() {
  echo "=============================="
  echo "   Bienvenido a Mi Script    "
  echo "=============================="
  echo # Línea en blanco
}

# Función que acepta argumentos (parámetros)
saludar_usuario() {
  local nombre="$1" # $1 es el primer argumento pasado a la función. 'local' hace la variable visible sólo dentro de la función
  local edad="$2"   # $2 es el segundo argumento

  if [ -z "$nombre" ]; then # -z comprueba si la cadena está vacía
    nombre="Usuario Desconocido" # Valor por defecto si no se pasa nombre
  fi

  echo "Hola, $nombre."

  # Usamos la edad si se proporcionó
  if [ -n "$edad" ]; then # -n comprueba si la cadena NO está vacía
     if [[ "$edad" =~ ^[0-9]+$ ]] && [ "$edad" -gt 0 ]; then # Validamos que sea un número positivo
        echo "Tienes $edad años."
        if [ "$edad" -ge 18 ]; then
          echo "Eres mayor de edad."
        else
          echo "Eres menor de edad."
        fi
     else
        echo "'$edad' no parece ser una edad válida."
     fi
  fi
  echo # Línea en blanco
}

# Función que simula devolver un valor (imprimiéndolo)
obtener_fecha_hora() {
  date "+%Y-%m-%d %H:%M:%S" # El comando 'date' con formato
}


#  Programa Principal (Llamadas a las funciones) 

mostrar_bienvenida # Llama a la primera función

# Llamamos a saludar_usuario con argumentos
saludar_usuario "Ana" "25"
saludar_usuario "Luis" # Sin edad
saludar_usuario # Sin argumentos

# Capturamos la "salida" de la función obtener_fecha_hora en una variable
fecha_actual=$(obtener_fecha_hora)
echo "La fecha y hora actual según la función es: $fecha_actual"

echo "Script finalizado."
```

* **Explicación:**
    * `nombre_funcion() { ... código ... }` o `function nombre_funcion { ... código ... }`: Formas de definir una función.
    * Para llamar a una función, simplemente escribe su nombre: `nombre_funcion`.
    * Dentro de una función, `$1`, `$2`, `$3`, etc., representan los argumentos pasados al llamarla. `$0` sigue siendo el nombre del script, no de la función. `$#` dentro de la función es el número de argumentos pasados *a la función*. `$@` o `$*` representan todos los argumentos pasados a la función.
    * `local variable="valor"`: Declara una variable que sólo existe dentro de la función actual. Es una **muy buena práctica** usar `local` para evitar modificar accidentalmente variables globales.
    * Bash no tiene un `return` como otros lenguajes para devolver datos complejos directamente. Hay dos formas comunes de "devolver" valores:
        1.  Hacer que la función imprima el resultado con `echo` y capturar esa salida con `variable=$(nombre_funcion arg1 arg2)`. (Como en `obtener_fecha_hora`).
        2.  Hacer que la función establezca una variable global (menos recomendado).
        3.  Usar el código de salida (`exit N` o `return N` dentro de la función, donde N es 0-255) para indicar éxito (0) o tipos específicos de error (1-255). Se comprueba con `$?` después de llamar a la función.

