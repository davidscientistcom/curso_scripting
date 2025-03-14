**Módulo 3: Gestión de Archivos y Procesos en Scripting**

### **Tema 6: Manejo de Archivos y Directorios en Bash, PowerShell y Python**

### **6.1 Introducción al Manejo de Archivos**
Los scripts de administración de sistemas suelen necesitar manipular archivos y directorios, ya sea para leer, escribir, eliminar o mover información.

Las operaciones básicas incluyen:
- Verificar la existencia de un archivo o directorio.
- Crear, leer y escribir archivos.
- Listar archivos en un directorio.
- Eliminar, copiar y mover archivos.

### **6.2 Verificar la Existencia de un Archivo o Directorio**
Antes de manipular un archivo o directorio, es importante verificar si existe.

#### **Ejemplo 1: Comprobar si un archivo existe**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el nombre del archivo: " archivo
if [ -f "$archivo" ]; then
    echo "El archivo '$archivo' existe."
else
    echo "El archivo '$archivo' NO existe."
fi
```

**PowerShell**
```powershell
$archivo = Read-Host "Introduce el nombre del archivo"
if (Test-Path $archivo) {
    Write-Output "El archivo '$archivo' existe."
} else {
    Write-Output "El archivo '$archivo' NO existe."
}
```

**Python**
```python
import os
archivo = input("Introduce el nombre del archivo: ")
if os.path.isfile(archivo):
    print(f"El archivo '{archivo}' existe.")
else:
    print(f"El archivo '{archivo}' NO existe.")
```

### **6.3 Crear y Escribir en un Archivo**

#### **Ejemplo 2: Crear un archivo y escribir contenido**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el nombre del archivo: " archivo
echo "Este es un archivo creado con Bash." > "$archivo"
echo "Archivo '$archivo' creado correctamente."
```

**PowerShell**
```powershell
$archivo = Read-Host "Introduce el nombre del archivo"
"Este es un archivo creado con PowerShell." | Out-File $archivo
Write-Output "Archivo '$archivo' creado correctamente."
```

**Python**
```python
archivo = input("Introduce el nombre del archivo: ")
with open(archivo, "w") as f:
    f.write("Este es un archivo creado con Python.\n")
print(f"Archivo '{archivo}' creado correctamente.")
```

### **6.4 Leer el Contenido de un Archivo**

#### **Ejemplo 3: Leer un archivo línea por línea**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el nombre del archivo a leer: " archivo
if [ -f "$archivo" ]; then
    cat "$archivo"
else
    echo "El archivo '$archivo' no existe."
fi
```

**PowerShell**
```powershell
$archivo = Read-Host "Introduce el nombre del archivo a leer"
if (Test-Path $archivo) {
    Get-Content $archivo
} else {
    Write-Output "El archivo '$archivo' no existe."
}
```

**Python**
```python
archivo = input("Introduce el nombre del archivo a leer: ")
try:
    with open(archivo, "r") as f:
        print(f.read())
except FileNotFoundError:
    print(f"El archivo '{archivo}' no existe.")
```

### **6.5 Listar Archivos en un Directorio**

#### **Ejemplo 4: Listar archivos en un directorio específico**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el directorio: " directorio
if [ -d "$directorio" ]; then
    ls -l "$directorio"
else
    echo "El directorio '$directorio' no existe."
fi
```

**PowerShell**
```powershell
$directorio = Read-Host "Introduce el directorio"
if (Test-Path $directorio) {
    Get-ChildItem -Path $directorio
} else {
    Write-Output "El directorio '$directorio' no existe."
}
```

**Python**
```python
import os
directorio = input("Introduce el directorio: ")
if os.path.isdir(directorio):
    archivos = os.listdir(directorio)
    for archivo in archivos:
        print(archivo)
else:
    print(f"El directorio '{directorio}' no existe.")
```

### **6.6 Copiar, Mover y Eliminar Archivos**

#### **Ejemplo 5: Copiar un archivo**

**Bash**
```bash
#!/bin/bash
read -p "Introduce el archivo de origen: " origen
read -p "Introduce el archivo de destino: " destino
if [ -f "$origen" ]; then
    cp "$origen" "$destino"
    echo "Archivo copiado correctamente."
else
    echo "El archivo '$origen' no existe."
fi
```

**PowerShell**
```powershell
$origen = Read-Host "Introduce el archivo de origen"
$destino = Read-Host "Introduce el archivo de destino"
if (Test-Path $origen) {
    Copy-Item -Path $origen -Destination $destino
    Write-Output "Archivo copiado correctamente."
} else {
    Write-Output "El archivo '$origen' no existe."
}
```

**Python**
```python
import shutil
import os
origen = input("Introduce el archivo de origen: ")
destino = input("Introduce el archivo de destino: ")
if os.path.isfile(origen):
    shutil.copy(origen, destino)
    print("Archivo copiado correctamente.")
else:
    print(f"El archivo '{origen}' no existe.")
```
