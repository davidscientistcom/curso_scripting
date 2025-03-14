**Módulo 4: Redes y Automatización**

### **Tema 8: Automatización de Tareas con Cron, Task Scheduler y systemd**

### **8.1 Introducción a la Automatización de Tareas**
En la administración de sistemas, es común automatizar tareas recurrentes, como copias de seguridad, monitoreo de procesos o limpieza de archivos temporales. En este tema exploraremos:

- **Cron** en Linux/macOS.
- **Task Scheduler** en Windows.
- **systemd timers** en sistemas Linux modernos.

Cada uno de estos mecanismos permite programar tareas para que se ejecuten en intervalos específicos sin intervención manual.

### **8.2 Programación de Tareas con Cron (Linux/macOS)**

Cron es un servicio que permite programar la ejecución de comandos en intervalos de tiempo predefinidos.

#### **Ejemplo 1: Programar un script para ejecutarse cada 5 minutos**

1. Editar el crontab:
```bash
crontab -e
```

2. Agregar la siguiente línea al archivo:
```bash
*/5 * * * * /ruta/al/script.sh
```
Esto ejecuta `script.sh` cada 5 minutos.

3. Guardar y salir.
4. Verificar las tareas programadas:
```bash
crontab -l
```

#### **Ejemplo 2: Ejecutar un script diariamente a la medianoche**
```bash
0 0 * * * /ruta/al/script.sh
```

#### **Ejemplo 3: Registrar la salida de un script en un archivo de log**
```bash
0 0 * * * /ruta/al/script.sh >> /var/log/miscript.log 2>&1
```


### **8.3 Programación de Tareas con Task Scheduler (Windows)**

El **Task Scheduler** en Windows permite automatizar la ejecución de scripts y programas.

#### **Ejemplo 4: Crear una tarea para ejecutar un script de PowerShell cada 10 minutos**

1. Abrir **Task Scheduler** (`taskschd.msc`).
2. Crear una nueva tarea.
3. En la pestaña "Triggers", seleccionar **Repetir cada 10 minutos**.
4. En la pestaña "Acciones", seleccionar **Iniciar un programa** y escribir:
```powershell
powershell.exe -File "C:\ruta\a\script.ps1"
```
5. Guardar y habilitar la tarea.

#### **Ejemplo 5: Programar la ejecución de un script diariamente a las 2 AM**
1. Crear una nueva tarea.
2. Configurar el trigger para ejecutarse a las **2:00 AM**.
3. Configurar la acción para ejecutar `script.ps1`.
4. Guardar y habilitar la tarea.

### **8.4 Automatización con systemd timers (Linux Moderno)**

En sistemas Linux modernos, **systemd timers** pueden ser una alternativa a cron, proporcionando mayor flexibilidad y control.

#### **Ejemplo 6: Crear un timer para ejecutar un script cada 15 minutos**

1. Crear un servicio systemd:
```bash
sudo nano /etc/systemd/system/miscript.service
```

Contenido del archivo:
```ini
[Unit]
Description=Ejecutar mi script

[Service]
ExecStart=/ruta/al/script.sh
```

2. Crear el timer:
```bash
sudo nano /etc/systemd/system/miscript.timer
```

Contenido del archivo:
```ini
[Unit]
Description=Ejecutar mi script cada 15 minutos

[Timer]
OnBootSec=5min
OnUnitActiveSec=15min
Unit=miscript.service

[Install]
WantedBy=timers.target
```

3. Habilitar y arrancar el timer:
```bash
sudo systemctl enable miscript.timer
sudo systemctl start miscript.timer
```

4. Verificar timers activos:
```bash
systemctl list-timers
```

### **8.5 Ejercicio Final: Automatizar una Tarea de Copia de Seguridad**

Objetivo:
- Crear un script que haga una copia de seguridad de un directorio cada día.
- Programar la ejecución automática en Linux y Windows.

#### **Solución en Bash con Cron**
```bash
#!/bin/bash
tar -czf /backup/respaldo_$(date +%F).tar.gz /ruta/a/copiar
```

Programarlo con cron:
```bash
0 2 * * * /ruta/al/script.sh
```

#### **Solución en PowerShell con Task Scheduler**
```powershell
$fecha = Get-Date -Format "yyyy-MM-dd"
Compress-Archive -Path "C:\ruta\a\copiar" -DestinationPath "C:\backup\respaldo_$fecha.zip"
```

Configurar en Task Scheduler para ejecutarse a las **2 AM**.

