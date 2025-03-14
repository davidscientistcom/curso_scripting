**Módulo 7: Automatización en Linux con Cron**

### **Tema 12: Funcionamiento Detallado de Cron en Ubuntu**



### **12.1 Introducción a Cron**

Cron es un demonio en sistemas Unix/Linux que permite programar la ejecución de comandos o scripts en momentos específicos. Es utilizado para la automatización de tareas repetitivas como:

- Copias de seguridad.
- Monitoreo del sistema.
- Actualización de paquetes.
- Envío de informes automáticos.

En Ubuntu, el servicio de `cron` se ejecuta en segundo plano y revisa los archivos de configuración para determinar qué tareas debe ejecutar y cuándo.



### **12.2 Estructura de Crontab**

Las tareas programadas en Cron se definen en archivos llamados `crontab`. Cada línea de un `crontab` representa una tarea programada y sigue el siguiente formato:

```bash
MINUTO HORA DÍA_DEL_MES MES DÍA_DE_LA_SEMANA COMANDO_A_EJECUTAR
```

| Campo            | Valores Permitidos  | Descripción |
|------------------|--------------------|-------------|
| **MINUTO**       | 0-59               | Minuto de ejecución |
| **HORA**         | 0-23               | Hora de ejecución |
| **DÍA_DEL_MES**  | 1-31               | Día del mes |
| **MES**          | 1-12               | Mes del año |
| **DÍA_DE_LA_SEMANA** | 0-7 (0 y 7 = Domingo) | Día de la semana |
| **COMANDO_A_EJECUTAR** | Ruta del script o comando | Acción a realizar |

Ejemplo de una tarea programada:
```bash
30 2 * * * /home/user/backup.sh
```
Esto ejecutará el script `backup.sh` todos los días a las **2:30 AM**.



### **12.3 Gestión de Tareas con Crontab**

#### **Ver y Editar el Crontab del Usuario**

Para abrir y editar el crontab del usuario actual:
```bash
crontab -e
```
Esto abrirá el archivo de configuración en un editor de texto como `nano` o `vim`.

#### **Listar las Tareas Programadas**
```bash
crontab -l
```
Muestra todas las tareas programadas para el usuario actual.

#### **Eliminar el Crontab del Usuario**
```bash
crontab -r
```
Elimina todas las tareas programadas del usuario actual.


### **12.4 Expresiones Especiales en Crontab**

Para mayor flexibilidad, se pueden usar caracteres especiales en los campos de tiempo:

| Símbolo | Significado |
|---------|------------|
| `*`     | Todos los valores permitidos (ej: cada minuto, cada hora) |
| `,`     | Lista de valores (ej: `1,5,10` = minuto 1, 5 y 10) |
| `-`     | Rango de valores (ej: `1-5` = del minuto 1 al 5) |
| `/`     | Intervalos (ej: `*/5` = cada 5 minutos) |

Ejemplo: Ejecutar un script cada 15 minutos:
```bash
*/15 * * * * /home/user/script.sh
```

Ejemplo: Ejecutar un script los lunes a las 3 AM:
```bash
0 3 * * 1 /home/user/mantenimiento.sh
```

### **12.5 Cron y Permisos de Usuarios**

Los archivos de configuración de `cron` pueden ser gestionados a nivel global y por usuario:

- **`/etc/crontab`**: Configuración del sistema. Solo accesible por `root`.
- **`/etc/cron.d/`**: Contiene archivos de configuración de cron para servicios específicos.
- **`/var/spool/cron/crontabs/`**: Contiene los crontabs de cada usuario del sistema.

Para permitir o restringir el uso de `cron` para un usuario específico:

- **Permitir acceso:** Agregar el usuario a `/etc/cron.allow`.
- **Restringir acceso:** Agregar el usuario a `/etc/cron.deny`.


### **12.6 Logs y Depuración de Cron**

Para verificar si una tarea se está ejecutando correctamente, se pueden revisar los logs:
```bash
grep CRON /var/log/syslog
```

También se pueden redirigir las salidas de los scripts para depuración:
```bash
*/10 * * * * /home/user/script.sh >> /home/user/cron.log 2>&1
```
Esto guardará la salida del script en `cron.log`, incluyendo errores.


### **12.7 Automatización Avanzada con Cron**

#### **Ejemplo 1: Ejecutar un Script Solo en Días Hábiles**
```bash
0 9 * * 1-5 /home/user/tarea.sh
```
Ejecutará `tarea.sh` a las 9 AM de lunes a viernes.

#### **Ejemplo 2: Reiniciar un Servicio Cada Hora**
```bash
0 * * * * systemctl restart apache2
```
Reinicia el servicio Apache cada hora.

#### **Ejemplo 3: Enviar un Correo con los Logs del Sistema Cada Día**
```bash
0 6 * * * cat /var/log/syslog | mail -s "Reporte de Logs" admin@dominio.com
```
Envía un correo con los logs del sistema a las 6 AM.

---

### **12.8 Alternativas a Cron**

Aunque `cron` es una herramienta poderosa, existen alternativas como:

- **`systemd timers`**: Más potente y flexible, ideal para servicios modernos.
- **`at`**: Para ejecutar tareas una sola vez en un tiempo determinado.
- **`anacron`**: Similar a `cron`, pero diseñado para sistemas que no están encendidos todo el tiempo.

Ejemplo de un `systemd timer`:
```bash
[Unit]
Description=Ejecutar respaldo diario

[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

---

### **Ejercicio Final: Configuración de Tareas con Cron**

#### **Objetivo**
Crear un script que realice lo siguiente:

1. Monitoree el uso de CPU y memoria.
2. Guarde un log con la información.
3. Se ejecute cada 10 minutos.

#### **Solución**

**Script de monitoreo (`monitor.sh`)**
```bash
#!/bin/bash
echo "$(date) - CPU: $(top -bn1 | grep 'Cpu(s)' | awk '{print $2 + $4}')% - Memoria: $(free -m | awk 'NR==2{printf "%.2f%%", $3*100/$2 }')" >> /var/log/monitor.log
```

**Agregarlo a `cron`**
```bash
crontab -e
# Agregar la siguiente línea:
*/10 * * * * /home/user/monitor.sh
```
