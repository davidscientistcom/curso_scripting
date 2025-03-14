**Módulo 3: Gestión de Archivos y Procesos en Scripting**

### **Tema 7: Gestión de Procesos en Bash, PowerShell y Python**

### **7.1 Introducción a la Gestión de Procesos**
Un proceso es un programa en ejecución dentro del sistema operativo. Los administradores de sistemas suelen necesitar:

- Listar procesos activos.
- Filtrar procesos por nombre o usuario.
- Finalizar procesos.
- Monitorear el consumo de CPU y memoria.

### **7.2 Listar Procesos en Ejecución**

#### **Ejemplo 1: Mostrar todos los procesos activos**

**Bash**
```bash
#!/bin/bash
ps aux
```

**PowerShell**
```powershell
Get-Process
```

**Python**
```python
import psutil
for proceso in psutil.process_iter(['pid', 'name']):
    print(f"PID: {proceso.info['pid']}, Nombre: {proceso.info['name']}")
```

#### **Ejemplo 2: Filtrar Procesos por Nombre**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el nombre del proceso: " proceso
ps aux | grep "$proceso" | grep -v grep
```

**PowerShell**
```powershell
$proceso = Read-Host "Introduce el nombre del proceso"
Get-Process | Where-Object { $_.ProcessName -match $proceso }
```

**Python**
```python
import psutil
proceso = input("Introduce el nombre del proceso: ").lower()
for p in psutil.process_iter(['pid', 'name']):
    if proceso in p.info['name'].lower():
        print(f"PID: {p.info['pid']}, Nombre: {p.info['name']}")
```

### **7.3 Finalizar Procesos**

#### **Ejemplo 3: Terminar un Proceso por PID**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el PID del proceso a terminar: " pid
kill "$pid"
echo "Proceso $pid terminado."
```

**PowerShell**
```powershell
$pid = Read-Host "Introduce el PID del proceso a terminar"
Stop-Process -Id $pid -Force
Write-Output "Proceso $pid terminado."
```

**Python**
```python
import psutil
pid = int(input("Introduce el PID del proceso a terminar: "))
try:
    proceso = psutil.Process(pid)
    proceso.terminate()
    print(f"Proceso {pid} terminado.")
except psutil.NoSuchProcess:
    print("No se encontró el proceso.")
```

#### **Ejemplo 4: Finalizar Todos los Procesos con un Nombre Específico**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el nombre del proceso a terminar: " proceso
pkill "$proceso"
echo "Procesos '$proceso' terminados."
```

**PowerShell**
```powershell
$proceso = Read-Host "Introduce el nombre del proceso a terminar"
Get-Process | Where-Object { $_.ProcessName -match $proceso } | Stop-Process -Force
Write-Output "Procesos '$proceso' terminados."
```

**Python**
```python
import psutil
proceso = input("Introduce el nombre del proceso a terminar: ").lower()
for p in psutil.process_iter(['pid', 'name']):
    if proceso in p.info['name'].lower():
        print(f"Terminando proceso: {p.info['pid']} - {p.info['name']}")
        p.terminate()
```

### **7.4 Monitoreo de Consumo de CPU y Memoria por Procesos**

#### **Ejemplo 5: Ver Procesos que Consumen más CPU**

**Bash**
```bash
#!/bin/bash
ps aux --sort=-%cpu | head -10
```

**PowerShell**
```powershell
Get-Process | Sort-Object -Property CPU -Descending | Select-Object -First 10
```

**Python**
```python
import psutil
procesos = sorted(psutil.process_iter(['pid', 'name', 'cpu_percent']), key=lambda p: p.info['cpu_percent'], reverse=True)
for p in procesos[:10]:
    print(f"PID: {p.info['pid']}, Nombre: {p.info['name']}, CPU: {p.info['cpu_percent']}%")
```

#### **Ejemplo 6: Ver Procesos que Consumen más Memoria**

**Bash**
```bash
#!/bin/bash
ps aux --sort=-%mem | head -10
```

**PowerShell**
```powershell
Get-Process | Sort-Object -Property WS -Descending | Select-Object -First 10
```

**Python**
```python
import psutil
procesos = sorted(psutil.process_iter(['pid', 'name', 'memory_percent']), key=lambda p: p.info['memory_percent'], reverse=True)
for p in procesos[:10]:
    print(f"PID: {p.info['pid']}, Nombre: {p.info['name']}, Memoria: {p.info['memory_percent']}%")
```

### **7.5 Iniciar y Reiniciar Procesos**

#### **Ejemplo 7: Iniciar un Proceso**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el comando a ejecutar: " comando
$comando &
echo "Proceso iniciado: $comando"
```

**PowerShell**
```powershell
$comando = Read-Host "Introduce el comando a ejecutar"
Start-Process -FilePath $comando
Write-Output "Proceso iniciado: $comando"
```

**Python**
```python
import subprocess
comando = input("Introduce el comando a ejecutar: ")
subprocess.Popen(comando, shell=True)
print(f"Proceso iniciado: {comando}")
```

#### **Ejemplo 8: Reiniciar un Proceso**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el nombre del proceso a reiniciar: " proceso
pkill "$proceso"
$proceso &
echo "Proceso '$proceso' reiniciado."
```

**PowerShell**
```powershell
$proceso = Read-Host "Introduce el nombre del proceso a reiniciar"
Get-Process -Name $proceso | Stop-Process -Force
Start-Process -FilePath $proceso
Write-Output "Proceso '$proceso' reiniciado."
```

**Python**
```python
import psutil
import subprocess
proceso = input("Introduce el nombre del proceso a reiniciar: ").lower()
for p in psutil.process_iter(['pid', 'name']):
    if proceso in p.info['name'].lower():
        p.terminate()
subprocess.Popen(proceso, shell=True)
print(f"Proceso '{proceso}' reiniciado.")
```