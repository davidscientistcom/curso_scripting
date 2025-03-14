**Módulo 1: Fundamentos del Scripting en Sistemas**

### **Tema 2: Variables y Operadores en Bash, PowerShell y Python**

### **2.1 Definición y uso de variables**

Una variable es un espacio en la memoria donde se almacena un valor que puede cambiar durante la ejecución del programa. En scripting, se utilizan para almacenar información como rutas de archivos, nombres de usuarios o configuraciones.

#### **Declaración de Variables**
Cada lenguaje tiene su propia forma de definir variables:

**Bash**
- No se usa `=` con espacios.
- No se declaran tipos de datos, todo es tratado como cadena de texto por defecto.
```bash
#!/bin/bash
nombre="David"
edad=30
echo "Hola, $nombre. Tienes $edad años."
```
- Para acceder a una variable, se usa `$nombre` o `${nombre}`.
- Para variables de solo lectura: `readonly nombre="David"`.
- Para eliminar una variable: `unset nombre`.

**PowerShell**
- Se usa `$` antes del nombre de la variable.
- Admite múltiples tipos de datos (`string`, `int`, `bool`, `array`).
```powershell
$nombre = "David"
$edad = 30
Write-Output "Hola, $nombre. Tienes $edad años."
```
- Para convertir tipos: `[int]$edad = "30"` (cambia de `string` a `entero`).
- Para eliminar una variable: `Remove-Variable nombre`.

**Python**
- No se usa `$`, simplemente se asigna con `=`.
- Python detecta automáticamente el tipo de dato.
```python
nombre = "David"
edad = 30
print(f"Hola, {nombre}. Tienes {edad} años.")
```
- Para borrar una variable: `del nombre`.



### **2.2 Tipos de Datos Básicos**
Cada lenguaje maneja los datos de manera diferente. En Bash, todo se trata como texto, mientras que en PowerShell y Python hay soporte para diferentes tipos de datos.

| Tipo de Dato  | Bash | PowerShell | Python |
|--------------|------|-----------|--------|
| Cadenas      | "texto" | "texto" | "texto" |
| Enteros      | "10" (texto) | 10 | 10 |
| Booleanos    | No existen como tipo | `$true / $false` | `True / False` |
| Listas       | `(("a" "b"))` | `@( "a", "b" )` | `[ "a", "b" ]` |
| Diccionarios | No existen | `@{key="value"}` | `{ "key": "value" }` |

Ejemplo de uso de listas/arrays:

**Bash**
```bash
nombres=("David" "Ana" "Carlos")
echo "El primer nombre es: ${nombres[0]}"
```

**PowerShell**
```powershell
$nombres = @("David", "Ana", "Carlos")
Write-Output "El primer nombre es: $($nombres[0])"
```

**Python**
```python
nombres = ["David", "Ana", "Carlos"]
print(f"El primer nombre es: {nombres[0]}")
```



### **2.3 Operadores Aritméticos, de Comparación y Lógicos**
Los operadores permiten realizar cálculos y comparaciones en scripts.

#### **Operadores Aritméticos**

| Operador   | Bash | PowerShell | Python |
|-----------|------|-----------|--------|
| Suma      | `$((a + b))` | `$a + $b` | `a + b` |
| Resta     | `$((a - b))` | `$a - $b` | `a - b` |
| Multiplicación | `$((a * b))` | `$a * $b` | `a * b` |
| División  | `$((a / b))` | `$a / $b` | `a / b` |
| Módulo   | `$((a % b))` | `$a % $b` | `a % b` |

Ejemplo en cada lenguaje:

**Bash**
```bash
a=10
b=5
echo "Suma: $((a + b))"
echo "Multiplicación: $((a * b))"
```

**PowerShell**
```powershell
$a = 10
$b = 5
Write-Output "Suma: $($a + $b)"
Write-Output "Multiplicación: $($a * $b)"
```

**Python**
```python
a = 10
b = 5
print(f"Suma: {a + b}")
print(f"Multiplicación: {a * b}")
```

#### **Operadores de Comparación**
Usados en condicionales (`if`).

| Operador   | Bash | PowerShell | Python |
|-----------|------|-----------|--------|
| Igual     | `[ "$a" -eq "$b" ]` | `$a -eq $b` | `a == b` |
| Diferente | `[ "$a" -ne "$b" ]` | `$a -ne $b` | `a != b` |
| Mayor     | `[ "$a" -gt "$b" ]` | `$a -gt $b` | `a > b` |
| Menor     | `[ "$a" -lt "$b" ]` | `$a -lt $b` | `a < b` |

#### **Operadores Lógicos**
Se usan para evaluar múltiples condiciones.

| Operador | Bash | PowerShell | Python |
|---------|------|-----------|--------|
| AND     | `[ "$a" -gt 5 ] && [ "$b" -lt 10 ]` | `$a -gt 5 -and $b -lt 10` | `a > 5 and b < 10` |
| OR      | `[ "$a" -gt 5 ] \|\| [ "$b" -lt 10 ]` | `$a -gt 5 -or $b -lt 10` | `a > 5 or b < 10` |
| NOT     | `[ ! "$a" -eq 5 ]` | `-not ($a -eq 5)` | `not (a == 5)` |

---

### **Ejercicio Práctico 2: Calculadora Simple**

**Objetivo**
- Solicitar dos números al usuario.
- Realizar suma, resta, multiplicación y división.

**Ejemplo en Bash:**
```bash
#!/bin/bash
read -p "Introduce el primer número: " a
read -p "Introduce el segundo número: " b
echo "Suma: $((a + b))"
echo "Resta: $((a - b))"
```

**En PowerShell:**
```powershell
$a = Read-Host "Introduce el primer número"
$b = Read-Host "Introduce el segundo número"
Write-Output "Suma: $($a + $b)"
```

**En Python:**
```python
a = int(input("Introduce el primer número: "))
b = int(input("Introduce el segundo número: "))
print(f"Suma: {a + b}")
```

