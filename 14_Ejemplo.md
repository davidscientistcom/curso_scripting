**Paso 1: Estructura del Menú Principal**

Vamos a crear un script (`backup_app.sh`) con un menú básico que permita al usuario seleccionar las diferentes opciones de backup.

```bash
#!/bin/bash

mostrar_menu() {
  echo "================================"
  echo "  Aplicación de Backup Simple  "
  echo "================================"
  echo "1. Backup Simple (todo)"
  echo "2. Backup Avanzado (por tipo de archivo)"
  echo "3. Backup Filtrado (por tipo y tamaño)"
  echo "4. Salir"
  echo ""
  read -p "Selecciona una opción: " opcion
}

case_menu() {
  case "$opcion" in
    1)
      echo "Has seleccionado: Backup Simple"
      backup_simple
      ;;
    2)
      echo "Has seleccionado: Backup Avanzado"
      backup_avanzado
      ;;
    3)
      echo "Has seleccionado: Backup Filtrado"
      backup_filtrado
      ;;
    4)
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
  mostrar_menu
  case_menu
  echo "" # Separador después de cada operación
done
```

**Explicación:**

* `mostrar_menu()`: Función que muestra las opciones del menú al usuario.
* `case_menu()`: Función que utiliza una estructura `case` para ejecutar diferentes acciones según la opción seleccionada por el usuario.
* Bucle `while true`: Mantiene el menú mostrándose hasta que el usuario elige la opción de salir (opción 4).
* Por ahora, las funciones `backup_simple`, `backup_avanzado` y `backup_filtrado` están declaradas pero vacías. Las implementaremos en los siguientes pasos.

Guarda este script como `backup_app.sh` y hazlo ejecutable con `chmod +x backup_app.sh`. Si lo ejecutas ahora, verás el menú.

**Paso 2: Implementando el Backup Simple**

Ahora vamos a implementar la función `backup_simple`. Utilizaremos el comando `cp` con la opción `-r` para copiar directorios recursivamente.

```bash
#!/bin/bash

mostrar_menu() {
  # ... (código del menú del Paso 1) ...
}

case_menu() {
  # ... (código del case del menú del Paso 1) ...
}

backup_simple() {
  echo ""
  echo "--- Backup Simple ---"
  read -p "Introduce la ruta de origen: " origen
  read -p "Introduce la ruta de destino: " destino

  if [ -z "$origen" ] || [ -z "$destino" ]; then
    echo "Error: Debes especificar tanto la ruta de origen como la de destino."
    return 1 # Indica que la función terminó con un error
  fi

  echo "Realizando backup de '$origen' a '$destino'..."
  cp -r "$origen" "$destino"

  if [ $? -eq 0 ]; then
    echo "Backup completado con éxito."
  else
    echo "Error durante el backup."
  fi
}

backup_avanzado() {
  echo "Función de Backup Avanzado aún no implementada."
}

backup_filtrado() {
  echo "Función de Backup Filtrado aún no implementada."
}

# Bucle principal del menú
while true; do
  mostrar_menu
  case_menu
  echo ""
done
```

**Explicación de `backup_simple()`:**

* Se pide al usuario que introduzca las rutas de origen y destino.
* Se verifica si ambas rutas han sido proporcionadas. Si no, se muestra un error y la función termina.
* Se utiliza `cp -r "$origen" "$destino"` para copiar el contenido de la ruta de origen a la ruta de destino. La opción `-r` es crucial para copiar directorios y su contenido de forma recursiva. Es importante usar comillas dobles alrededor de las variables para manejar correctamente rutas con espacios.
* Se comprueba el código de salida del comando `cp` usando `$?`. Un código de salida de 0 generalmente indica éxito.

Ahora, si ejecutas el script y seleccionas la opción 1, te pedirá las rutas y realizará la copia. ¡Prueba con algunas carpetas de prueba!

**Paso 3: Implementando el Backup Avanzado (por tipo de archivo)**

Ahora vamos a añadir la funcionalidad para hacer un backup solo de ciertos tipos de archivos. Utilizaremos el comando `find` para buscar los archivos y luego `cp` para copiarlos.

```bash
#!/bin/bash

mostrar_menu() {
  # ... (código del menú) ...
}

case_menu() {
  # ... (código del case) ...
}

backup_simple() {
  # ... (código de backup_simple) ...
}

backup_avanzado() {
  echo ""
  echo "--- Backup Avanzado (por tipo de archivo) ---"
  read -p "Introduce la ruta de origen: " origen
  read -p "Introduce la ruta de destino: " destino
  read -p "Introduce la extensión del archivo a respaldar (ej: .txt): " extension

  if [ -z "$origen" ] || [ -z "$destino" ] || [ -z "$extension" ]; then
    echo "Error: Debes especificar origen, destino y extensión del archivo."
    return 1
  fi

  echo "Buscando archivos con extensión '$extension' en '$origen' y respaldando a '$destino'..."

  # Aseguramos que la extensión comience con un punto si no lo tiene
  if [[ ! "$extension" =~ ^\. ]]; then
    extension=".$extension"
  fi

  find "$origen" -type f -name "*$extension" -exec cp {} "$destino" \;

  if [ $? -eq 0 ]; then
    echo "Backup avanzado completado con éxito."
  else
    echo "Puede que hayan ocurrido errores durante el backup avanzado."
  fi
}

backup_filtrado() {
  echo "Función de Backup Filtrado aún no implementada."
}

# Bucle principal del menú
while true; do
  mostrar_menu
  case_menu
  echo ""
done
```

**Explicación de `backup_avanzado()`:**

* Se pide al usuario la ruta de origen, la ruta de destino y la extensión del archivo que quiere respaldar (por ejemplo, `.txt`, `.jpg`).
* Se verifica que se hayan proporcionado todos los datos necesarios.
* Se utiliza el comando `find`:
    * `"$origen"`: Especifica el directorio donde buscar.
    * `-type f`: Busca solo archivos regulares.
    * `-name "*$extension"`: Busca archivos cuyo nombre termine con la extensión especificada. El `*` es un comodín.
    * `-exec cp {} "$destino" \;`: Por cada archivo encontrado (`{}`), ejecuta el comando `cp` para copiarlo al directorio de destino. El `\;` marca el final del comando a ejecutar por `find`.
* Se verifica el código de salida de `find`.

Ahora, si seleccionas la opción 2, podrás especificar un tipo de archivo para hacer el backup.

**Paso 4: Implementando el Backup Filtrado (por tipo y tamaño)**

Finalmente, vamos a implementar la función de backup filtrado por tipo de archivo y tamaño máximo. También utilizaremos `find` para esto.

```bash
#!/bin/bash

mostrar_menu() {
  # ... (código del menú) ...
}

case_menu() {
  # ... (código del case) ...
}

backup_simple() {
  # ... (código de backup_simple) ...
}

backup_avanzado() {
  # ... (código de backup_avanzado) ...
}

backup_filtrado() {
  echo ""
  echo "--- Backup Filtrado (por tipo y tamaño) ---"
  read -p "Introduce la ruta de origen: " origen
  read -p "Introduce la ruta de destino: " destino
  read -p "Introduce la extensión del archivo a respaldar (ej: .log): " extension
  read -p "Introduce el tamaño máximo del archivo (ej: 1M, 500k): " tamano_maximo

  if [ -z "$origen" ] || [ -z "$destino" ] || [ -z "$extension" ] || [ -z "$tamano_maximo" ]; then
    echo "Error: Debes especificar origen, destino, extensión y tamaño máximo."
    return 1
  fi

  echo "Buscando archivos con extensión '$extension' y tamaño máximo '$tamano_maximo' en '$origen' y respaldando a '$destino'..."

  # Aseguramos que la extensión comience con un punto si no lo tiene
  if [[ ! "$extension" =~ ^\. ]]; then
    extension=".$extension"
  fi

  find "$origen" -type f -name "*$extension" -size "-$tamano_maximo" -exec cp {} "$destino" \;

  if [ $? -eq 0 ]; then
    echo "Backup filtrado completado con éxito."
  else
    echo "Puede que hayan ocurrido errores durante el backup filtrado."
  fi
}

# Bucle principal del menú
while true; do
  mostrar_menu
  case_menu
  echo ""
done
```

**Explicación de `backup_filtrado()`:**

* Se pide al usuario la ruta de origen, la ruta de destino, la extensión del archivo y el tamaño máximo.
* Se verifica que se hayan proporcionado todos los datos.
* Se utiliza el comando `find` con una opción adicional:
    * `-size "-$tamano_maximo"`: Busca archivos cuyo tamaño sea menor o igual al especificado en `tamano_maximo`. `find` entiende diferentes unidades de tamaño como `k` (kilobytes), `M` (megabytes), `G` (gigabytes), etc.

Ahora, si seleccionas la opción 3, podrás filtrar los backups por tipo de archivo y tamaño máximo.

**Consideraciones Adicionales y Mejoras:**

* **Manejo de rutas relativas y absolutas:** El comando `cp` y `find` funcionan tanto con rutas relativas como absolutas, por lo que no necesitamos hacer nada especial para manejarlas. El usuario simplemente introduce la ruta tal cual.
* **Elegancia en la falta de ruta:** Ya hemos implementado la verificación de que se proporcionen las rutas necesarias en cada función. Si no se proporcionan, se muestra un mensaje de error y la función termina.
* **Herramienta de backup:** Hemos utilizado `cp` para el backup simple y `find` combinado con `cp` para los backups más avanzados. Para backups más robustos y con funcionalidades como compresión, backups incrementales, etc., se podrían considerar herramientas como `tar`, `rsync` o `duplicity`. Sin embargo, para este ejercicio de aprendizaje, `cp` y `find` son suficientes para ilustrar los conceptos.
* **Información al usuario:** Hemos añadido mensajes para informar al usuario sobre el progreso y el resultado de cada operación de backup.
* **Validación de entrada:** Podríamos añadir validaciones más robustas para asegurarnos de que el usuario introduce datos válidos (por ejemplo, verificar que el tamaño máximo sea un número seguido de una unidad válida).
* **Creación del directorio de destino:** Actualmente, si el directorio de destino no existe, el comando `cp` puede comportarse de manera inesperada dependiendo de si se está copiando un archivo o un directorio. Sería buena práctica verificar si el directorio de destino existe y crearlo si no. Esto se puede hacer con `mkdir -p "$destino"`.
* **Mensajes más detallados:** Para backups más complejos, podríamos mostrar cuántos archivos se han copiado, si ha habido errores específicos, etc.
