**Módulo 2: Control de Flujo y Estructuras de Control**

### **Tema 5: Bucles en Bash, PowerShell y Python**

### **5.1 Introducción a los Bucles**
Los bucles permiten ejecutar un bloque de código repetidamente mientras se cumple una condición o para recorrer estructuras de datos como listas o archivos.

Existen dos tipos principales de bucles en los lenguajes de scripting:

| Tipo | Descripción |
|------|------------|
| **`for`** | Itera sobre una secuencia de valores. |
| **`while`** | Se ejecuta mientras una condición sea verdadera. |

| Lenguaje | Bucle `for` | Bucle `while` |
|----------|------------|--------------|
| **Bash** | `for var in lista; do ... done` | `while [ condición ]; do ... done` |
| **PowerShell** | `foreach ($var in lista) { ... }` | `while (condición) { ... }` |
| **Python** | `for var in lista:` | `while condición:` |


### **5.2 Bucle `for`**
El bucle `for` se usa para iterar sobre elementos de una lista o ejecutar código un número determinado de veces.

#### **Ejemplo 1: Imprimir números del 1 al 5**

**Bash**
```bash
#!/bin/bash
for i in {1..5}; do
    echo "Número: $i"
done
```

**PowerShell**
```powershell
foreach ($i in 1..5) {
    Write-Output "Número: $i"
}
```

**Python**
```python
for i in range(1, 6):
    print(f"Número: {i}")
```

#### **Ejemplo 2: Recorrer una lista de nombres**

**Bash**
```bash
#!/bin/bash
nombres=("David" "Ana" "Carlos")
for nombre in "${nombres[@]}"; do
    echo "Hola, $nombre"
done
```

**PowerShell**
```powershell
$nombres = @("David", "Ana", "Carlos")
foreach ($nombre in $nombres) {
    Write-Output "Hola, $nombre"
}
```

**Python**
```python
nombres = ["David", "Ana", "Carlos"]
for nombre in nombres:
    print(f"Hola, {nombre}")
```

### **5.3 Bucle `while`**
El bucle `while` se ejecuta mientras una condición sea verdadera.

#### **Ejemplo 3: Contador regresivo de 5 a 1**

**Bash**
```bash
#!/bin/bash
contador=5
while [ "$contador" -gt 0 ]; do
    echo "Contando: $contador"
    ((contador--))
done
```

**PowerShell**
```powershell
$contador = 5
while ($contador -gt 0) {
    Write-Output "Contando: $contador"
    $contador--
}
```

**Python**
```python
contador = 5
while contador > 0:
    print(f"Contando: {contador}")
    contador -= 1
```

### **5.4 `break` y `continue`**
- `break`: Termina el bucle inmediatamente.
- `continue`: Salta la iteración actual y sigue con la siguiente.

#### **Ejemplo 4: Detener el bucle cuando se encuentra un número específico (`break`)**

**Bash**
```bash
#!/bin/bash
for i in {1..10}; do
    if [ "$i" -eq 5 ]; then
        break
    fi
    echo "Número: $i"
done
```

**PowerShell**
```powershell
foreach ($i in 1..10) {
    if ($i -eq 5) { break }
    Write-Output "Número: $i"
}
```

**Python**
```python
for i in range(1, 11):
    if i == 5:
        break
    print(f"Número: {i}")
```

#### **Ejemplo 5: Saltar la iteración si el número es par (`continue`)**

**Bash**
```bash
#!/bin/bash
for i in {1..10}; do
    if [ $((i % 2)) -eq 0 ]; then
        continue
    fi
    echo "Número impar: $i"
done
```

**PowerShell**
```powershell
foreach ($i in 1..10) {
    if ($i % 2 -eq 0) { continue }
    Write-Output "Número impar: $i"
}
```

**Python**
```python
for i in range(1, 11):
    if i % 2 == 0:
        continue
    print(f"Número impar: {i}")
```

### **Ejercicio Práctico 6: Tabla de Multiplicar**

#### **Objetivo**
- Pedir al usuario un número.
- Mostrar la tabla de multiplicar del número ingresado hasta el 10.

##### **Solución en Bash**
```bash
#!/bin/bash
read -p "Introduce un número: " num
for i in {1..10}; do
    echo "$num x $i = $((num * i))"
done
```

##### **Solución en PowerShell**
```powershell
$num = Read-Host "Introduce un número"
foreach ($i in 1..10) {
    Write-Output "$num x $i = $($num * $i)"
}
```

##### **Solución en Python**
```python
num = int(input("Introduce un número: "))
for i in range(1, 11):
    print(f"{num} x {i} = {num * i}")
```

### **Ejercicio Práctico 7: Verificar si un Número es Primo**

#### **Objetivo**
- Pedir un número al usuario.
- Determinar si el número es primo.

##### **Solución en Bash**
```bash
#!/bin/bash
read -p "Introduce un número: " num
es_primo=1

for ((i=2; i<num; i++)); do
    if [ $((num % i)) -eq 0 ]; then
        es_primo=0
        break
    fi
done

if [ "$es_primo" -eq 1 ] && [ "$num" -gt 1 ]; then
    echo "$num es primo."
else
    echo "$num no es primo."
fi
```

##### **Solución en PowerShell**
```powershell
$num = Read-Host "Introduce un número"
$es_primo = $true

for ($i=2; $i -lt $num; $i++) {
    if ($num % $i -eq 0) {
        $es_primo = $false
        break
    }
}

if ($es_primo -and $num -gt 1) {
    Write-Output "$num es primo."
} else {
    Write-Output "$num no es primo."
}
```

##### **Solución en Python**
```python
num = int(input("Introduce un número: "))
es_primo = True

for i in range(2, num):
    if num % i == 0:
        es_primo = False
        break

if es_primo and num > 1:
    print(f"{num} es primo.")
else:
    print(f"{num} no es primo.")
```
