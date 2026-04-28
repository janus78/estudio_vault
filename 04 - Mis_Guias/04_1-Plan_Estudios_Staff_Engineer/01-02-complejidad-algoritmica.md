# 01-02 — Complejidad Algorítmica

> **Prerequisito:** Haber completado [01-01-estructuras-de-datos.md](./01-01-estructuras-de-datos.md) y su checkpoint.  
> **Objetivo de este archivo:** Darte el framework para analizar cualquier algoritmo que encuentres — en entrevistas, en código heredado, en decisiones de arquitectura. No memorizas complejidades. Aprendes a derivarlas.

🆓 **MIT OCW 6.006 — Lecture 1:** Abre esta lecture ahora. Es la introducción formal al modelo de cómputo que usaremos. Ver completa antes de continuar: [https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-fall-2011/](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-fall-2011/)

---

## Por qué la complejidad importa — el argumento real

La complejidad algorítmica no es teoría académica. Es la diferencia entre un sistema que escala y uno que colapsa en producción sin que nadie entienda por qué.

Considera esto:

| Complejidad | n = 100 | n = 1,000 | n = 10,000 | n = 1,000,000 | n = 1,000,000,000 |
|-------------|---------|-----------|------------|---------------|-------------------|
| O(1) | 1 | 1 | 1 | 1 | 1 |
| O(log n) | 7 | 10 | 13 | 20 | 30 |
| O(n) | 100 | 1,000 | 10,000 | 1,000,000 | 1,000,000,000 |
| O(n log n) | 664 | 9,966 | 132,877 | 19,931,569 | 29,897,352,854 |
| O(n²) | 10,000 | 1,000,000 | 100,000,000 | 10¹² | 10¹⁸ |
| O(2ⁿ) | ~10³⁰ | imposible | imposible | imposible | imposible |

Un endpoint que hace una búsqueda O(n²) sobre los datos de un usuario funciona perfectamente con 100 registros en desarrollo. En producción con 10,000 registros por usuario, el tiempo de respuesta es 10,000 veces peor. Si tenías 10ms de latencia en dev, ahora tienes ~28 horas. Eso no es una exageración — es aritmética.

**Escenario real:** Un equipo de backend implementa un sistema de deduplicación de eventos. La implementación inicial usa un `List<string>` con `.Contains()` para verificar si un evento ya fue procesado. O(n) por verificación. Con baja carga (cientos de eventos), todo perfecto. Con carga real (millones de eventos), la CPU llega al 100% y el sistema deja de procesar. El fix: cambiar `List<string>` por `HashSet<string>`. `.Contains()` pasa de O(n) a O(1). La carga del CPU cae al 3%.

Este tipo de problema no aparece en code review porque el código "funciona". Solo un modelo mental de complejidad lo detecta antes de producción.

---

## Notaciones: O, Ω, Θ

Estas tres notaciones describen el comportamiento de un algoritmo según el tamaño de la entrada `n`.

**Big-O (O) — cota superior (peor caso):**

`f(n) = O(g(n))` significa que el crecimiento de `f(n)` no supera el de `g(n)` a partir de algún `n` suficientemente grande. Es el límite superior. Cuando dices "este algoritmo es O(n²)", estás prometiendo que no será peor que cuadrático.

**Big-Omega (Ω) — cota inferior (mejor caso):**

`f(n) = Ω(g(n))` significa que el crecimiento de `f(n)` es al menos tan rápido como `g(n)`. Es el límite inferior. "Este algoritmo es Ω(n)" significa que no puede ser mejor que lineal — siempre necesita al menos n operaciones.

**Big-Theta (Θ) — cota ajustada:**

`f(n) = Θ(g(n))` significa que el algoritmo crece exactamente como `g(n)` — tiene la misma cota superior e inferior. Es el caso donde el mejor y peor caso tienen la misma complejidad. Merge Sort es Θ(n log n) — siempre es n log n, independientemente de los datos.

**¿Por qué en entrevistas casi siempre hablamos de Big-O?**

Porque en práctica nos importa garantizar que el sistema no exceda ciertos límites de rendimiento. El peor caso es lo que garantizamos. Decir "es O(n log n)" es la promesa más fuerte que puedes hacer sobre un algoritmo.

**Operaciones dominantes — por qué ignoramos constantes:**

`O(2n)` es equivalente a `O(n)`. `O(n² + n)` es equivalente a `O(n²)`. En complejidad asintótica, solo importa el término que crece más rápido. Las constantes y los términos menores desaparecen a medida que `n` crece.

```
n = 1,000,000:
  O(n):     1,000,000 operaciones
  O(2n):    2,000,000 operaciones
  O(n²):    1,000,000,000,000 operaciones
  
La diferencia entre O(n) y O(2n): factor de 2.
La diferencia entre O(n) y O(n²): factor de 1,000,000.
```

Las constantes importan en optimización de bajo nivel. Para elegir entre algoritmos, la clase de complejidad es lo que determina el comportamiento a escala.

🆓 **MIT OCW 6.006 — Lecture 2:** Model of computation y definición formal de Big-O. Ver ahora si quieres la versión matemática rigurosa.

---

## Complejidades comunes con ejemplos en C#

### O(1) — Constante

No importa cuánto crezca `n` — el tiempo de ejecución es siempre el mismo.

```csharp
// Acceso a array por índice
int value = array[5]; // O(1) — aritmética de puntero directa

// Dictionary lookup
bool exists = dictionary.ContainsKey("clave"); // O(1) promedio

// Push/Pop en Stack
stack.Push(item); // O(1) amortizado
var top = stack.Pop(); // O(1)

// Peek en Heap — el mínimo siempre está en la raíz
int min = minHeap.Peek(); // O(1)
```

### O(log n) — Logarítmica

Cada paso divide el problema a la mitad. Con 1 millón de elementos, necesitas como máximo 20 pasos.

```csharp
// Binary Search — el ejemplo canónico de O(log n)
int BinarySearch(int[] sortedArray, int target)
{
    int left = 0, right = sortedArray.Length - 1;
    
    while (left <= right)
    {
        int mid = left + (right - left) / 2; // evitar overflow con (left + right) / 2
        
        if (sortedArray[mid] == target) return mid;
        if (sortedArray[mid] < target)
            left = mid + 1;  // Descartar mitad izquierda
        else
            right = mid - 1; // Descartar mitad derecha
    }
    
    return -1; // No encontrado
}

// Cada iteración descarta exactamente la mitad de los elementos restantes.
// Con n = 1,000,000: máximo 20 iteraciones.
// Con n = 1,000,000,000: máximo 30 iteraciones.

// BST balanceado — insert/search
bst.Insert(42); // O(log n) — baja por el árbol descartando mitades
bst.Contains(42); // O(log n)

// PriorityQueue — enqueue/dequeue
priorityQueue.Enqueue("tarea", 5); // O(log n)
priorityQueue.Dequeue(); // O(log n)
```

### O(n) — Lineal

Procesar cada elemento exactamente una vez. El tiempo crece proporcional a la entrada.

```csharp
// Linear search — recorrer hasta encontrar o llegar al final
int LinearSearch(int[] array, int target)
{
    for (int i = 0; i < array.Length; i++) // n iteraciones
    {
        if (array[i] == target) return i;
    }
    return -1;
}

// Calcular suma de todos los elementos
int Sum(int[] array)
{
    int total = 0;
    foreach (int x in array) // n iteraciones
        total += x;
    return total;
}

// Copiar un array
T[] Copy<T>(T[] original)
{
    var copy = new T[original.Length];
    for (int i = 0; i < original.Length; i++) // n iteraciones
        copy[i] = original[i];
    return copy;
}
```

### O(n log n) — Lineal-logarítmica

El "sweet spot" de los buenos algoritmos de sorting. Aparece cuando hay una fase O(log n) que se repite n veces, o cuando divides el problema recursivamente y combinas en O(n).

```csharp
// Array.Sort en .NET — usa Introsort internamente (combinación de QuickSort + HeapSort)
Array.Sort(array); // O(n log n)

// LINQ OrderBy — O(n log n) bajo el capó
var sorted = list.OrderBy(x => x.Name).ToList(); // O(n log n)
```

### O(n²) — Cuadrática

Aparece típicamente con loops anidados donde el loop interno recorre n elementos para cada elemento del loop externo.

```csharp
// Bubble Sort — el ejemplo clásico de O(n²)
void BubbleSort(int[] array)
{
    for (int i = 0; i < array.Length - 1; i++) // n veces
    {
        for (int j = 0; j < array.Length - i - 1; j++) // ~n veces
        {
            if (array[j] > array[j + 1])
                (array[j], array[j + 1]) = (array[j + 1], array[j]);
        }
    }
}

// Encontrar todos los pares duplicados en un array — O(n²) ingenuo
void FindDuplicatePairs(int[] array)
{
    for (int i = 0; i < array.Length; i++)       // n
        for (int j = i + 1; j < array.Length; j++) // n
            if (array[i] == array[j])
                Console.WriteLine($"Par duplicado: {array[i]}");
}
// Versión O(n) con HashSet:
void FindDuplicatePairsOptimized(int[] array)
{
    var seen = new HashSet<int>();
    foreach (int x in array)
    {
        if (seen.Contains(x)) Console.WriteLine($"Duplicado: {x}");
        else seen.Add(x);
    }
}
```

⚠️ **La trampa de O(n²) en producción:** Código que parece inocente puede esconder O(n²). El ejemplo clásico: un loop que llama a `.Contains()` de una `List<T>` en cada iteración. El loop es O(n), `.Contains()` es O(n) → total O(n²). El fix siempre es: pre-construir un `HashSet<T>` antes del loop.

### O(2ⁿ) — Exponencial

Aparece en algoritmos que generan todos los subconjuntos de un conjunto, o en recursión sin memoización donde cada llamada genera 2 sub-llamadas.

```csharp
// Fibonacci recursivo sin memoización — O(2ⁿ)
int FibNaive(int n)
{
    if (n <= 1) return n;
    return FibNaive(n - 1) + FibNaive(n - 2); // Cada llamada genera 2 llamadas
}
// Fib(50) en este algoritmo tarda ~minutos. Fib(100) es astronómicamente más lento.

// Con memoización — O(n)
int FibMemo(int n, Dictionary<int, int>? memo = null)
{
    memo ??= new Dictionary<int, int>();
    if (n <= 1) return n;
    if (memo.ContainsKey(n)) return memo[n];
    
    memo[n] = FibMemo(n - 1, memo) + FibMemo(n - 2, memo);
    return memo[n];
}
```

### O(n!) — Factorial

Aparece en permutaciones. Con n=20, n! ≈ 2.4 × 10¹⁸. Completamente inmanejable en práctica.

```csharp
// Generar todas las permutaciones — O(n!)
void Permutations(int[] array, int start = 0)
{
    if (start == array.Length)
    {
        Console.WriteLine(string.Join(",", array));
        return;
    }
    for (int i = start; i < array.Length; i++)
    {
        (array[start], array[i]) = (array[i], array[start]);
        Permutations(array, start + 1); // n! llamadas en total
        (array[start], array[i]) = (array[i], array[start]);
    }
}
```

---

## Cómo analizar un algoritmo desconocido

Este es el framework que aplicas en una entrevista cuando te muestran código que no has visto antes. Los pasos son siempre los mismos:

**Paso 1 — Identificar operaciones básicas:** ¿Qué hace la operación más elemental en el interior del algoritmo?

**Paso 2 — Identificar loops y su tamaño real:** ¿Cuántas veces se ejecuta cada loop? ¿El loop interno depende del externo?

**Paso 3 — Identificar recursión:** ¿Cuántas sub-llamadas genera cada llamada? ¿Cuánto se reduce el problema en cada nivel?

**Paso 4 — Identificar la operación dominante:** ¿Qué término crece más rápido?

**Paso 5 — Verificar con casos extremos:** ¿Hay un caso donde el algoritmo se comporta mucho peor?

### Ejemplo 1 — Loop simple

```csharp
// ¿Cuál es la complejidad?
void PrintSquares(int n)
{
    for (int i = 1; i <= n; i++) // Paso 2: loop de n iteraciones
        Console.WriteLine(i * i); // Paso 1: operación O(1) por iteración
}
// Paso 4: n × O(1) = O(n)
// Respuesta: O(n)
```

### Ejemplo 2 — Loops anidados con tamaños distintos

```csharp
// ¿Cuál es la complejidad? Cuidado — no es automáticamente O(n²)
void NestedLoops(int n, int m)
{
    for (int i = 0; i < n; i++)    // n iteraciones
        for (int j = 0; j < m; j++) // m iteraciones (independiente de n)
            Console.Write(i * j);
}
// Paso 2: loop externo n veces, loop interno m veces
// Paso 4: O(n × m)
// Si n ≈ m: O(n²). Si m es constante (ej: siempre 10): O(n).
// Respuesta: O(n × m) — los loops anidados son O(n²) SOLO si ambos dependen del mismo n
```

### Ejemplo 3 — Recursión con árbol de llamadas

```csharp
// ¿Cuál es la complejidad de este MergeSort simplificado?
void MergeSort(int[] array, int left, int right)
{
    if (left >= right) return;
    
    int mid = (left + right) / 2;
    MergeSort(array, left, mid);       // Llamada recursiva izquierda
    MergeSort(array, mid + 1, right);  // Llamada recursiva derecha
    Merge(array, left, mid, right);    // O(n) — combinar las dos mitades
}
```

Para analizar recursión, dibuja el árbol de llamadas:

```
Nivel 0:     MergeSort(0, n)          → 1 llamada, combina n elementos
Nivel 1:     MS(0, n/2) + MS(n/2, n)  → 2 llamadas, cada una combina n/2
Nivel 2:     4 llamadas, cada una combina n/4
...
Nivel log n: n llamadas, cada una combina 1 elemento
```

Trabajo total por nivel: n elementos × O(1) para combinar = O(n) por nivel.
Número de niveles: log n (el árbol tiene altura log n).
Total: O(n log n).

---

## Complejidad espacial

El tiempo no es el único recurso limitado. La memoria importa igual — y a veces más.

**Stack space en recursión:**

Cada llamada recursiva agrega un frame al call stack. En recursión sin tail call optimization (C# no tiene TCO), una recursión de profundidad n usa O(n) de stack space.

```csharp
// Esta función recursiva usa O(n) de stack space
int FactorialRecursive(int n)
{
    if (n <= 1) return 1;
    return n * FactorialRecursive(n - 1); // n frames apilados simultáneamente
}

// Esta versión iterativa usa O(1) de stack space
int FactorialIterative(int n)
{
    int result = 1;
    for (int i = 2; i <= n; i++)
        result *= i;
    return result;
}
```

⚠️ **StackOverflowException en producción:** Es el resultado de una recursión demasiado profunda (típicamente >10,000 frames en .NET). El culpable clásico: procesar datos en un árbol o grafo recursivamente sin verificar la profundidad. Para grafos de profundidad arbitraria, siempre usa un stack explícito (iterativo).

**El trade-off tiempo vs espacio — memoización como ejemplo canónico:**

```csharp
// Sin memoización: O(2ⁿ) tiempo, O(n) espacio (stack de recursión)
int FibSlow(int n) => n <= 1 ? n : FibSlow(n-1) + FibSlow(n-2);

// Con memoización: O(n) tiempo, O(n) espacio (tabla de memo)
// Cambiamos tiempo exponencial por espacio lineal — excelente trade-off
int[] _memo = new int[1001];
int FibFast(int n)
{
    if (n <= 1) return n;
    if (_memo[n] != 0) return _memo[n];
    return _memo[n] = FibFast(n-1) + FibFast(n-2);
}

// Bottom-up (tabulation): O(n) tiempo, O(1) espacio si solo necesitamos los últimos 2
int FibOptimal(int n)
{
    if (n <= 1) return n;
    int prev2 = 0, prev1 = 1;
    for (int i = 2; i <= n; i++)
    {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
// Llegamos de O(2ⁿ) tiempo/O(n) espacio → O(n) tiempo/O(n) espacio → O(n) tiempo/O(1) espacio
```

---

## Complejidad amortizada

La complejidad amortizada responde: ¿cuál es el costo promedio de una operación a lo largo de muchas operaciones?

Algunas operaciones son ocasionalmente caras pero raramente. El análisis amortizado distribuye ese costo excepcional entre todas las operaciones que lo rodean.

### El ejemplo canónico — `List<T>.Add()` en .NET

Cuando haces `list.Add(item)`:
- Caso normal (array tiene espacio): O(1) — simplemente escribe en la siguiente posición
- Caso resize (array lleno): O(n) — copia todos los n elementos al nuevo array

¿Cómo puede ser O(1) amortizado si a veces es O(n)?

**Demostración — método agregado (aggregate method):**

Imagina agregar n elementos a un `List<T>` que empieza con capacidad 1. Los resizes ocurren en capacidades 1, 2, 4, 8, 16, 32, ... Cada resize copia k elementos (donde k es la capacidad anterior).

Costo total de todos los resizes:
```
1 + 2 + 4 + 8 + ... + n/2 + n = 2n - 1 ≈ 2n
```

Esta es una serie geométrica. Su suma es aproximadamente 2n.

Costo total de las n operaciones Add (trabajo normal + trabajo de resize):
```
n (copias normales) + 2n (copias de resize) = 3n operaciones
```

Costo amortizado por operación: `3n / n = 3` = O(1).

Por eso el factor de crecimiento 2x (duplicar al hacer resize) es la elección óptima: cualquier factor menor a 2x causaría más resizes frecuentes; mayor a 2x desperdiciaría más memoria. 2x es el equilibrio matemáticamente justificado.

```csharp
// Esto es visible en .NET si inspeccionas la capacidad
var list = new List<int>(); // Capacidad inicial: 0
list.Add(1); Console.WriteLine(list.Capacity); // 4
list.Add(2); list.Add(3); list.Add(4);
list.Add(5); Console.WriteLine(list.Capacity); // 8 — duplicó
// Sigue duplicando: 4 → 8 → 16 → 32 → 64...
```

⚠️ **Implicación práctica:** Si sabes que agregarás 1 millón de elementos, pasa `new List<int>(1_000_000)`. Evitas ~20 resizes y las copias masivas que conllevan. En código de alto rendimiento, la capacidad inicial importa.

---

## Complejidad de sorting algorithms

No todos los algoritmos de sorting son iguales. Esta tabla es referencia permanente:

| Algoritmo | Mejor caso | Caso promedio | Peor caso | Espacio | Estable |
|-----------|------------|---------------|-----------|---------|---------|
| BubbleSort | O(n) | O(n²) | O(n²) | O(1) | Sí |
| SelectionSort | O(n²) | O(n²) | O(n²) | O(1) | No |
| InsertionSort | O(n) | O(n²) | O(n²) | O(1) | Sí |
| MergeSort | O(n log n) | O(n log n) | O(n log n) | O(n) | Sí |
| QuickSort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| HeapSort | O(n log n) | O(n log n) | O(n log n) | O(1) | No |

**Por qué evitar BubbleSort, SelectionSort e InsertionSort en datos grandes:**

Son O(n²) en el caso promedio. Con n = 10,000, son 100 millones de operaciones. MergeSort con los mismos datos: ~133,000 operaciones. La diferencia es de 750x.

**La excepción de InsertionSort:** Es O(n) en el mejor caso (array casi ordenado) y tiene constantes muy bajas. Para arrays pequeños (<10-15 elementos) o datos casi ordenados, InsertionSort gana en práctica. Es por eso que Introsort (el algoritmo de `Array.Sort` en .NET) cambia a InsertionSort para subproblemas pequeños.

**MergeSort — por qué O(n log n) es garantizado:**

```csharp
// Implementación de MergeSort en C# con análisis de complejidad inline
void MergeSort(int[] arr, int left, int right)
{
    if (left >= right) return; // Caso base: 0 o 1 elemento
    
    int mid = left + (right - left) / 2; // Evitar overflow
    
    MergeSort(arr, left, mid);       // Recursión izquierda — T(n/2)
    MergeSort(arr, mid + 1, right);  // Recursión derecha — T(n/2)
    
    Merge(arr, left, mid, right);    // O(n) — esta es la clave
}

// La fase Merge es O(n) — combina dos arrays ordenados en uno
void Merge(int[] arr, int left, int mid, int right)
{
    // Copiar las dos mitades a arrays temporales
    int leftSize = mid - left + 1;
    int rightSize = right - mid;
    
    int[] leftArr = new int[leftSize];
    int[] rightArr = new int[rightSize];
    
    Array.Copy(arr, left, leftArr, 0, leftSize);
    Array.Copy(arr, mid + 1, rightArr, 0, rightSize);
    
    // Merge — comparar elementos de ambas mitades, insertar el menor
    int i = 0, j = 0, k = left;
    while (i < leftSize && j < rightSize)
    {
        if (leftArr[i] <= rightArr[j])
            arr[k++] = leftArr[i++];
        else
            arr[k++] = rightArr[j++];
    }
    
    // Copiar lo que sobró
    while (i < leftSize) arr[k++] = leftArr[i++];
    while (j < rightSize) arr[k++] = rightArr[j++];
}

// Análisis:
// T(n) = 2T(n/2) + O(n)  ← recurrencia del MergeSort
// Por el Master Theorem: T(n) = O(n log n)
// El "log n" viene de los niveles de recursión (n → n/2 → n/4 → ... → 1)
// El "n" viene de la fase Merge en cada nivel
```

**Costo de MergeSort: O(n) de espacio auxiliar.** Los arrays temporales en la fase Merge suman O(n) de espacio. Esto es el principal desventaja vs HeapSort.

**QuickSort — por qué es O(n²) en peor caso pero se usa de todas formas:**

El peor caso de QuickSort ocurre cuando el pivot siempre divide el array de forma completamente desbalanceada (ej: array ya ordenado con pivot en el primer elemento). En ese caso, cada recursión reduce el problema en solo 1 elemento → O(n²).

Por qué se usa de todas formas:
1. En datos aleatorios, el caso promedio es O(n log n) con constantes más bajas que MergeSort
2. Es in-place (O(log n) de stack space vs O(n) de MergeSort)
3. Con una buena estrategia de selección de pivot (median-of-3, pivot aleatorio), el peor caso se vuelve extremadamente improbable

`Array.Sort` en .NET usa Introsort: empieza con QuickSort, pero si detecta que la recursión se vuelve demasiado profunda (señal de peor caso), cambia a HeapSort garantizando O(n log n) en todos los casos.

---

## Complejidad de colecciones de .NET — desmitificado

Esta sección conecta lo teórico con tu stack diario. Asegúrate de poder justificar cada complejidad.

### `List<T>`

| Operación | Complejidad | Justificación |
|-----------|-------------|---------------|
| `list[i]` | O(1) | Acceso directo al array interno — aritmética de puntero |
| `list.Add(item)` al final | O(1) amortizado | O(n) ocasionalmente cuando hace resize |
| `list.Insert(i, item)` | O(n) | Desplaza todos los elementos posteriores a i |
| `list.Remove(item)` | O(n) | Busca en O(n) + desplaza en O(n) |
| `list.RemoveAt(i)` | O(n) | Desplaza todos los elementos posteriores a i |
| `list.Contains(item)` | O(n) | Búsqueda lineal — sin hash, sin orden |
| `list.Count` | O(1) | Campo privado, se actualiza en cada Add/Remove |

### `Dictionary<K,V>`

| Operación | Complejidad promedio | Complejidad peor caso | Cuándo ocurre el peor caso |
|-----------|---------------------|----------------------|---------------------------|
| `dict[key]` | O(1) | O(n) | Todas las keys tienen el mismo hash |
| `dict.ContainsKey(key)` | O(1) | O(n) | Idem |
| `dict.Add(key, value)` | O(1) amortizado | O(n) | Resize + rehash cuando load factor > 0.72 |
| `dict.Remove(key)` | O(1) | O(n) | Idem |

### `HashSet<T>`

Misma complejidad que `Dictionary<K,V>` — es esencialmente un Dictionary sin valores.

**¿Cuándo `HashSet<T>` en lugar de `List<T>`?** Cuando necesitas verificar existencia frecuentemente. `hashSet.Contains(item)` es O(1). `list.Contains(item)` es O(n). Si tu código tiene `.Contains()` en un loop, estás pagando O(n²) cuando podría ser O(n).

### `SortedDictionary<K,V>` vs `SortedList<K,V>`

| Característica | SortedDictionary | SortedList |
|----------------|-----------------|------------|
| Estructura interna | Red-Black Tree | Array ordenado |
| Lookup | O(log n) | O(log n) via binary search |
| Insert | O(log n) | O(n) — desplaza elementos |
| Delete | O(log n) | O(n) — desplaza elementos |
| Iteración | O(n) — in-order del árbol | O(n) — array contiguo (mejor caché) |
| Uso de memoria | Mayor (punteros del árbol) | Menor (array contiguo) |

**Cuándo usar cuál:**
- `SortedDictionary`: Muchas inserciones/eliminaciones + necesitas orden
- `SortedList`: Pocos cambios, muchas lecturas secuenciales, memoria importa

### `LinkedList<T>`

| Operación | Complejidad | Nota |
|-----------|-------------|------|
| `AddFirst/AddLast` | O(1) | Tiene referencias a head y tail |
| `AddAfter(node, item)` | O(1) | Si ya tienes el `LinkedListNode<T>` |
| `Remove(item)` | O(n) | Tiene que buscar el nodo |
| `Find(value)` | O(n) | Recorrido secuencial |
| `list[i]` | N/A | No existe — LinkedList no tiene indexer |

💡 **El patrón correcto para `LinkedList<T>` en producción:**

```csharp
// INCORRECTO — pierdes todo el beneficio de O(1)
linkedList.Remove(value); // O(n) para encontrar + O(1) para eliminar

// CORRECTO — guardar referencia al nodo para eliminación O(1)
var nodeRef = linkedList.AddLast(value); // Guardar el nodo retornado
// ... más tarde:
linkedList.Remove(nodeRef); // O(1) — ya tienes la referencia directa
```

---

## Patrones de complejidad para detectar en code review

Estos son los anti-patrones más comunes que transforman código O(n) en O(n²) sin que nadie lo note:

```csharp
// ❌ ANTI-PATRÓN 1: .Contains() en List dentro de un loop — O(n²)
var processed = new List<string>();
foreach (var item in items) // O(n)
{
    if (!processed.Contains(item)) // O(n) — aquí está el problema
        processed.Add(item);
}

// ✅ FIX: HashSet — O(n)
var processed = new HashSet<string>();
foreach (var item in items)
    processed.Add(item); // Add retorna false si ya existe — O(1)


// ❌ ANTI-PATRÓN 2: LINQ .Where() anidado — O(n²)
var result = listA.Where(a => listB.Any(b => b.Id == a.Id)).ToList();
// listB.Any() es O(n) para cada elemento de listA → O(n²) total

// ✅ FIX: Pre-construir un HashSet de IDs — O(n)
var bIds = new HashSet<int>(listB.Select(b => b.Id)); // O(n) una vez
var result = listA.Where(a => bIds.Contains(a.Id)).ToList(); // O(1) por elemento


// ❌ ANTI-PATRÓN 3: String concatenation en loop — O(n²) en espacio y tiempo
string result = "";
foreach (var item in items)
    result += item + ","; // Cada += crea un nuevo string — O(total length)

// ✅ FIX: StringBuilder — O(n)
var sb = new StringBuilder();
foreach (var item in items)
    sb.Append(item).Append(',');
string result = sb.ToString();
```

---

> **🏁 Checkpoint:** Antes de avanzar, debes poder:
> - Calcular la complejidad Big-O de cualquier función C# con loops simples, anidados y recursión básica — sin ayuda
> - Demostrar matemáticamente por qué `List<T>.Add()` es O(1) amortizado
> - Identificar los 3 anti-patrones de complejidad en code review sin que te los señalen
> - Decir de memoria la complejidad de las operaciones principales de `List<T>`, `Dictionary<K,V>`, `HashSet<T>`, `SortedDictionary<K,V>` y `LinkedList<T>`
>
> **Siguiente archivo:** [01-03-memoria-y-gestion.md →](./01-03-memoria-y-gestion.md)
