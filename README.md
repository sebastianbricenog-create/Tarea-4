# Tarea #4: Análisis de Algoritmos (CS2013)

**Estudiante:** Sebastian Vir Briceño Gora  
**Institución:** Universidad de Ingeniería y Tecnología (UTEC)  
**Profesor:** José A. Chávez Álvarez  
**Ciclo:** 2026-0

---

## Problema 1: Análisis de Incrementos Dependientes
### 1. Modelado mediante Doble Sumatoria
Para cuantificar el número total de operaciones elementales ($count++$), analizamos los límites de los bucles anidados. El bucle externo recorre todos los enteros desde $1$ hasta $n$. Para cada valor fijo de $i$, el bucle interno comienza en $i$ y avanza en pasos de magnitud $i$ hasta alcanzar $n$.

Matemáticamente, esto se expresa como:
$$T(n) = \sum_{i=1}^{n} \sum_{j=i, \text{ paso } i}^{n} 1$$

### 2. Simplificación y Cota Superior
Observamos que para un $i$ determinado, la cantidad de iteraciones del bucle interno es exactamente $\lfloor n/i \rfloor$. Aplicando la propiedad de la función piso ($\lfloor x \rfloor \leq x$), establecemos la cota superior:
$$T(n) = \sum_{i=1}^{n} \left\lfloor \frac{n}{i} \right\rfloor \leq \sum_{i=1}^{n} \frac{n}{i}$$

Factorizando la constante $n$ fuera del sumatorio:
$$T(n) \leq n \cdot \sum_{i=1}^{n} \frac{1}{i}$$

### 3. Rol de la Serie Armónica y Justificación de $O(n \log n)$
La expresión resultante $\sum_{i=1}^{n} \frac{1}{i}$ define la **Serie Armónica ($H_n$)**. Por teoría de análisis asintótico y el criterio de comparación integral, sabemos que la suma armónica crece logarítmicamente:
$$H_n = \int_{1}^{n} \frac{1}{x} dx + O(1) = \ln(n) + \gamma + O\left(\frac{1}{n}\right)$$
Sustituyendo esta aproximación en nuestro modelo:
$$T(n) \leq n \cdot (\ln n + \gamma) \implies T(n) = O(n \log n)$$

---

## Problema 2: Región Triangular de Cómputo
### 1. Modelado Matemático
El bucle interno tiene un límite superior que decrece linealmente conforme $i$ aumenta. El número total de incrementos es:
$$T(n) = \sum_{i=1}^{n} \sum_{j=1}^{n-i+1} 1 = \sum_{i=1}^{n} (n - i + 1)$$

### 2. Desarrollo de la Sumatoria (Suma de Gauss)
Expandiendo los términos para visualizar la progresión:
- Para $i=1 \implies n$ iteraciones
- Para $i=2 \implies n-1$ iteraciones
- ...
- Para $i=n \implies 1$ iteración
  Esto conforma la suma de los primeros $n$ números naturales:
  $$T(n) = n + (n-1) + \dots + 1 = \frac{n(n+1)}{2} = \frac{n^2}{2} + \frac{n}{2}$$

### 3. Conclusión de Complejidad
Aplicando el límite cuando $n \to \infty$ y eliminando términos de menor orden:
**Complejidad:** $T(n) \in \Theta(n^2)$.

---

## Problema 3: Algoritmo de Carga Decreciente
### 1. Modelado de Operaciones
En este algoritmo, $i$ se reduce a la mitad en cada paso del `while`, y el bucle `for` ejecuta $i$ operaciones. El modelo resultante es:
$$T(n) = \sum_{k=1}^{\log_2 n} \frac{n}{2^k}$$

### 2. Demostración por Inducción
**Objetivo:** Probar que $\sum_{k=0}^{\infty} \frac{n}{2^k} \leq 2n$.
- **Base ($n=1$):** $\frac{1}{2^0} = 1 \leq 2(1)$. (Cumple).
- **Hipótesis:** Asumimos que la propiedad se mantiene para un valor $n$.
- **Paso Inductivo:** La sumatoria es una **Serie Geométrica** de razón $r=1/2$. La suma infinita converge a:
  $$S = n \left( \frac{1}{1 - 1/2} \right) = n \left( \frac{1}{0.5} \right) = 2n$$
  Dado que nuestra serie es finita, el valor siempre será estrictamente menor o igual a la cota teórica de $2n$.

### 3. Complejidad Final
Al estar acotado superiormente por una función lineal:
**Complejidad:** $T(n) \in \Theta(n)$.

---

## Problema 6: Recurrencia por División
### 1. Definición del Modelo
La relación de recurrencia para el algoritmo dado es:
$$T(n) = T(n/3) + c, \text{ con } T(1) = 1$$

### 2. Resolución por Expansión Iterativa
- $T(n) = T(n/3) + c$
- $T(n) = [T(n/9) + c] + c = T(n/3^2) + 2c$
- $T(n) = T(n/3^k) + kc$

### 3. Profundidad y Complejidad
La recursión alcanza su base cuando $\frac{n}{3^k} = 1$, lo que implica $k = \log_3 n$.
Sustituyendo el valor de $k$ en la ecuación general:
$$T(n) = T(1) + c \log_3 n = 1 + c \log_3 n$$
**Complejidad Final:** $O(\log n)$.  
**Profundidad Máxima:** $\lceil \log_3 n \rceil$.

---

## Problema 7: Recursión con Trabajo Lineal
### 1. Recurrencia Obtenida
El algoritmo realiza $n$ llamadas recursivas, cada una con un tamaño de problema reducido a la mitad ($n/2$):
$$T(n) = n \cdot T(n/2) + c$$

### 2. Análisis por Expansión
- $T(n) = n \cdot T(n/2) + c$
- $T(n) = n \left[ \frac{n}{2} T(n/4) + c \right] + c = \frac{n^2}{2} T(n/4) + nc + c$
- $T(n) = \frac{n^3}{8} T(n/8) + \frac{n^2c}{2} + nc + c$

### 3. Determinación de Complejidad
Observamos que el factor de ramificación variable $n$ domina el crecimiento exponencial de la función. Al no ser un valor constante, el crecimiento supera cualquier cota polinomial estándar.  
**Complejidad Final:** $T(n) = O(n^{\log n})$.