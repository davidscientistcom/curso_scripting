**Módulo 6: Bash Avanzado**

### **Tema 11: Scripting Avanzado en Bash**

### **11.1 Introducción a Bash Avanzado**

Bash es un poderoso lenguaje de scripting en sistemas Unix/Linux, permitiendo:

- Automatización de tareas complejas.
- Manipulación avanzada de archivos y procesos.
- Gestión de redes y servicios.
- Interacción con bases de datos y APIs.

Este módulo cubre conceptos avanzados para escribir scripts eficientes y robustos.


### **11.2 Variables Avanzadas y Parámetros**

#### **Ejemplo 1: Parámetros Posicionales y Opcionales**
```bash
#!/bin/bash
nombre=$1
edad=${2:-"Desconocida"}
echo "Hola, $nombre. Tienes $edad años."
```

#### **Ejemplo 2: Variables de Entorno y Exportación**
```bash
#!/bin/bash
export PATH="$PATH:/usr/local/bin"
echo "Nuevo PATH: $PATH"
```

#### **Ejemplo 3: Expansión de Parámetros**
```bash
#!/bin/bash
archivo="/home/user/documento.txt"
echo "Nombre: ${archivo##*/}"
echo "Extensión: ${archivo##*.}"
echo "Ruta sin extensión: ${archivo%.*}"
```

### **11.3 Control de Flujo y Funciones Avanzadas**

#### **Ejemplo 4: Uso de Arrays y Bucles**
```bash
#!/bin/bash
nombres=("Carlos" "Ana" "David")
for nombre in "${nombres[@]}"; do
    echo "Hola, $nombre"
done
```

#### **Ejemplo 5: Definir y Llamar Funciones**
```bash
#!/bin/bash
saludar() {
    echo "Hola, $1!"
}
saludar "Pedro"
```

### **11.4 Manejo Avanzado de Archivos y Directorios**

#### **Ejemplo 6: Buscar y Procesar Archivos**
```bash
#!/bin/bash
find /var/log -type f -name "*.log" -exec grep "error" {} \;
```

#### **Ejemplo 7: Backup Automático de Archivos**
```bash
#!/bin/bash
fecha=$(date +%Y%m%d)
tar -czf "/backup/respaldo_$fecha.tar.gz" /home/user/documentos
echo "Respaldo creado"
```

### **11.5 Interacción con Redes y Servicios**

#### **Ejemplo 8: Verificar Conectividad a un Servidor**
```bash
#!/bin/bash
servidor="google.com"
if ping -c 1 $servidor &> /dev/null; then
    echo "Conectado a $servidor"
else
    echo "No se pudo conectar a $servidor"
fi
```

#### **Ejemplo 9: Descargar Datos desde una API**
```bash
#!/bin/bash
curl -s "https://api.weatherapi.com/v1/current.json?key=API_KEY&q=Madrid"
```

### **11.6 Automatización de Tareas y Systemd**

#### **Ejemplo 10: Programar una Tarea con Cron**
```bash
crontab -e
# Agregar: 0 2 * * * /home/user/backup.sh
```

#### **Ejemplo 11: Crear un Servicio con systemd**
1. Crear el archivo del servicio:
```bash
sudo nano /etc/systemd/system/mi_script.service
```

2. Agregar contenido:
```ini
[Unit]
Description=Ejecutar script de mantenimiento

[Service]
ExecStart=/home/user/mi_script.sh
```

3. Habilitar y arrancar el servicio:
```bash
sudo systemctl enable mi_script.service
sudo systemctl start mi_script.service
```

### **11.7 Ejercicio Final: Monitoreo y Notificaciones**

**Objetivo:** Monitorear el uso de CPU y memoria, guardar un log y enviar un correo si el CPU supera el 80%.

```bash
#!/bin/bash
cpu=$(top -bn1 | grep "Cpu(s)" | awk '{print $2 + $4}')
mem=$(free -m | awk 'NR==2{printf "%.2f", $3*100/$2 }')

log="/var/log/monitoreo.log"
echo "$(date) - CPU: $cpu% - Memoria: $mem%" >> $log

if (( $(echo "$cpu > 80" | bc -l) )); then
    echo "Alerta: Uso alto de CPU" | mail -s "Alerta de CPU" admin@dominio.com
fi
```
