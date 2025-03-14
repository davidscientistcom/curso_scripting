**Módulo 5: PowerShell Avanzado**

### **Tema 10: Scripting Avanzado en PowerShell**

### **10.1 Introducción a PowerShell Avanzado**

PowerShell es una herramienta poderosa para la administración de sistemas Windows y Linux, permitiendo:

- Automatización de tareas administrativas complejas.
- Gestión de servidores y Active Directory.
- Control de servicios, procesos y eventos.
- Creación de interfaces gráficas.

Este módulo cubrirá conceptos avanzados de PowerShell para aprovechar al máximo sus capacidades.

### **10.2 Manejo Avanzado de Objetos en PowerShell**

PowerShell se basa en objetos, lo que permite manipular datos de manera estructurada.

#### **Ejemplo 1: Trabajar con objetos y propiedades**
```powershell
$proceso = Get-Process | Select-Object -First 1
$proceso | Format-List *
```

#### **Ejemplo 2: Filtrar y ordenar objetos**
```powershell
Get-Process | Where-Object { $_.CPU -gt 100 } | Sort-Object -Property CPU -Descending
```

#### **Ejemplo 3: Crear objetos personalizados**
```powershell
$obj = [PSCustomObject]@{
    Nombre = "David"
    Edad = 30
    Ciudad = "Madrid"
}
$obj
```


### **10.3 Funciones y Módulos en PowerShell**

#### **Ejemplo 4: Definir y llamar una función**
```powershell
function Saludar {
    param ($nombre)
    Write-Output "Hola, $nombre!"
}
Saludar -nombre "Carlos"
```

#### **Ejemplo 5: Crear un módulo reutilizable**
1. Crear un archivo `MiModulo.psm1`:
```powershell
function Get-Info {
    Get-WmiObject Win32_OperatingSystem | Select-Object Caption, Version, OSArchitecture
}
```
2. Importar el módulo en otro script:
```powershell
Import-Module .\MiModulo.psm1
Get-Info
```


### **10.4 Creación de Interfaces Gráficas (GUI) en PowerShell**

PowerShell permite crear interfaces gráficas para interactuar con los usuarios.

#### **Ejemplo 9: Crear una ventana emergente con un mensaje**
```powershell
Add-Type -AssemblyName System.Windows.Forms
[System.Windows.Forms.MessageBox]::Show("Hola desde PowerShell GUI")
```

#### **Ejemplo 10: Crear una interfaz con botones y entrada de texto**
```powershell
Add-Type -AssemblyName System.Windows.Forms
$form = New-Object System.Windows.Forms.Form
$form.Text = "Formulario PowerShell"
$form.Size = New-Object System.Drawing.Size(300,200)

$label = New-Object System.Windows.Forms.Label
$label.Text = "Introduce tu nombre:"
$label.Location = New-Object System.Drawing.Point(10,10)
$form.Controls.Add($label)

$input = New-Object System.Windows.Forms.TextBox
$input.Location = New-Object System.Drawing.Point(10,30)
$form.Controls.Add($input)

$button = New-Object System.Windows.Forms.Button
$button.Text = "Aceptar"
$button.Location = New-Object System.Drawing.Point(10,60)
$button.Add_Click({ [System.Windows.Forms.MessageBox]::Show("Hola, " + $input.Text) })
$form.Controls.Add($button)

$form.ShowDialog()
```

### **10.5 Programación de Tareas Automatizadas con PowerShell**

#### **Ejemplo 11: Crear una tarea programada**
```powershell
$action = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-File C:\Scripts\Backup.ps1"
$trigger = New-ScheduledTaskTrigger -Daily -At 2AM
Register-ScheduledTask -TaskName "Backup Diario" -Action $action -Trigger $trigger -User "SYSTEM"
```

#### **Ejemplo 12: Monitoreo de Eventos del Sistema**
```powershell
Get-WinEvent -LogName "Application" -MaxEvents 10 | Format-Table TimeCreated, Id, Message -AutoSize
```

### **Ejercicio Final: Automatización Completa con PowerShell**

**Objetivo:** Crear un script que realice las siguientes acciones:
- Monitorear el uso de CPU y memoria.
- Guardar la información en un archivo.
- Enviar un correo si el uso de CPU supera el 80%.

```powershell
$cpu = Get-WmiObject Win32_Processor | Select-Object LoadPercentage
$mem = Get-WmiObject Win32_OperatingSystem | Select-Object FreePhysicalMemory, TotalVisibleMemorySize

$usoCPU = $cpu.LoadPercentage
$usoMemoria = 100 - (($mem.FreePhysicalMemory / $mem.TotalVisibleMemorySize) * 100)

$reporte = "CPU: $usoCPU% - Memoria: $usoMemoria%"
$reporte | Out-File "C:\Monitoreo\reporte.txt"

if ($usoCPU -gt 80) {
    Send-MailMessage -To "admin@dominio.com" -From "alertas@dominio.com" -Subject "Alerta de CPU" -Body "Uso alto de CPU: $usoCPU%" -SmtpServer "smtp.dominio.com"
}
```
