**Módulo 1: Fundamentos del Scripting en Sistemas**

### **Tema 1: Introducción a los Scripts de Sistema**

#### **1.1 ¿Qué es un script y para qué se usa en sistemas?**

Un script es un conjunto de instrucciones escritas en un lenguaje de scripting que se ejecutan en un intérprete en lugar de ser compiladas. Su propósito principal es la automatización de tareas repetitivas en un sistema operativo, facilitando la administración y optimización de procesos.

Algunas de las tareas comunes que se pueden automatizar con scripting incluyen:

- Gestión de usuarios y permisos.
- Monitoreo del sistema y servicios.
- Copias de seguridad y restauración de datos.
- Instalación y actualización de software.
- Manipulación de archivos y registros de logs.

Los scripts pueden ejecutarse manualmente o programarse para ejecutarse automáticamente en determinados momentos mediante herramientas como cron (Linux), Task Scheduler (Windows) o systemd timers.

#### **1.2 Diferencias entre Bash, PowerShell y Python en la administración de sistemas**

Cada lenguaje de scripting tiene sus propias características y casos de uso específicos:

| Característica  | Bash | PowerShell | Python |
|---------------|------|-----------|--------|
| **Plataforma** | Linux/macOS | Windows (aunque hay soporte en Linux) | Multiplataforma |
| **Propósito** | Administración de sistemas Unix/Linux | Administración de sistemas Windows (y ahora también Linux) | Uso general y administración multiplataforma |
| **Sintaxis** | Similar a comandos de terminal | Basado en objetos, más estructurado | Orientado a programación con muchas librerías |
| **Soporte de Objetos** | No | Sí, trabaja con objetos .NET | Sí, pero con enfoque más general |
| **Facilidad para tareas del sistema** | Muy eficiente para tareas en Linux | Ideal para Windows (Active Directory, registros, servicios) | Más flexible, pero requiere librerías externas para algunas tareas del sistema |
| **Interacción con el sistema** | Directa con comandos del sistema | Profunda integración con Windows y WMI | Uso de módulos como os, subprocess, etc. |

Ejemplo comparativo: Mostrar la fecha actual en cada lenguaje.

**Bash:**
```bash
date
```

**PowerShell:**
```powershell
Get-Date
```

**Python:**
```python
import datetime
print(datetime.datetime.now())
```

Cada lenguaje tiene su nicho específico, pero Python se está volviendo una alternativa cada vez más popular debido a su flexibilidad y amplia comunidad.

#### **1.3 Ejecución de scripts en cada entorno**

Para ejecutar scripts en cada uno de estos lenguajes, es necesario conocer los permisos y la sintaxis adecuada.

##### **Ejecución en Bash**

1. Crear un archivo de script con extensión `.sh`:
```bash
nano script.sh
```

2. Escribir un script sencillo:
```bash
#!/bin/bash
echo "Hola, mundo desde Bash"
```

3. Dar permisos de ejecución:
```bash
chmod +x script.sh
```

4. Ejecutar el script:
```bash
./script.sh
```

##### **Ejecución en PowerShell**

1. Crear un archivo con extensión `.ps1`:
```powershell
New-Item -Path script.ps1 -ItemType File
```

2. Escribir un script sencillo:
```powershell
Write-Output "Hola, mundo desde PowerShell"
```

3. Permitir ejecución de scripts si está restringida:
```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

4. Ejecutar el script:
```powershell
.\script.ps1
```

##### **Ejecución en Python**

1. Crear un archivo de script con extensión `.py`:
```bash
nano script.py
```

2. Escribir un script sencillo:
```python
print("Hola, mundo desde Python")
```

3. Ejecutar el script:
```bash
python3 script.py
```

> **Nota:** En Windows, puedes usar `python script.py` si tienes Python instalado y configurado en el `PATH`.


### **Ejemplo Práctico 1: Crear un script que muestre información básica del sistema**

El siguiente ejercicio permitirá familiarizarse con la ejecución de scripts en los tres lenguajes.

**Objetivo**
Crear un script que muestre:

1. El nombre del usuario actual.
2. La fecha y hora actual.
3. El sistema operativo en uso.

##### **Solución en Bash**
```bash
#!/bin/bash
echo "Usuario: $(whoami)"
echo "Fecha y hora: $(date)"
echo "Sistema operativo: $(uname -a)"
```

##### **Solución en PowerShell**
```powershell
Write-Output "Usuario: $env:USERNAME"
Write-Output "Fecha y hora: $(Get-Date)"
Write-Output "Sistema operativo: $(Get-CimInstance Win32_OperatingSystem).Caption"
```

##### **Solución en Python**
```python
import os
import datetime
import platform

print(f"Usuario: {os.getlogin()}")
print(f"Fecha y hora: {datetime.datetime.now()}")
print(f"Sistema operativo: {platform.system()} {platform.release()}")
```