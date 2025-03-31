**Práctica: Analizador de Registros Web (Acceso Básico)**

**Concepto:** El objetivo es crear un script que simule un análisis básico de un archivo de registro de un servidor web (en formato común). El script permitirá a los usuarios obtener información útil sobre las peticiones realizadas al servidor, utilizando `awk` para el procesamiento.

**Pasos para los estudiantes:**

1.  **Crear un archivo de registro de ejemplo:**
    * Crear un archivo de texto llamado `acceso.log` con algunas líneas que simulen entradas de un registro web. Cada línea podría tener el siguiente formato (simplificado):

        ```
        192.168.1.10 - - [20/Mar/2025:10:00:00 +0000] "GET /index.html HTTP/1.1" 200 1234
        192.168.1.15 - - [20/Mar/2025:10:05:30 +0000] "GET /images/logo.png HTTP/1.1" 200 56789
        192.168.1.10 - - [20/Mar/2025:10:10:15 +0000] "GET /about.html HTTP/1.1" 304 -
        203.0.113.45 - - [20/Mar/2025:10:12:00 +0000] "POST /submit.php HTTP/1.1" 200 55
        192.168.1.15 - - [20/Mar/2025:10:18:45 +0000] "GET /style.css HTTP/1.1" 200 987
        ```

    * **Explicación del formato :**
        * `IP del cliente`
        * `- -` (campos de identidad y usuario, a menudo vacíos)
        * `[Fecha y hora de la petición]`
        * `"Método HTTP Petición Protocolo"`
        * `Código de estado HTTP`
        * `Tamaño de la respuesta en bytes`

2.  **Crear el script `analizador_logs.sh`:**

3.  **Implementar un menú:** El script debe mostrar un menú con las siguientes opciones:

    ```
    =====================================
      Analizador Básico de Registros Web
    =====================================
    1. Listar todas las IPs que accedieron al sitio.
    2. Mostrar la cantidad total de peticiones.
    3. Mostrar la cantidad de peticiones por código de estado (ej: 200, 304, 404).
    4. Listar las peticiones al archivo 'index.html'.
    5. Salir.
    Selecciona una opción:
    ```

4.  **Implementar la funcionalidad de cada opción utilizando `awk`:**

    * **Opción 1: Listar todas las IPs:**
        * Utilizar `awk` para imprimir el primer campo de cada línea del archivo `acceso.log`.
        * **Pista:** `awk '{ print $1 }' acceso.log`

    * **Opción 2: Mostrar la cantidad total de peticiones:**
        * Utilizar `awk` para contar el número de líneas en el archivo `acceso.log`.
        * **Pista:** `awk 'END { print NR }' acceso.log` (NR es el número de registro/línea)

    * **Opción 3: Mostrar la cantidad de peticiones por código de estado:**
        * Utilizar `awk` para contar las ocurrencias de cada código de estado (que es el penúltimo campo).
        * **Pista:** Puedes usar un array asociativo en `awk` para almacenar los conteos.
            ```awk
            '{ estados[$9]++ }
            END { for (codigo in estados) { print codigo ": " estados[codigo] } }' acceso.log
            ```

    * **Opción 4: Listar las peticiones al archivo 'index.html':**
        * Utilizar `awk` para encontrar las líneas donde el tercer campo (dentro de las comillas dobles) contenga `/index.html`.
        * **Pista:** Puedes usar el operador `~` para buscar una expresión regular o simplemente comparar la cadena. Recuerda que el tercer campo está entre comillas dobles, así que tendrás que trabajar con eso. Una forma sería extraer el tercer campo y luego buscar dentro.
            ```awk
            -F'"' '{ if ($2 ~ "/index.html") print $0 }' acceso.log
            ```
            *(Nota: Aquí cambiamos el delimitador a la comilla doble para facilitar la extracción del segundo campo, que contiene la petición).*

5.  **Estructura del script:** Utilizar un bucle `while` y una estructura `case` para manejar el menú y las opciones del usuario, similar a los ejemplos anteriores.

**Ejemplo de estructura del script `analizador_logs.sh`:**

```bash
#!/bin/bash

mostrar_menu() {
  # ... (mostrar el menú) ...
}

case_menu() {
  case "$opcion" in
    1)
      echo "--- IPs de acceso ---"
      awk '{ print $1 }' acceso.log
      ;;
    2)
      echo "--- Cantidad total de peticiones ---"
      awk 'END { print NR }' acceso.log
      ;;
    3)
      echo "--- Peticiones por código de estado ---"
      awk '{ estados[$9]++ } END { for (codigo in estados) { print codigo ": " estados[codigo] } }' acceso.log
      ;;
    4)
      echo "--- Peticiones a index.html ---"
      awk -F'"' '{ if ($2 ~ "/index.html") print $0 }' acceso.log
      ;;
    5)
      echo "Saliendo."
      exit 0
      ;;
    *)
      echo "Opción inválida."
      ;;
  esac
}

# Bucle principal
while true; do
  mostrar_menu
  read -p "Selecciona una opción: " opcion
  case_menu
  echo ""
done
```
