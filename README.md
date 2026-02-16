# Tarea #4: Análisis de Algoritmos (CS2013)

**Estudiante:** Sebastian Vir Briceño Gora  
**Institución:** Universidad de Ingeniería y Tecnología (UTEC)  
**Profesor:** José A. Chávez Álvarez  
**Ciclo:** 2026-0

---

## Problema 1: Incrementos Dependientes
### 1. Modelado mediante Doble Sumatoria
Analizamos el comportamiento de los bucles: el externo itera $n$ veces, mientras que el interno tiene un punto de inicio y un tamaño de paso dependientes de la variable de control $i$.
$$T(n) = \sum_{i=1}^{n} \sum_{j=i, \text{ step } i}^{n} 1$$

### 2. Transformación y Cota Superior
Para cada $i$, el bucle interno se ejecuta $\lfloor n/i \rfloor$ veces. Aplicando la propiedad de la función parte entera y factorizando $n$:
$$T(n) = \sum_{i=1}^{n} \lfloor \frac{n}{i} \rfloor \leq n \cdot \sum_{i=1}^{n} \frac{1}{i}$$

### 3. Justificación Rigurosa y Serie Armónica
La sumatoria $\sum_{i=1}^{n} \frac{1}{i}$ define al $n$-ésimo número armónico ($H_n$). Por el **Criterio de Comparación Integral**, sabemos que:
$$H_n = \ln(n) + \gamma + O(1/n)$$
Donde $\gamma$ es la constante de Euler-Mascheroni. Al multiplicar por el factor lineal $n$:
$$T(n) = n \cdot (\ln n + \gamma) \implies O(n \log n)$$

---

## Problema 2: Región Triangular de Cómputo
### 1. Modelo Matemático
$$T(n) = \sum_{i=1}^{n} \sum_{j=1}^{n-i+1} 1 = \sum_{i=1}^{n} (n - i + 1)$$
### 2. Desarrollo mediante Progresión Aritmética
Al expandir los términos para $i=1, 2, \dots, n$, observamos la suma de una progresión aritmética:
$$T(n) = n + (n-1) + (n-2) + \dots + 1$$
Aplicando la fórmula de la **Suma de Gauss**:
$$T(n) = \frac{n(n+1)}{2} = \frac{n^2}{2} + \frac{n}{2}$$
### 3. Complejidad Asintótica
Por la regla de la suma, el término de mayor orden domina el crecimiento:
**Complejidad:** $T(n) \in \Theta(n^2)$.

---

## Problema 3: Carga Decreciente
### 1. Modelado de Operaciones
El algoritmo presenta una reducción geométrica del tamaño del problema en el bucle principal, con un trabajo interno proporcional a dicho tamaño:
$$T(n) = \sum_{k=1}^{\log_2 n} \frac{n}{2^k}$$

### 2. Demostración por Convergencia de Serie
La expresión representa una **Serie Geométrica** de razón $r=1/2$. Probamos mediante el límite superior de la serie infinita:
$$S = n \cdot \sum_{k=1}^{\infty} (\frac{1}{2})^k = n \cdot \left( \frac{1}{1 - 0.5} - 1 \right) = n$$
Incluso sumando el término inicial, la cota superior se mantiene en $2n$.
**Complejidad:** $T(n) \in \Theta(n)$.

---

## Problema 4: Bucles Dependientes (Logarítmicos)
### 1. Análisis del Modelo
El bucle interno realiza una cantidad de operaciones equivalente a $\lfloor \log_2 i \rfloor$:
$$T(n) = \sum_{i=1}^{n} \log_2 i = \log_2(n!)$$
### 2. Aproximación de Stirling
Utilizando la **Fórmula de Stirling** para el logaritmo del factorial:
$$\log_2(n!) = n \log_2 n - n \log_2 e + \Theta(\log n)$$
**Complejidad:** $T(n) \in \Theta(n \log n)$.

---

## Problema 5: Condiciones Dependientes
### 1. Análisis Probabilístico
La condición `(i+j) % 2 == 0` se cumple en exactamente $\lceil i/2 \rceil$ casos para cada iteración del bucle externo.
$$T(n) = \sum_{i=1}^{n} \frac{i}{2} \approx \frac{1}{2} \cdot \frac{n^2}{2}$$
**Complejidad:** $T(n) \in \Theta(n^2)$.

---

## Problema 6: Recurrencia por División
### 1. Definición del Modelo
$$T(n) = T(n/3) + c, \quad T(1) = 1$$
### 2. Expansión Iterativa Paso a Paso
- $T(n) = T(n/3) + c$
- $T(n) = T(n/3^2) + 2c$
- $T(n) = T(n/3^k) + kc$
### 3. Cierre y Profundidad
La recursión termina cuando $n/3^k = 1 \implies k = \log_3 n$.
**Profundidad Máxima:** $\lceil \log_3 n \rceil$.  
**Complejidad:** $O(\log n)$.

---

## Problema 7: Recursión con Trabajo Lineal
### 1. Modelo de Recurrencia
$$T(n) = n \cdot T(n/2) + c$$
### 2. Resolución por Sustitución
Expandiendo los niveles de recursión:
- $T(n) = n T(n/2) + c$
- $T(n) = n [ (n/2) T(n/4) + c ] + c = \frac{n^2}{2} T(n/4) + nc + c$
- $T(n) = \frac{n^3}{8} T(n/8) + \frac{n^2c}{2} + nc + c$
### 3. Conclusión de Complejidad
El factor de ramificación variable $n$ domina la ecuación, superando cualquier cota polinomial.
**Complejidad:** $T(n) = O(n^{\log n})$.