**Paso 1: Estructura del Menú de Información de Usuarios**

Crearemos un script (`info_usuarios.sh`) con un menú que permita al usuario seleccionar qué información quiere ver sobre los usuarios del sistema.

```bash
#!/bin/bash

mostrar_menu_usuarios() {
  echo "====================================="
  echo "  Información de Usuarios del Sistema  "
  echo "====================================="
  echo "1. Listar todos los usuarios (nombre de usuario)"
  echo "2. Mostrar detalles de un usuario (nombre, UID, GID)"
  echo "3. Listar usuarios con una shell específica"
  echo "4. Mostrar el directorio home de todos los usuarios"
  echo "5. Salir"
  echo ""
  read -p "Selecciona una opción: " opcion
}

case_menu_usuarios() {
  case "$opcion" in
    1)
      echo "Has seleccionado: Listar todos los usuarios"
      listar_usuarios
      ;;
    2)
      echo "Has seleccionado: Mostrar detalles de un usuario"
      mostrar_detalles_usuario
      ;;
    3)
      echo "Has seleccionado: Listar usuarios con una shell específica"
      listar_usuarios_por_shell
      ;;
    4)
      echo "Has seleccionado: Mostrar el directorio home de todos los usuarios"
      listar_directorios_home
      ;;
    5)
      echo "Saliendo de la aplicación."
      exit 0
      ;;
    *)
      echo "Opción inválida. Por favor, intenta de nuevo."
      ;;
  esac
}

# Bucle principal del menú
while true; do
  mostrar_menu_usuarios
  case_menu_usuarios
  echo ""
done
```

**Paso 2: Implementando la Opción 1: Listar todos los usuarios**

La información de los usuarios del sistema se encuentra principalmente en el archivo `/etc/passwd`. Cada línea de este archivo contiene información sobre un usuario, con los campos separados por dos puntos (`:`). El primer campo es el nombre de usuario.

```bash
#!/bin/bash

mostrar_menu_usuarios() {
  # ... (código del menú del Paso 1) ...
}

case_menu_usuarios() {
  # ... (código del case del menú del Paso 1) ...
}

listar_usuarios() {
  echo ""
  echo "--- Listado de todos los usuarios ---"
  awk -F':' '{ print $1 }' /etc/passwd
  echo ""
}

mostrar_detalles_usuario() {
  echo "Opción aún no implementada."
}

listar_usuarios_por_shell() {
  echo "Opción aún no implementada."
}

listar_directorios_home() {
  echo "Opción aún no implementada."
}

# Bucle principal del menú
while true; do
  mostrar_menu_usuarios
  case_menu_usuarios
  echo ""
done
```

**Explicación de `listar_usuarios()`:**

* `awk -F':' '{ print $1 }' /etc/passwd`: Este es el corazón de la función.
    * `awk`: Invoca el comando `awk`.
    * `-F':'`: Especifica que el delimitador de campos es el carácter dos puntos (`:`). Esto es importante porque los campos en `/etc/passwd` están separados por `:`.
    * `{ print $1 }`: Este es el bloque de acción de `awk`. Se ejecuta para cada línea del archivo de entrada. `$1` representa el primer campo de la línea (el nombre de usuario). `print $1` imprime este primer campo.
    * `/etc/passwd`: Este es el archivo de entrada que `awk` procesa.

Ejecuta el script y selecciona la opción 1. Deberías ver una lista de todos los nombres de usuario del sistema.

**Paso 3: Implementando la Opción 2: Mostrar detalles de un usuario**

Ahora permitiremos al usuario introducir un nombre de usuario y mostraremos su nombre, UID (User ID) y GID (Group ID). Estos campos son el primero, tercero y cuarto respectivamente en `/etc/passwd`.

```bash
#!/bin/bash

mostrar_menu_usuarios() {
  # ... (código del menú) ...
}

case_menu_usuarios() {
  # ... (código del case) ...
}

listar_usuarios() {
  # ... (código de listar_usuarios) ...
}

mostrar_detalles_usuario() {
  echo ""
  echo "--- Detalles de un usuario ---"
  read -p "Introduce el nombre de usuario: " usuario_a_buscar

  if [ -z "$usuario_a_buscar" ]; then
    echo "Error: Debes introducir un nombre de usuario."
    return 1
  fi

  awk -F':' -v usuario="$usuario_a_buscar" '$1 == usuario { print "Nombre de usuario: " $1 ", UID: " $3 ", GID: " $4 }' /etc/passwd

  if [ $? -ne 0 ]; then
    echo "No se encontró el usuario '$usuario_a_buscar'."
  fi
  echo ""
}

listar_usuarios_por_shell() {
  echo "Opción aún no implementada."
}

listar_directorios_home() {
  echo "Opción aún no implementada."
}

# Bucle principal del menú
while true; do
  mostrar_menu_usuarios
  case_menu_usuarios
  echo ""
done
```

**Explicación de `mostrar_detalles_usuario()`:**

* Se pide al usuario que introduzca el nombre de usuario.
* `awk -F':' -v usuario="$usuario_a_buscar" '$1 == usuario { print "Nombre de usuario: " $1 ", UID: " $3 ", GID: " $4 }' /etc/passwd`:
    * `-v usuario="$usuario_a_buscar"`: Permite pasar la variable de Bash `usuario_a_buscar` a una variable de `awk` llamada `usuario`.
    * `$1 == usuario`: Esta es la condición. Comprueba si el primer campo (`$1`, el nombre de usuario en el archivo) es igual al valor de la variable `usuario` que pasamos.
    * `{ print "Nombre de usuario: " $1 ", UID: " $3 ", GID: " $4 }`: Si la condición es verdadera, se ejecuta este bloque. Imprime el nombre de usuario (`$1`), el UID (`$3`, el tercer campo) y el GID (`$4`, el cuarto campo) con etiquetas descriptivas.

Selecciona la opción 2 y prueba con algún nombre de usuario de tu sistema (por ejemplo, tu propio nombre de usuario).

**Paso 4: Implementando la Opción 3: Listar usuarios con una shell específica**

El séptimo campo en `/etc/passwd` contiene la shell que utiliza el usuario. Permitiremos al usuario introducir una shell y listaremos todos los usuarios que la utilizan.

```bash
#!/bin/bash

mostrar_menu_usuarios() {
  # ... (código del menú) ...
}

case_menu_usuarios() {
  # ... (código del case) ...
}

listar_usuarios() {
  # ... (código de listar_usuarios) ...
}

mostrar_detalles_usuario() {
  # ... (código de mostrar_detalles_usuario) ...
}

listar_usuarios_por_shell() {
  echo ""
  echo "--- Usuarios con una shell específica ---"
  read -p "Introduce la shell a buscar (ej: /bin/bash): " shell_a_buscar

  if [ -z "$shell_a_buscar" ]; then
    echo "Error: Debes introducir una shell."
    return 1
  fi

  awk -F':' -v shell="$shell_a_buscar" '$7 == shell { print $1 }' /etc/passwd
  echo ""
}

listar_directorios_home() {
  echo "Opción aún no implementada."
}

# Bucle principal del menú
while true; do
  mostrar_menu_usuarios
  case_menu_usuarios
  echo ""
done
```

**Explicación de `listar_usuarios_por_shell()`:**

* Se pide al usuario que introduzca la ruta de la shell.
* `awk -F':' -v shell="$shell_a_buscar" '$7 == shell { print $1 }' /etc/passwd`:
    * `$7 == shell`: Comprueba si el séptimo campo (`$7`, la shell del usuario) es igual a la shell introducida por el usuario.
    * Si coincide, se imprime el nombre de usuario (`$1`).

Selecciona la opción 3 e introduce una shell común como `/bin/bash` o `/bin/sh`. Deberías ver la lista de usuarios que utilizan esa shell.

**Paso 5: Implementando la Opción 4: Mostrar el directorio home de todos los usuarios**

El sexto campo en `/etc/passwd` contiene el directorio home del usuario.

```bash
#!/bin/bash

mostrar_menu_usuarios() {
  # ... (código del menú) ...
}

case_menu_usuarios() {
  # ... (código del case) ...
}

listar_usuarios() {
  # ... (código de listar_usuarios) ...
}

mostrar_detalles_usuario() {
  # ... (código de mostrar_detalles_usuario) ...
}

listar_usuarios_por_shell() {
  # ... (código de listar_usuarios_por_shell) ...
}

listar_directorios_home() {
  echo ""
  echo "--- Directorios home de todos los usuarios ---"
  awk -F':' '{ print $1 " -> " $6 }' /etc/passwd
  echo ""
}

# Bucle principal del menú
while true; do
  mostrar_menu_usuarios
  case_menu_usuarios
  echo ""
done
```

**Explicación de `listar_directorios_home()`:**

* `awk -F':' '{ print $1 " -> " $6 }' /etc/passwd`:
    * Se imprime el primer campo (`$1`, nombre de usuario), seguido de la cadena " -> ", y luego el sexto campo (`$6`, el directorio home).

Selecciona la opción 4 para ver el directorio home de cada usuario del sistema.

**Conclusión y Posibles Mejoras:**

Este script proporciona un ejemplo práctico de cómo utilizar `awk` para procesar archivos de texto estructurados como `/etc/passwd`. Los estudiantes pueden ver cómo se especifica el delimitador de campos, cómo se accede a los diferentes campos y cómo se pueden realizar condiciones para filtrar la información.

**Posibles mejoras para este script:**

* **Validación de entrada:** Podríamos añadir validación para asegurarnos de que el usuario introduce una opción válida del menú.
* **Manejo de errores:** Podríamos añadir comprobaciones para ver si el archivo `/etc/passwd` existe y es legible.
* **Formato de salida más elaborado:** Se podría utilizar `printf` en `awk` para formatear la salida de manera más precisa.
* **Opción para buscar usuarios por UID o GID:** Se podría añadir una opción para buscar usuarios basándose en su ID numérico.
* **Leer información de otros archivos:** Se podría extender para leer información de otros archivos relacionados con usuarios y grupos, como `/etc/group`.