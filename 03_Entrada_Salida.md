1º**Módulo 1: Fundamentos del Scripting en Sistemas**

### **Tema 3: Entrada y Salida Estándar en Bash, PowerShell y Python**

---

### **3.1 Mostrar Información en Pantalla**
Los scripts necesitan imprimir información en la terminal. Cada lenguaje tiene su propia forma de hacerlo:

| Acción | Bash | PowerShell | Python |
|---------|------|-----------|--------|
| Imprimir texto simple | `echo "Hola"` | `Write-Output "Hola"` | `print("Hola")` |
| Imprimir variable | `echo "Hola $var"` | `Write-Output "Hola $var"` | `print(f"Hola {var}")` |
| Imprimir con salto de línea | `echo -e "Hola\nMundo"` | `"Hola\nMundo"` | `print("Hola\nMundo")` |

**Ejemplo: Mostrar información del sistema**
Cada script imprimirá el usuario actual, el nombre del sistema y la fecha.

**Bash**
```bash
#!/bin/bash
echo "Usuario: $(whoami)"
echo "Sistema: $(uname -o)"
echo "Fecha: $(date)"
```

**PowerShell**
```powershell
Write-Output "Usuario: $env:USERNAME"
Write-Output "Sistema: (Get-CimInstance Win32_OperatingSystem).Caption"
Write-Output "Fecha: $(Get-Date)"
```

**Python**
```python
import os
import platform
import datetime

print(f"Usuario: {os.getlogin()}")
print(f"Sistema: {platform.system()} {platform.release()}")
print(f"Fecha: {datetime.datetime.now()}")
```

---

### **3.2 Leer Datos del Usuario**
Solicitar datos al usuario es fundamental para la interacción con scripts.

| Lenguaje | Código |
|----------|--------|
| Bash | `read -p "Introduce tu nombre: " nombre` |
| PowerShell | `$nombre = Read-Host "Introduce tu nombre"` |
| Python | `nombre = input("Introduce tu nombre: ")` |

**Ejemplo: Pedir el nombre y mostrar un mensaje personalizado.**

**Bash**
```bash
#!/bin/bash
read -p "Introduce tu nombre: " nombre
echo "Hola, $nombre"
```

**PowerShell**
```powershell
$nombre = Read-Host "Introduce tu nombre"
Write-Output "Hola, $nombre"
```

**Python**
```python
nombre = input("Introduce tu nombre: ")
print(f"Hola, {nombre}")
```

---

### **3.3 Entrada y Salida con Redirección**
Podemos redirigir la salida de comandos o scripts a archivos.
A continuación tienes una lista (en lugar de tabla) con las principales redirecciones y ejemplos en **Bash**, **PowerShell** y **Python**. Se incluye redirección y añadido tanto de la salida estándar como de la salida de error, la combinación de ambas salidas y la redirección de entrada.



#### 1. Redirigir salida estándar (`>`)

- **Bash**  
  ```bash
  echo "Hello" > file.txt
  ```
  Esto crea (o sobrescribe) `file.txt` con el contenido `"Hello"`.

- **PowerShell**  
  ```powershell
  "Hello" | Out-File file.txt
  ```
  Envía la cadena `"Hello"` al archivo `file.txt` (sobrescribiéndolo si existe).

- **Python**  
  ```python
  with open("file.txt", "w") as f:
      f.write("Hello\n")
  ```
  Abre (o crea) el archivo `file.txt` en modo escritura (`"w"`) y escribe `"Hello"`.



#### 2. Añadir (append) a la salida estándar (`>>`)

- **Bash**  
  ```bash
  echo "Hello" >> file.txt
  ```
  Añade `"Hello"` al final de `file.txt` sin borrar lo que ya contenga.

- **PowerShell**  
  ```powershell
  "Hello" | Out-File file.txt -Append
  ```
  Añade la cadena `"Hello"` al final de `file.txt`.

- **Python**  
  ```python
  with open("file.txt", "a") as f:
      f.write("Hello\n")
  ```
  Abre el archivo en modo adjuntar (`"a"`) y escribe `"Hello"` al final.



#### 3. Redirigir error estándar (`2>`)

- **Bash**  
  ```bash
  ls archivo_que_no_existe 2> error.txt
  ```
  Si `archivo_que_no_existe` no existe, el mensaje de error se redirige a `error.txt`.

- **PowerShell**  
  ```powershell
  Get-Item archivo_que_no_existe 2> error.txt
  ```
  Redirige el error (Stream 2) a `error.txt`.

- **Python**  
  ```python
  import sys

  try:
      with open("archivo_que_no_existe.txt", "r") as f:
          data = f.read()
  except Exception as e:
      with open("error.txt", "w") as f:
          # Redirigimos el error a un archivo
          f.write(str(e) + "\n")
  ```
  En Python, normalmente capturas la excepción y escribes el error manualmente en el archivo.



#### 4. Añadir (append) error estándar (`2>>`)

- **Bash**  
  ```bash
  ls archivo_que_no_existe 2>> error.txt
  ```
  Añade el error al final de `error.txt` sin sobrescribirlo.

- **PowerShell**  
  ```powershell
  Get-Item archivo_que_no_existe 2>> error.txt
  ```
  Igual que en Bash, pero en PowerShell se usa la misma sintaxis para el stream 2.

- **Python**  
  ```python
  import sys

  try:
      with open("archivo_que_no_existe.txt", "r") as f:
          data = f.read()
  except Exception as e:
      with open("error.txt", "a") as f:
          f.write(str(e) + "\n")
  ```
  Abre el archivo en modo “append” (`"a"`) y añade el texto del error.



#### 5. Redirigir **ambas** salidas (estándar y error)

- **Bash**  
  ```bash
  comando > todo.txt 2>&1
  ```
  Primero rediriges la salida estándar a `todo.txt`, luego rediriges la salida de error al mismo destino de la salida estándar.

- **PowerShell**  
  ```powershell
  comando > todo.txt 2>&1
  ```
  También funciona igual que en Bash.  
  > *En versiones más recientes de PowerShell (>=5) existe `*> todo.txt` que redirige todas las corrientes (salida, error, verbose, debug…).*

- **Python**  
  ```python
  import sys
  from contextlib import redirect_stdout, redirect_stderr

  with open("todo.txt", "w") as f:
      with redirect_stdout(f), redirect_stderr(f):
          print("Esto va a la salida estándar.")
          print("Esto es un error.", file=sys.stderr)
  ```
  Se redirigen ambas corrientes (stdout y stderr) al mismo archivo.


#### 6. Redirigir entrada estándar (`<`)

- **Bash**  
  ```bash
  sort < file.txt
  ```
  Toma el contenido de `file.txt` como entrada de `sort`.

- **PowerShell**  
  ```powershell
  Get-Content file.txt | Sort-Object
  ```
  PowerShell no usa `<` para redirigir la entrada; en su lugar se usa un **pipeline** con `Get-Content`.

- **Python**  
  ```python
  with open("file.txt", "r") as f:
      data = f.read()
  # Luego podrías procesar 'data', por ejemplo:
  print(sorted(data.split()))
  ```
  Se abre el archivo y se lee su contenido en la variable `data`.



### Ojo!
- En **Bash**, los números de file descriptor (`0` para entrada, `1` para salida estándar y `2` para error estándar) se utilizan con los operadores `>`, `>>`, `2>`, `2>>`, etc.  
- En **PowerShell**, a partir de la versión 3.0 se maneja la salida por “streams”: `1>` para salida, `2>` para error, `3>` para información, etc.  
- En **Python**, no existe un operador de redirección automático (como en Bash/PowerShell). Generalmente se hace abriendo un archivo y escribiendo en él. Para la salida estándar y de error, puedes usar la librería `sys` y/o `contextlib.redirect_stdout` / `contextlib.redirect_stderr`.

**Ejemplo: Guardar información del sistema en un archivo.**

**Bash**
```bash
#!/bin/bash
echo "Usuario: $(whoami)" > info.txt
echo "Sistema: $(uname -o)" >> info.txt
echo "Fecha: $(date)" >> info.txt
```

**PowerShell**
```powershell
"Usuario: $env:USERNAME" | Out-File info.txt
"Sistema: (Get-CimInstance Win32_OperatingSystem).Caption" | Add-Content info.txt
"Fecha: $(Get-Date)" | Add-Content info.txt
```

**Python**
```python
import os, platform, datetime

with open("info.txt", "w") as f:
    f.write(f"Usuario: {os.getlogin()}\n")
    f.write(f"Sistema: {platform.system()} {platform.release()}\n")
    f.write(f"Fecha: {datetime.datetime.now()}\n")
```

---

### **3.4 Leer un Archivo**
| Lenguaje | Código |
|----------|--------|
| Bash | `cat archivo.txt` |
| PowerShell | `Get-Content archivo.txt` |
| Python | `with open("archivo.txt", "r") as f: print(f.read())` |

**Ejemplo: Leer e imprimir el contenido de un archivo.**

**Bash**
```bash
#!/bin/bash
cat info.txt
```

**PowerShell**
```powershell
Get-Content info.txt
```

**Python**
```python
with open("info.txt", "r") as f:
    print(f.read())
```

---

### **3.5 Redirección de Entrada**
En algunos casos, en lugar de escribir en pantalla, un script puede leer de un archivo.

| Lenguaje | Código |
|----------|--------|
| Bash | `sort < archivo.txt` |
| PowerShell | `Get-Content archivo.txt | Sort-Object` |
| Python | `with open("archivo.txt") as f: datos = f.readlines(); datos.sort()` |

**Ejemplo: Ordenar líneas de un archivo.**

**Bash**
```bash
#!/bin/bash
sort < info.txt
```

**PowerShell**
```powershell
Get-Content info.txt | Sort-Object
```

**Python**
```python
with open("info.txt", "r") as f:
    lineas = f.readlines()
    for linea in sorted(lineas):
        print(linea.strip())
```

---

### **Ejercicio Práctico 3: Registro de Eventos**

#### **Objetivo**
- Pedir el nombre de usuario.
- Registrar la fecha y la operación realizada.
- Guardar el resultado en un archivo `registro.log`.

##### **Solución en Bash**
```bash
#!/bin/bash
read -p "Introduce tu nombre: " nombre
echo "$(date) - Usuario: $nombre inició sesión" >> registro.log
echo "Registro guardado en registro.log"
```

##### **Solución en PowerShell**
```powershell
$nombre = Read-Host "Introduce tu nombre"
"$((Get-Date) - Usuario: $nombre inició sesión)" | Add-Content registro.log
Write-Output "Registro guardado en registro.log"
```

##### **Solución en Python**
```python
import datetime

nombre = input("Introduce tu nombre: ")

with open("registro.log", "a") as f:
    f.write(f"{datetime.datetime.now()} - Usuario: {nombre} inició sesión\n")

print("Registro guardado en registro.log")
```

---

Este capítulo cubre la entrada y salida estándar, la redirección y la manipulación de archivos. **¿Pasamos al siguiente tema?**

