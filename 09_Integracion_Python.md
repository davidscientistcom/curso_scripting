**Módulo 4: Redes y Automatización**

### **Tema 9: Integración de Python con Bash y PowerShell**

### **9.1 Introducción a la Integración entre Lenguajes**

Python es un lenguaje de scripting versátil que permite interactuar con Bash en Linux y macOS, y con PowerShell en Windows. Esta integración es útil para:

- Ejecutar comandos del sistema desde Python.
- Capturar y procesar la salida de scripts Bash y PowerShell.
- Combinar la automatización en diferentes entornos.


### **9.2 Ejecutar Comandos de Bash y PowerShell desde Python**

#### **Ejemplo 1: Ejecutar un Comando Simple**

**Bash desde Python**
```python
import subprocess

resultado = subprocess.run(["ls", "-l"], capture_output=True, text=True)
print(resultado.stdout)
```

**PowerShell desde Python**
```python
import subprocess

resultado = subprocess.run(["powershell", "Get-Process"], capture_output=True, text=True)
print(resultado.stdout)
```

#### **Ejemplo 2: Capturar la Salida de un Script**

**Bash**
```bash
#!/bin/bash
echo "Hola desde Bash"
```
**Python llamando al script:**
```python
import subprocess

resultado = subprocess.run(["./script.sh"], capture_output=True, text=True)
print(resultado.stdout)
```

**PowerShell**
```powershell
Write-Output "Hola desde PowerShell"
```
**Python llamando al script:**
```python
resultado = subprocess.run(["powershell", "-File", "script.ps1"], capture_output=True, text=True)
print(resultado.stdout)
```



### **9.3 Ejecutar Scripts Python desde Bash y PowerShell**

#### **Ejemplo 3: Llamar a un Script de Python desde Bash**

**Python (script.py)**
```python
print("Hola desde Python")
```
**Bash**
```bash
#!/bin/bash
python3 script.py
```

#### **Ejemplo 4: Llamar a un Script de Python desde PowerShell**

**PowerShell**
```powershell
python script.py
```



### **9.4 Comunicación entre Scripts de Diferentes Lenguajes**

#### **Ejemplo 5: Pasar Parámetros entre Bash, PowerShell y Python**

**Bash (script_bash.sh)**
```bash
#!/bin/bash
nombre=$1
echo "Hola, $nombre desde Bash"
```
**Llamada desde Python:**
```python
import subprocess

nombre = "David"
resultado = subprocess.run(["bash", "script_bash.sh", nombre], capture_output=True, text=True)
print(resultado.stdout)
```

**PowerShell (script_ps.ps1)**
```powershell
param ($nombre)
Write-Output "Hola, $nombre desde PowerShell"
```
**Llamada desde Python:**
```python
resultado = subprocess.run(["powershell", "-File", "script_ps.ps1", "David"], capture_output=True, text=True)
print(resultado.stdout)
```



### **9.5 Casos Prácticos de Integración**

#### **Ejemplo 6: Monitoreo de Procesos con Python y Bash/PowerShell**

**Bash**
```bash
#!/bin/bash
ps aux --sort=-%cpu | head -5
```
**PowerShell**
```powershell
Get-Process | Sort-Object -Property CPU -Descending | Select-Object -First 5
```
**Python llamando a ambos scripts:**
```python
import subprocess

print("Monitoreo en Linux:")
resultado_bash = subprocess.run(["bash", "script_bash.sh"], capture_output=True, text=True)
print(resultado_bash.stdout)

print("Monitoreo en Windows:")
resultado_ps = subprocess.run(["powershell", "-File", "script_ps.ps1"], capture_output=True, text=True)
print(resultado_ps.stdout)
```

#### **Ejemplo 7: Script de Copia de Seguridad con Python y Bash/PowerShell**

**Python (backup.py)**
```python
import subprocess

sistema = input("Ingrese el sistema operativo (linux/windows): ").lower()
if sistema == "linux":
    subprocess.run(["bash", "backup.sh"])
elif sistema == "windows":
    subprocess.run(["powershell", "-File", "backup.ps1"])
else:
    print("Sistema no reconocido")
```

**Bash (backup.sh)**
```bash
#!/bin/bash
tar -czf /backup/respaldo_$(date +%F).tar.gz /ruta/a/copiar
echo "Respaldo creado en Linux"
```

**PowerShell (backup.ps1)**
```powershell
$fecha = Get-Date -Format "yyyy-MM-dd"
Compress-Archive -Path "C:\ruta\a\copiar" -DestinationPath "C:\backup\respaldo_$fecha.zip"
Write-Output "Respaldo creado en Windows"
```



### **Ejercicio Final: Creación de un Sistema de Administración de Usuarios**

Objetivo:
- Un script en Python que cree usuarios en Linux con Bash y en Windows con PowerShell.

**Python (gestor_usuarios.py)**
```python
import subprocess

usuario = input("Introduce el nombre del nuevo usuario: ")
sistema = input("Ingrese el sistema operativo (linux/windows): ").lower()

if sistema == "linux":
    subprocess.run(["bash", "crear_usuario.sh", usuario])
elif sistema == "windows":
    subprocess.run(["powershell", "-File", "crear_usuario.ps1", usuario])
else:
    print("Sistema no reconocido")
```

**Bash (crear_usuario.sh)**
```bash
#!/bin/bash
usuario=$1
sudo useradd "$usuario"
echo "Usuario $usuario creado en Linux."
```

**PowerShell (crear_usuario.ps1)**
```powershell
param ($usuario)
New-LocalUser -Name $usuario -NoPassword
Write-Output "Usuario $usuario creado en Windows."
```
