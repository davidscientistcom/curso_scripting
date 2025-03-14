**Módulo 2: Control de Flujo y Estructuras de Control**

### **Tema 4: Condicionales en Bash, PowerShell y Python**

### **4.1 Introducción a los Condicionales**
Los condicionales permiten ejecutar diferentes bloques de código dependiendo de si una condición se cumple o no. Se usan para la toma de decisiones en un script.

#### **Estructura básica en cada lenguaje**
| Lenguaje | Sintaxis |
|----------|---------|
| **Bash** | `if [ condición ]; then ... elif [ otra_condición ]; then ... else ... fi` |
| **PowerShell** | `if (condición) { ... } elseif (otra_condición) { ... } else { ... }` |
| **Python** | `if condición: ... elif otra_condición: ... else: ...` |


### **4.2 Condicional `if`**
**Ejemplo 1: Verificar si un número es positivo**

**Bash**
```bash
#!/bin/bash
read -p "Introduce un número: " num
if [ "$num" -gt 0 ]; then
    echo "El número es positivo."
fi
```

**PowerShell**
```powershell
$num = Read-Host "Introduce un número"
if ($num -gt 0) {
    Write-Output "El número es positivo."
}
```

**Python**
```python
num = int(input("Introduce un número: "))
if num > 0:
    print("El número es positivo.")
```

### **4.3 `if-else`: Alternativa en caso contrario**
**Ejemplo 2: Verificar si un número es positivo o negativo**

**Bash**
```bash
#!/bin/bash
read -p "Introduce un número: " num
if [ "$num" -gt 0 ]; then
    echo "El número es positivo."
else
    echo "El número es negativo o cero."
fi
```

**PowerShell**
```powershell
$num = Read-Host "Introduce un número"
if ($num -gt 0) {
    Write-Output "El número es positivo."
} else {
    Write-Output "El número es negativo o cero."
}
```

**Python**
```python
num = int(input("Introduce un número: "))
if num > 0:
    print("El número es positivo.")
else:
    print("El número es negativo o cero.")
```


### **4.4 `if-elif-else`: Múltiples condiciones**
**Ejemplo 3: Clasificar un número en positivo, negativo o cero**

**Bash**
```bash
#!/bin/bash
read -p "Introduce un número: " num
if [ "$num" -gt 0 ]; then
    echo "El número es positivo."
elif [ "$num" -lt 0 ]; then
    echo "El número es negativo."
else
    echo "El número es cero."
fi
```

**PowerShell**
```powershell
$num = Read-Host "Introduce un número"
if ($num -gt 0) {
    Write-Output "El número es positivo."
} elseif ($num -lt 0) {
    Write-Output "El número es negativo."
} else {
    Write-Output "El número es cero."
}
```

**Python**
```python
num = int(input("Introduce un número: "))
if num > 0:
    print("El número es positivo.")
elif num < 0:
    print("El número es negativo.")
else:
    print("El número es cero.")
```


### **4.5 Condicionales Anidados**
**Ejemplo 4: Verificar si un número está en un rango y si es par**

**Bash**
```bash
#!/bin/bash
read -p "Introduce un número: " num
if [ "$num" -ge 1 ] && [ "$num" -le 100 ]; then
    if [ $((num % 2)) -eq 0 ]; then
        echo "El número está en el rango y es par."
    else
        echo "El número está en el rango pero es impar."
    fi
else
    echo "El número está fuera del rango."
fi
```

**PowerShell**
```powershell
$num = Read-Host "Introduce un número"
if ($num -ge 1 -and $num -le 100) {
    if ($num % 2 -eq 0) {
        Write-Output "El número está en el rango y es par."
    } else {
        Write-Output "El número está en el rango pero es impar."
    }
} else {
    Write-Output "El número está fuera del rango."
}
```

**Python**
```python
num = int(input("Introduce un número: "))
if 1 <= num <= 100:
    if num % 2 == 0:
        print("El número está en el rango y es par.")
    else:
        print("El número está en el rango pero es impar.")
else:
    print("El número está fuera del rango.")
```


### **Ejercicio Práctico 5: Clasificación de Edades**

#### **Objetivo**
- Pedir la edad del usuario.
- Decir si es niño (0-12), adolescente (13-17), adulto (18-64) o mayor (65+).

##### **Solución en Bash**
```bash
#!/bin/bash
read -p "Introduce tu edad: " edad
if [ "$edad" -ge 0 ] && [ "$edad" -le 12 ]; then
    echo "Eres un niño."
elif [ "$edad" -ge 13 ] && [ "$edad" -le 17 ]; then
    echo "Eres un adolescente."
elif [ "$edad" -ge 18 ] && [ "$edad" -le 64 ]; then
    echo "Eres un adulto."
else
    echo "Eres mayor."
fi
```

##### **Solución en PowerShell**
```powershell
$edad = Read-Host "Introduce tu edad"
if ($edad -ge 0 -and $edad -le 12) {
    Write-Output "Eres un niño."
} elseif ($edad -ge 13 -and $edad -le 17) {
    Write-Output "Eres un adolescente."
} elseif ($edad -ge 18 -and $edad -le 64) {
    Write-Output "Eres un adulto."
} else {
    Write-Output "Eres mayor."
}
```

##### **Solución en Python**
```python
edad = int(input("Introduce tu edad: "))
if 0 <= edad <= 12:
    print("Eres un niño.")
elif 13 <= edad <= 17:
    print("Eres un adolescente.")
elif 18 <= edad <= 64:
    print("Eres un adulto.")
else:
    print("Eres mayor.")
```


