# 02-01 — Patrones Lineales

> **Prerequisito:** Haber leído [02-00-overview.md](./02-00-overview.md)  
> **Principio de este archivo:** No memorizas soluciones — aprendes a reconocer señales. Cada patrón tiene una firma en el enunciado. Entrenar el ojo para verla vale más que resolver 100 problemas sin ese entrenamiento.

🎯 **Antes de empezar:** Abre AlgoMonster, navega a la sección "Two Pointers" y lee la introducción del patrón. Regresa aquí para la explicación profunda y los templates en C#. La combinación de ambos es más efectiva que cualquiera por sí solo.

---

## Patrón 1 — Two Pointers

### 1. Intuición

Imagina que tienes un array ordenado y necesitas encontrar dos números que sumen un target. El enfoque naive: probar cada par posible — O(n²) en tiempo. Pero hay una propiedad que el array ordenado te da gratis: si el par actual suma demasiado, necesitas un número más pequeño; si suma muy poco, necesitas uno más grande. Esa propiedad te permite eliminar opciones sin revisarlas — eso es exactamente lo que hacen dos punteros.

Un puntero en el extremo izquierdo (el más pequeño disponible), uno en el extremo derecho (el más grande disponible). Evalúas la condición. Si no se cumple, mueves el puntero que tiene más información para darte — el del extremo equivocado. El espacio de búsqueda se reduce a la mitad en cada paso. O(n) total.

La misma idea aplica en la variante "same direction": un puntero lento que marca dónde escribir, uno rápido que explora. Útil para remover duplicados in-place sin memoria extra.

### 2. Señales de reconocimiento

Lee el enunciado y busca:
- Array **ordenado** (la palabra "sorted" en LeetCode, o "ascending/descending" en la descripción)
- Buscar **par, tripla, o subset** que cumpla condición numérica (suma, diferencia, producto)
- Palabras como "dos extremos", "mínimo", "máximo", o "suma target"
- **Remover duplicados** o elementos específicos in-place
- "Contenedor" o área entre dos líneas (Container With Most Water)

⚠️ **Señal que confunde:** Ver "array" y asumir Two Pointers. El array debe ser ordenado (o el orden no importa pero el criterio de movimiento de punteros es claro) para que Two Pointers tenga ventaja sobre fuerza bruta.

### 3. Templates en C#

```csharp
// ─── VARIANTE 1: Opposite direction (converging) ───
// Uso: array ordenado, buscar par/tripla, Container with Most Water
int left = 0, right = nums.Length - 1;

while (left < right)
{
    int sum = nums[left] + nums[right];

    if (sum == target)
    {
        // Condición cumplida — procesar y decidir si continuar o return
        return new[] { left, right };
    }
    else if (sum < target)
    {
        left++;  // Necesitamos un número mayor → avanzar izquierda
    }
    else
    {
        right--; // Necesitamos un número menor → retroceder derecha
    }
}

// ─── VARIANTE 2: Same direction (fast/slow sobre array) ───
// Uso: remover duplicados/elementos, filtrar in-place
// slow = puntero de escritura, fast = puntero de exploración
int slow = 0;

for (int fast = 0; fast < nums.Length; fast++)
{
    if (nums[fast] != /* condición de exclusión */)
    {
        nums[slow] = nums[fast]; // Escribir en la posición slow
        slow++;
    }
    // Si la condición NO se cumple, fast avanza pero slow no
    // Efectivamente "salta" el elemento no deseado
}
// slow es el nuevo Length del array resultante
return slow;
```

### 4. Problemas representativos

**Problema 1: Two Sum II — Input Array Is Sorted (LeetCode 167)**

Input: array ordenado, target. Output: índices [1-indexed] de los dos números que suman target. Garantizado que existe exactamente una solución.

```csharp
public int[] TwoSum(int[] numbers, int target)
{
    int left = 0, right = numbers.Length - 1;

    while (left < right)
    {
        int sum = numbers[left] + numbers[right];

        if (sum == target)
            return new[] { left + 1, right + 1 }; // 1-indexed
        else if (sum < target)
            left++;   // El par suma muy poco → necesitamos número mayor
        else
            right--;  // El par suma demasiado → necesitamos número menor
    }

    return Array.Empty<int>(); // Nunca llega aquí por garantía del problema
}
// Tiempo: O(n) | Espacio: O(1)
```

La clave del "por qué funciona": el array ordenado garantiza que al mover `left` hacia la derecha, el próximo elemento siempre es mayor o igual. Al mover `right` hacia la izquierda, siempre es menor o igual. Nunca descartamos un par válido.

---

**Problema 2: Container With Most Water (LeetCode 11)**

Input: array de alturas de barras. Output: máxima cantidad de agua entre cualquier par de barras.

El agua entre las barras `i` y `j` es `min(height[i], height[j]) × (j - i)`.

La intuición: si empezamos con el contenedor más ancho posible (punteros en los extremos), la única forma de potencialmente aumentar el agua es mover el puntero del lado MÁS BAJO — porque mover el puntero del lado más alto solo puede empeorar o mantener la altura del mínimo, y además reduce el ancho.

```csharp
public int MaxArea(int[] height)
{
    int left = 0, right = height.Length - 1;
    int maxWater = 0;

    while (left < right)
    {
        // Agua = base × altura mínima
        int water = (right - left) * Math.Min(height[left], height[right]);
        maxWater = Math.Max(maxWater, water);

        // Mover el puntero del lado más bajo
        // Razonamiento: el lado más alto no puede mejorar el mínimo si nos quedamos
        // solo puede mantenerse igual o empeorar — el único candidato a mejorar es el más bajo
        if (height[left] < height[right])
            left++;
        else
            right--;
    }

    return maxWater;
}
// Tiempo: O(n) | Espacio: O(1)
```

---

**Problema 3: Remove Duplicates from Sorted Array (LeetCode 26)**

Input: array ordenado con duplicados. Output: cantidad de elementos únicos; modificar el array in-place para que los primeros k elementos sean los únicos.

```csharp
public int RemoveDuplicates(int[] nums)
{
    if (nums.Length == 0) return 0;

    int slow = 0; // Puntero de escritura — marca la última posición única escrita

    for (int fast = 1; fast < nums.Length; fast++)
    {
        // Solo avanzamos slow cuando encontramos un elemento diferente al último único
        if (nums[fast] != nums[slow])
        {
            slow++;
            nums[slow] = nums[fast]; // Escribir el nuevo único en la siguiente posición
        }
        // Si nums[fast] == nums[slow] → duplicado, fast avanza, slow no
    }

    return slow + 1; // Cantidad de elementos únicos (slow es índice 0-based)
}
// Tiempo: O(n) | Espacio: O(1) — la clave del patrón same-direction
```

### 5. Cuándo NO aplica

- Array **no ordenado** — a menos que ordenar sea parte válida de la solución (verificar que el sort no cambie el problema: O(n log n) + O(n) sigue siendo O(n log n))
- Cuando necesitas acceder a **más de dos posiciones** simultáneamente (considera Sliding Window o Hash Map)
- Cuando el problema pide subsets **no contiguos** (Sliding Window asume contiguidad; Two Pointers también — si el orden no importa, considera Hash Map)
- Problemas de **árboles o grafos** — el concepto de "puntero" no aplica directamente a estructuras no lineales

🎯 **AlgoMonster:** Completa los problemas guiados de "Two Pointers" antes de avanzar. El sistema de repetición espaciada te mostrará las líneas donde cometiste errores.  
🆓 **NeetCode 150:** Resuelve los problemas del grupo "Two Pointers" en NeetCode.io después de AlgoMonster.

---

## Patrón 2 — Sliding Window

### 1. Intuición

Tienes un array y necesitas encontrar el subarray de longitud k con la mayor suma. El enfoque naive: para cada posición i, sumar los k elementos a partir de i — O(n×k). Pero hay redundancia masiva: la ventana de i a i+k-1 comparte k-1 elementos con la ventana de i+1 a i+k. En lugar de recalcular toda la suma, ¿qué pasa si simplemente agregas el nuevo elemento y quitas el que salió? Una suma, una resta. O(n) total.

Ese principio — mantener un estado que se actualiza incrementalmente en lugar de recalcularse desde cero — es Sliding Window.

La variante de **ventana variable** expande el extremo derecho continuamente. Cuando la ventana viola una condición (tiene demasiados caracteres únicos, la suma supera el límite), contrae el extremo izquierdo hasta que la ventana vuelve a ser válida. El resultado óptimo está en algún punto de esa expansión.

### 2. Señales de reconocimiento

- "**Subarray** o **substring contiguo**" — la palabra "contiguo" o "consecutive" es la señal más fuerte
- "**Tamaño k**" — ventana fija
- "**Suma máxima/mínima** en subarray de longitud k"
- "**Substring más largo/corto** que cumple condición X"
- "Sin caracteres repetidos", "a lo más k caracteres distintos" — ventana variable
- Cualquier problema donde el resultado es sobre una **secuencia continua con agregación**

⚠️ **Señal falsa:** "Máxima suma de k elementos" — si los elementos no necesitan ser contiguos, esto es Top K (Heap), no Sliding Window.

### 3. Templates en C#

```csharp
// ─── VARIANTE 1: Ventana fija de tamaño k ───
public double FindMaxAverage(int[] nums, int k)
{
    // Inicializar la primera ventana
    int windowSum = 0;
    for (int i = 0; i < k; i++)
        windowSum += nums[i];

    int maxSum = windowSum;

    // Deslizar: agregar el nuevo elemento, quitar el que salió
    for (int i = k; i < nums.Length; i++)
    {
        windowSum += nums[i] - nums[i - k]; // Agregar nums[i], remover nums[i-k]
        maxSum = Math.Max(maxSum, windowSum);
    }

    return (double)maxSum / k;
}

// ─── VARIANTE 2: Ventana variable (expand/contract) ───
// Estado de la ventana puede ser: suma, count, Dictionary de frecuencias, HashSet
int left = 0;
var windowState = new Dictionary<char, int>(); // Ejemplo: frecuencia de caracteres
int result = 0;

for (int right = 0; right < s.Length; right++)
{
    // EXPANDIR: agregar s[right] al estado de la ventana
    windowState[s[right]] = windowState.GetValueOrDefault(s[right], 0) + 1;

    // CONTRAER: mientras la ventana sea inválida, mover left
    while (/* ventana inválida: ej. windowState.Count > k */)
    {
        // Remover s[left] del estado
        windowState[s[left]]--;
        if (windowState[s[left]] == 0)
            windowState.Remove(s[left]);
        left++;
    }

    // La ventana [left, right] es válida aquí — actualizar resultado
    result = Math.Max(result, right - left + 1);
}
```

### 4. Problemas representativos

**Problema 1: Maximum Average Subarray I (LeetCode 643)**

Input: array de enteros, entero k. Output: promedio máximo de subarray de longitud exactamente k.

```csharp
public double FindMaxAverage(int[] nums, int k)
{
    // Suma de la primera ventana
    int windowSum = 0;
    for (int i = 0; i < k; i++)
        windowSum += nums[i];

    int maxSum = windowSum;

    for (int i = k; i < nums.Length; i++)
    {
        // Suma incremental: entra nums[i], sale nums[i-k]
        windowSum += nums[i] - nums[i - k];
        maxSum = Math.Max(maxSum, windowSum);
    }

    return (double)maxSum / k;
}
// Tiempo: O(n) | Espacio: O(1)
```

---

**Problema 2: Longest Substring Without Repeating Characters (LeetCode 3)**

Input: string. Output: longitud del substring más largo sin caracteres repetidos.

El estado de la ventana es un HashSet de caracteres actuales. La ventana es inválida cuando el nuevo carácter ya existe en el set (habría repetición).

```csharp
public int LengthOfLongestSubstring(string s)
{
    var charSet = new HashSet<char>(); // Estado: caracteres en la ventana actual
    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.Length; right++)
    {
        // Contraer ventana hasta que s[right] no esté en el set
        while (charSet.Contains(s[right]))
        {
            charSet.Remove(s[left]);
            left++;
        }

        // s[right] ya no está en el set — agregar y actualizar máximo
        charSet.Add(s[right]);
        maxLength = Math.Max(maxLength, right - left + 1);
    }

    return maxLength;
}
// Tiempo: O(n) | Espacio: O(min(n, charset_size))
```

⚠️ **Optimización:** Puedes usar un `Dictionary<char, int>` que guarda el último índice de cada carácter para saltar `left` directamente en lugar de moverlo uno por uno. Esto mantiene O(n) pero reduce las iteraciones del while en práctica.

---

**Problema 3: Minimum Window Substring (LeetCode 76) — Hard**

Input: strings `s` y `t`. Output: substring más corto de `s` que contiene todos los caracteres de `t`.

El estado de la ventana requiere rastrear cuántos caracteres de `t` están "satisfechos" dentro de la ventana — no solo presentes, sino con la frecuencia correcta.

```csharp
public string MinWindow(string s, string t)
{
    if (s.Length < t.Length) return "";

    // Frecuencia requerida de cada carácter en t
    var required = new Dictionary<char, int>();
    foreach (char c in t)
        required[c] = required.GetValueOrDefault(c, 0) + 1;

    // Estado actual de la ventana
    var windowFreq = new Dictionary<char, int>();
    int satisfied = 0;            // Cuántos caracteres de t están satisfechos
    int totalRequired = required.Count; // Cuántos tipos de caracteres necesitamos satisfacer

    int left = 0;
    int minLen = int.MaxValue;
    int minLeft = 0;

    for (int right = 0; right < s.Length; right++)
    {
        char c = s[right];
        windowFreq[c] = windowFreq.GetValueOrDefault(c, 0) + 1;

        // ¿Este carácter satisface el requerimiento de t?
        if (required.ContainsKey(c) && windowFreq[c] == required[c])
            satisfied++;

        // Contraer ventana mientras sea válida (todos los requerimientos satisfechos)
        while (satisfied == totalRequired)
        {
            // Esta ventana es válida — registrar si es la más corta
            int windowLen = right - left + 1;
            if (windowLen < minLen)
            {
                minLen = windowLen;
                minLeft = left;
            }

            // Contraer: remover s[left]
            char leftChar = s[left];
            windowFreq[leftChar]--;
            if (required.ContainsKey(leftChar) && windowFreq[leftChar] < required[leftChar])
                satisfied--; // Ya no satisfacemos este carácter
            left++;
        }
    }

    return minLen == int.MaxValue ? "" : s.Substring(minLeft, minLen);
}
// Tiempo: O(|s| + |t|) | Espacio: O(|s| + |t|) por los diccionarios
```

### 5. Cuándo NO aplica

- Elementos **no contiguos** — si el problema pide subsets en cualquier posición, considera Two Pointers o Hash Map
- Cuando el **orden dentro de la ventana importa** para la condición (generalmente indica Stack o Deque)
- Cuando la condición de validez de la ventana depende de **posiciones fuera de la ventana** — la contracción del left no puede asumir que solo importa lo que está dentro
- Arrays con **números negativos** donde necesitas máxima suma — Sliding Window no funciona (una ventana menor puede ser mayor, no puedes contraer linealmente). Usa Kadane's algorithm.

🎯 **AlgoMonster:** Sección "Sliding Window" — completa los problemas guiados en orden. Hay variantes importantes de ventana variable que el sistema de repetición espaciada consolida.  
🆓 **NeetCode 150:** Grupo "Sliding Window" — incluye Minimum Window Substring como el caso más complejo del patrón.

---

## Patrón 3 — Fast & Slow Pointers

### 1. Intuición

Si dos corredores corren en un círculo, el más rápido eventualmente alcanzará al más lento — siempre. Esa propiedad matemática es la esencia de Fast & Slow Pointers.

En una linked list con ciclo: el puntero rápido avanza 2 nodos por paso, el lento avanza 1. Si hay ciclo, eventualmente se encuentran dentro del ciclo. Si no hay ciclo, el rápido llega a `null`. Detección de ciclo en O(n) con O(1) de espacio — sin HashSet, sin marcar nodos.

La segunda aplicación: encontrar el nodo medio de una linked list. Cuando el puntero rápido llega al final (o `null`), el lento está exactamente a la mitad. Sin necesidad de recorrer primero para contar.

La propiedad matemática del punto de encuentro (Floyd's Algorithm): después de detectar el ciclo, si mueves uno de los punteros al inicio y avanzas ambos a velocidad 1, se encuentran exactamente en el inicio del ciclo. No necesitas saber el tamaño del ciclo.

### 2. Señales de reconocimiento

- "**Detectar ciclo**" en linked list — la señal más directa
- "**Punto de inicio del ciclo**" — extensión del patrón
- "**Nodo medio**" de linked list sin conocer el tamaño
- "**Happy number**" — secuencia que eventualmente forma ciclo
- "**Palíndromo**" en linked list (combina Fast & Slow con In-place Reversal)
- Cualquier problema donde necesitas detectar si una secuencia "regresa a un estado anterior"

### 3. Template en C#

```csharp
// ─── Clase auxiliar (asumida en todos los problemas de linked list) ───
public class ListNode
{
    public int Val;
    public ListNode? Next;
    public ListNode(int val = 0, ListNode? next = null) { Val = val; Next = next; }
}

// ─── Detección de ciclo (Floyd's Cycle Detection) ───
public bool HasCycle(ListNode? head)
{
    ListNode? slow = head;
    ListNode? fast = head;

    while (fast != null && fast.Next != null)
    {
        slow = slow!.Next;         // Avanza 1
        fast = fast.Next.Next;     // Avanza 2

        if (slow == fast)          // Se encontraron → hay ciclo
            return true;
    }
    return false; // fast llegó a null → no hay ciclo
}

// ─── Encontrar inicio del ciclo ───
public ListNode? DetectCycle(ListNode? head)
{
    ListNode? slow = head;
    ListNode? fast = head;

    // Fase 1: detectar si hay ciclo
    while (fast != null && fast.Next != null)
    {
        slow = slow!.Next;
        fast = fast.Next.Next;
        if (slow == fast) break; // Se encontraron
    }

    // No hay ciclo
    if (fast == null || fast.Next == null) return null;

    // Fase 2: encontrar el inicio del ciclo
    // Propiedad matemática de Floyd: mover uno al inicio y avanzar ambos a velocidad 1
    // Se encuentran exactamente en el inicio del ciclo
    slow = head;
    while (slow != fast)
    {
        slow = slow!.Next;
        fast = fast!.Next;
    }
    return slow; // inicio del ciclo
}

// ─── Encontrar nodo medio ───
public ListNode? FindMiddle(ListNode? head)
{
    ListNode? slow = head;
    ListNode? fast = head;

    while (fast != null && fast.Next != null)
    {
        slow = slow!.Next;
        fast = fast.Next.Next;
    }
    // Cuando fast llegó al final (o null), slow está en el medio
    // Para n par: slow apunta al segundo de los dos nodos centrales
    return slow;
}
```

### 4. Problemas representativos

**Problema 1: Linked List Cycle (LeetCode 141)**

```csharp
public bool HasCycle(ListNode? head)
{
    ListNode? slow = head, fast = head;

    while (fast?.Next != null)
    {
        slow = slow!.Next;
        fast = fast.Next.Next;
        if (slow == fast) return true;
    }
    return false;
}
// Tiempo: O(n) | Espacio: O(1)
// Alternativa naive: HashSet para rastrear nodos visitados → O(n) tiempo, O(n) espacio
// Por qué Fast & Slow es mejor: mismo tiempo, espacio constante
```

---

**Problema 2: Middle of the Linked List (LeetCode 876)**

```csharp
public ListNode MiddleNode(ListNode head)
{
    ListNode? slow = head, fast = head;

    while (fast?.Next != null)
    {
        slow = slow!.Next;
        fast = fast.Next.Next;
    }
    return slow!;
    // Para lista de longitud par (ej. 4 nodos): devuelve el segundo nodo central (índice 2)
    // Para lista de longitud impar (ej. 5 nodos): devuelve el nodo central exacto (índice 2)
}
// Tiempo: O(n) | Espacio: O(1)
```

---

**Problema 3: Happy Number (LeetCode 202)**

Un número es "happy" si al reemplazarlo repetidamente por la suma de los cuadrados de sus dígitos eventualmente llega a 1. Si no, entra en un ciclo que nunca incluye 1.

La clave: este proceso de reemplazo forma una secuencia implícita. Si tiene ciclo → no es happy. Fast & Slow detecta el ciclo sin almacenar todos los números visitados.

```csharp
public bool IsHappy(int n)
{
    int slow = n;
    int fast = SumOfSquaredDigits(n); // fast empieza un paso adelante

    while (fast != 1 && slow != fast)
    {
        slow = SumOfSquaredDigits(slow);         // Avanza 1
        fast = SumOfSquaredDigits(SumOfSquaredDigits(fast)); // Avanza 2
    }

    return fast == 1; // Si fast llegó a 1 → happy; si se encontraron (ciclo) → no happy
}

private int SumOfSquaredDigits(int n)
{
    int sum = 0;
    while (n > 0)
    {
        int digit = n % 10;
        sum += digit * digit;
        n /= 10;
    }
    return sum;
}
// Tiempo: O(log n) | Espacio: O(1)
```

### 5. Cuándo NO aplica

- **Arrays con índices** — para detectar ciclos en arrays numéricos, considera Cyclic Sort o el mismo Floyd's algorithm pero con el array como estructura de "siguiente nodo" (LeetCode 287 — Find the Duplicate Number)
- Cuando necesitas **todos los nodos del ciclo**, no solo detectar si existe (necesitas HashSet o marcar nodos)
- **Árboles** — los árboles no tienen ciclos por definición (son grafos acíclicos), no aplica

🎯 **AlgoMonster:** Sección "Fast and Slow Pointers".  
🆓 **NeetCode 150:** "Linked List" → Linked List Cycle y Find the Duplicate Number son los representativos del patrón.

---

## Patrón 4 — Merge Intervals

### 1. Intuición

Tienes una lista de reuniones con hora de inicio y fin. ¿Cuáles se solapan? ¿Cuántas salas necesitas? El problema de intervalos aparece constantemente en problemas de scheduling, calendarios, rangos de IP, y cualquier contexto con "períodos de tiempo".

La observación clave: si ordenas los intervalos por su tiempo de inicio, el solapamiento entre dos intervalos consecutivos es detectable con una sola comparación. El intervalo `[a, b]` solapa con `[c, d]` si y solo si `c <= b` (el inicio del segundo es menor o igual al fin del primero). Una vez ordenados, un solo pass lineal resuelve la mayoría de problemas de intervalos.

### 2. Señales de reconocimiento

- "**Intervalos**", "**rangos**", "**periodos**"
- "**Reuniones**", "**horarios**", "**disponibilidad**"
- "**Solapamiento**", "**unión**", "**intersección**" de rangos
- "**Insertar intervalo**" en lista existente
- Coordenadas numéricas con condición de overlap
- "**Cuántas salas**" se necesitan (implica contar solapamientos simultáneos)

### 3. Template en C#

```csharp
// ─── Merge overlapping intervals ───
public int[][] Merge(int[][] intervals)
{
    // Paso 1: ordenar por el inicio del intervalo
    Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));

    var merged = new List<int[]>();
    merged.Add(intervals[0]);

    for (int i = 1; i < intervals.Length; i++)
    {
        int[] last = merged[^1];         // Último intervalo fusionado
        int[] current = intervals[i];

        if (current[0] <= last[1])
        {
            // Hay solapamiento: extender el fin del último intervalo si necesario
            last[1] = Math.Max(last[1], current[1]);
        }
        else
        {
            // No hay solapamiento: agregar como nuevo intervalo separado
            merged.Add(current);
        }
    }

    return merged.ToArray();
}
// Tiempo: O(n log n) por el sort | Espacio: O(n) para la lista resultante
```

### 4. Problemas representativos

**Problema 1: Merge Intervals (LeetCode 56)**

```csharp
public int[][] Merge(int[][] intervals)
{
    Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));

    var result = new List<int[]> { intervals[0] };

    for (int i = 1; i < intervals.Length; i++)
    {
        int[] last = result[^1];
        // Caso 1: solapamiento → extender
        if (intervals[i][0] <= last[1])
            last[1] = Math.Max(last[1], intervals[i][1]);
        // Caso 2: sin solapamiento → agregar
        else
            result.Add(intervals[i]);
    }

    return result.ToArray();
}
// Tiempo: O(n log n) | Espacio: O(n)
```

---

**Problema 2: Insert Interval (LeetCode 57)**

Input: lista de intervalos no solapados ordenados, nuevo intervalo. Output: lista con el nuevo intervalo insertado y mergeado donde corresponda.

```csharp
public int[][] Insert(int[][] intervals, int[] newInterval)
{
    var result = new List<int[]>();
    int i = 0;
    int n = intervals.Length;

    // Fase 1: agregar todos los intervalos que terminan ANTES de que empiece el nuevo
    while (i < n && intervals[i][1] < newInterval[0])
    {
        result.Add(intervals[i]);
        i++;
    }

    // Fase 2: fusionar todos los que solapan con el nuevo intervalo
    while (i < n && intervals[i][0] <= newInterval[1])
    {
        newInterval[0] = Math.Min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.Max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.Add(newInterval); // El nuevo intervalo ya está fusionado

    // Fase 3: agregar el resto de intervalos que empiezan DESPUÉS del nuevo
    while (i < n)
    {
        result.Add(intervals[i]);
        i++;
    }

    return result.ToArray();
}
// Tiempo: O(n) | Espacio: O(n)
```

---

**Problema 3: Meeting Rooms II (LeetCode 253) — cuántas salas se necesitan**

Input: lista de intervalos de reuniones. Output: número mínimo de salas de conferencia necesarias.

La idea: necesitas una sala nueva cada vez que una reunión comienza antes de que otra termine. Rastrear los tiempos de fin de todas las reuniones activas con un min-heap.

```csharp
public int MinMeetingRooms(int[][] intervals)
{
    if (intervals.Length == 0) return 0;

    // Ordenar por tiempo de inicio
    Array.Sort(intervals, (a, b) => a[0].CompareTo(b[0]));

    // Min-heap: guarda el tiempo de fin de cada sala activa
    // El tope siempre es la sala que termina más pronto (candidata a reutilizarse)
    var endTimes = new PriorityQueue<int, int>();

    foreach (int[] meeting in intervals)
    {
        int start = meeting[0], end = meeting[1];

        if (endTimes.Count > 0 && endTimes.Peek() <= start)
        {
            // La sala que termina más pronto termina antes o cuando empieza esta reunión
            // Reutilizar esa sala: remover su tiempo de fin anterior
            endTimes.Dequeue();
        }

        // Agregar el tiempo de fin de esta reunión (nueva sala o reutilizada)
        endTimes.Enqueue(end, end);
    }

    // El número de elementos en el heap = número de salas necesarias
    return endTimes.Count;
}
// Tiempo: O(n log n) | Espacio: O(n)
```

### 5. Cuándo NO aplica

- Intervalos en estructuras **no lineales** (árboles de segmentos son más apropiados para rangos jerárquicos)
- Cuando el solapamiento importa entre **todos los pares**, no solo consecutivos en orden cronológico
- Cuando los intervalos representan **rangos continuos en 2D** (coordenadas x,y) — requiere enfoque de sweep line más complejo

🎯 **AlgoMonster:** Sección "Merge Intervals".  
🆓 **NeetCode 150:** Grupo "Intervals" — incluye todos los representativos del patrón.

---

## Patrón 5 — Cyclic Sort

### 1. Intuición

Si tienes un array de n números en el rango [1, n], cada número tiene exactamente un lugar "natural" donde debería estar: el número `k` debería estar en el índice `k-1`. Si todos los números estuvieran en su lugar natural, el array estaría ordenado.

Cyclic Sort aprovecha esta restricción de rango para hacer un sort in-place en O(n) — imposible en el caso general (cualquier comparison sort es Ω(n log n)), pero posible aquí porque la posición "correcta" de cada elemento es calculable directamente.

El algoritmo: recorre el array, y para cada posición `i`, si el número `nums[i]` no está en su posición natural `nums[i]-1`, haz swap. Repite hasta que `nums[i]` esté en su lugar. Solo entonces avanza `i`.

Después del sort, un segundo pass encuentra cualquier posición donde `nums[i] != i+1` — esa es la posición del número faltante o el lugar donde hay un duplicado.

### 2. Señales de reconocimiento

- Array con **n números en rango [1, n]** o **[0, n-1]** — esta restricción es crítica
- "**Encontrar número faltante**"
- "**Encontrar número duplicado**" o "**todos los duplicados**"
- "**Todos los números del 1 al n excepto...**"
- El problema pide solución O(n) en tiempo y O(1) en espacio — esa restricción delata Cyclic Sort

⚠️ **Señal que confunde:** No todos los problemas de "número faltante" usan Cyclic Sort. Si el array no está en rango [1, n], usa otras técnicas (XOR, suma matemática, Binary Search sobre respuesta).

### 3. Template en C#

```csharp
// ─── Cyclic Sort base ───
void CyclicSort(int[] nums)
{
    int i = 0;
    while (i < nums.Length)
    {
        int correctIndex = nums[i] - 1; // Índice donde DEBERÍA estar nums[i]

        if (nums[i] != nums[correctIndex]) // ¿Está en su lugar correcto?
        {
            // Swap: mover nums[i] a su posición correcta
            (nums[i], nums[correctIndex]) = (nums[correctIndex], nums[i]);
            // NO incrementamos i — el nuevo nums[i] puede que tampoco esté en su lugar
        }
        else
        {
            i++; // Está en su lugar correcto (o es duplicado de la posición) → avanzar
        }
    }
}

// ─── Encontrar el faltante después del sort ───
// for (int i = 0; i < nums.Length; i++)
//     if (nums[i] != i + 1) return i + 1; // El número i+1 está faltando
```

### 4. Problemas representativos

**Problema 1: Missing Number (LeetCode 268)**

Input: array con n números distintos del rango [0, n]. Output: el único número faltante.

```csharp
public int MissingNumber(int[] nums)
{
    // Rango [0, n] — correctIndex = nums[i] (no nums[i]-1)
    int i = 0;
    while (i < nums.Length)
    {
        if (nums[i] < nums.Length && nums[i] != i)
            (nums[i], nums[nums[i]]) = (nums[nums[i]], nums[i]);
        else
            i++;
    }

    // Buscar el índice donde nums[i] != i
    for (int j = 0; j < nums.Length; j++)
        if (nums[j] != j) return j;

    return nums.Length; // El n está faltando
}
// Tiempo: O(n) | Espacio: O(1)

// Alternativa más simple (sin modificar el array): suma matemática
// return n*(n+1)/2 - nums.Sum();
// La suma esperada menos la suma real = el faltante
// O(n) tiempo, O(1) espacio — considerar esta si el array no puede modificarse
```

---

**Problema 2: Find All Duplicates in an Array (LeetCode 442)**

Input: array de n enteros en [1, n] donde algunos aparecen dos veces, otros una vez. Output: todos los duplicados.

```csharp
public IList<int> FindDuplicates(int[] nums)
{
    var duplicates = new List<int>();
    int i = 0;

    // Cyclic sort
    while (i < nums.Length)
    {
        int correctIndex = nums[i] - 1;
        if (nums[i] != nums[correctIndex]) // Si no está en su lugar Y no es un duplicado
            (nums[i], nums[correctIndex]) = (nums[correctIndex], nums[i]);
        else
            i++;
        // Cuando nums[i] == nums[correctIndex] puede ser que:
        // 1. nums[i] == i+1 → ya está en su lugar → avanzar
        // 2. nums[i] != i+1 pero ya hay uno igual en correctIndex → duplicado → avanzar
    }

    // Encontrar duplicados: índice donde nums[i] != i+1
    for (int j = 0; j < nums.Length; j++)
        if (nums[j] != j + 1)
            duplicates.Add(nums[j]); // El valor en esta posición es el duplicado

    return duplicates;
}
// Tiempo: O(n) | Espacio: O(1) sin contar el output
```

---

**Problema 3: Find the Duplicate Number (LeetCode 287)**

Input: array de n+1 enteros donde cada número está en [1, n], exactamente un duplicado. Restricción: no modificar el array, O(1) espacio extra.

Esta restricción hace que Cyclic Sort no sea la solución (requiere modificar el array). La alternativa es Floyd's Cycle Detection tratando el array como una linked list implícita donde `nums[i]` apunta al índice `nums[i]`.

```csharp
public int FindDuplicate(int[] nums)
{
    // Tratar el array como linked list implícita: índice i → índice nums[i]
    // El duplicado crea un ciclo en esta lista implícita
    int slow = nums[0];
    int fast = nums[0];

    // Fase 1: detectar ciclo
    do
    {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);

    // Fase 2: encontrar inicio del ciclo (= el duplicado)
    slow = nums[0];
    while (slow != fast)
    {
        slow = nums[slow];
        fast = nums[fast];
    }

    return slow; // El inicio del ciclo es el número duplicado
}
// Tiempo: O(n) | Espacio: O(1) — cumple las restricciones del problema
```

### 5. Cuándo NO aplica

- Números **fuera del rango [1, n]** — el índice "natural" no es calculable
- Arrays con **múltiples duplicados complejos** donde el counting es necesario (usa Hash Map)
- Cuando el array **no puede modificarse** — usa Floyd's Cycle Detection (como en el Problema 3) o suma matemática/XOR si aplica
- Cuando el rango es **[0, n-1]** hay que ajustar el `correctIndex` a `nums[i]` directamente (no `nums[i]-1`)

🎯 **AlgoMonster:** Sección "Cyclic Sort".  
🆓 **NeetCode 150:** "Arrays & Hashing" → Missing Number y Find the Duplicate Number son los representativos.

---

## Patrón 6 — In-place Reversal of a Linked List

### 1. Intuición

Para revertir una linked list, la solución naive es copiarla a un array, invertir el array, y reconstruir la lista — O(n) tiempo y espacio. Pero hay una manera de hacer exactamente lo mismo con O(1) de espacio: redirigir punteros.

La idea: en lugar de mover nodos, cambias a qué nodo apunta `Next`. El nodo actual pasa a apuntar al nodo anterior. Antes de hacer ese cambio, guardas el siguiente nodo (de lo contrario lo pierdes). Iteras hasta llegar al final. El último nodo procesado es el nuevo `head`.

Para reversal de un segmento [left, right], primero llegas a la posición `left`, aplicas el reversal solo en ese segmento, y reconectas con el resto de la lista.

### 2. Señales de reconocimiento

- "**Revertir linked list**" completa o de posición m a n
- "**Rotar linked list** k posiciones"
- "**Revertir cada k elementos**" de la lista
- "**Swap de pares** de nodos"
- "**Palíndromo** de linked list" (combina Fast & Slow + reversal de la segunda mitad)

### 3. Template en C#

```csharp
// ─── Reversal completo ───
public ListNode? ReverseList(ListNode? head)
{
    ListNode? prev = null;
    ListNode? current = head;

    while (current != null)
    {
        ListNode? next = current.Next; // 1. Guardar el siguiente ANTES de perderlo
        current.Next = prev;           // 2. Redirigir: current apunta al anterior
        prev = current;                // 3. Avanzar prev
        current = next;                // 4. Avanzar current al siguiente guardado
    }

    return prev; // prev es el nuevo head (el último nodo procesado)
}
// Tiempo: O(n) | Espacio: O(1)

// ─── Reversal de posición left a right (1-indexed) ───
public ListNode? ReverseBetween(ListNode? head, int left, int right)
{
    if (head == null || left == right) return head;

    // Nodo dummy para simplificar el manejo del head
    var dummy = new ListNode(0) { Next = head };
    ListNode? prevLeft = dummy; // El nodo justo antes de la posición left

    // Avanzar hasta justo antes de left
    for (int i = 1; i < left; i++)
        prevLeft = prevLeft!.Next;

    // current es el primer nodo del segmento a revertir
    ListNode? current = prevLeft!.Next;
    ListNode? prev = null;

    // Revertir el segmento de left a right
    for (int i = 0; i < right - left + 1; i++)
    {
        ListNode? next = current!.Next;
        current.Next = prev;
        prev = current;
        current = next;
    }

    // Reconectar:
    // - El nodo original en posición left ahora es el último del segmento revertido
    //   → debe apuntar al nodo que sigue después de right (= current)
    prevLeft.Next!.Next = current;
    // - prevLeft debe apuntar al nuevo primer nodo del segmento (= prev)
    prevLeft.Next = prev;

    return dummy.Next;
}
```

### 4. Problemas representativos

**Problema 1: Reverse Linked List (LeetCode 206)**

```csharp
public ListNode? ReverseList(ListNode? head)
{
    ListNode? prev = null, current = head;

    while (current != null)
    {
        ListNode? next = current.Next;
        current.Next = prev;
        prev = current;
        current = next;
    }

    return prev;
}
// Tiempo: O(n) | Espacio: O(1) — iterativo
// La versión recursiva es O(n) en ESPACIO (call stack) — peor, no la uses en entrevista
// a menos que el entrevistador la pida específicamente
```

---

**Problema 2: Reverse Linked List II — de posición left a right (LeetCode 92)**

```csharp
public ListNode? ReverseBetween(ListNode? head, int left, int right)
{
    var dummy = new ListNode(0) { Next = head };
    ListNode? prevLeft = dummy;

    for (int i = 1; i < left; i++)
        prevLeft = prevLeft!.Next;

    ListNode? current = prevLeft!.Next;
    ListNode? prev = null;

    for (int i = 0; i < right - left + 1; i++)
    {
        ListNode? next = current!.Next;
        current.Next = prev;
        prev = current;
        current = next;
    }

    prevLeft.Next!.Next = current; // El tail del segmento apunta al resto
    prevLeft.Next = prev;          // prevLeft apunta al nuevo head del segmento

    return dummy.Next;
}
// Tiempo: O(n) | Espacio: O(1)
```

---

**Problema 3: Reverse Nodes in k-Group (LeetCode 25) — Hard**

Input: linked list, entero k. Output: lista con cada grupo de k nodos revertido. Si el grupo final tiene menos de k nodos, dejarlo como está.

```csharp
public ListNode? ReverseKGroup(ListNode? head, int k)
{
    // Verificar si hay al menos k nodos disponibles para revertir
    ListNode? check = head;
    int count = 0;
    while (check != null && count < k)
    {
        check = check.Next;
        count++;
    }
    if (count < k) return head; // Menos de k nodos → no revertir, retornar tal cual

    // Revertir los primeros k nodos
    ListNode? prev = null, current = head;
    for (int i = 0; i < k; i++)
    {
        ListNode? next = current!.Next;
        current.Next = prev;
        prev = current;
        current = next;
    }

    // head ahora es el último nodo del grupo revertido
    // Recursivamente procesar el resto y conectar
    head!.Next = ReverseKGroup(current, k);

    return prev; // prev es el nuevo head del grupo revertido
}
// Tiempo: O(n) | Espacio: O(n/k) por el call stack de la recursión
// Para versión iterativa: O(1) espacio — más compleja de implementar pero preferible en producción
```

### 5. Cuándo NO aplica

- **Arrays** — para revertir un array o subarray, usa Two Pointers (opposite direction). El reversal in-place de array es más simple que el de linked list
- Cuando revertir implica **cambiar valores** en lugar de redirigir punteros (arrays de valores inmutables)
- Cuando la lista usa **doble dirección** (doubly linked list) — el reversal es diferente (también cambias `Prev`)

🎯 **AlgoMonster:** Sección "In-place Reversal of Linked List".  
🆓 **NeetCode 150:** "Linked List" → Reverse Linked List y Reverse Nodes in k-Group son los representativos.

---

## Tabla resumen — 6 patrones lineales

| Patrón | Señal principal | Tiempo típico | Espacio típico | Estructura requerida |
|---|---|---|---|---|
| Two Pointers | Array ordenado + buscar par | O(n) | O(1) | Array o string |
| Sliding Window | Subarray/substring contiguo | O(n) | O(k) | Array o string |
| Fast & Slow Pointers | Ciclo o nodo medio | O(n) | O(1) | Linked list |
| Merge Intervals | Intervalos/rangos + solapamiento | O(n log n) | O(n) | Array de pares |
| Cyclic Sort | [1,n] range + faltante/duplicado | O(n) | O(1) | Array con restricción |
| In-place Reversal | Revertir linked list o segmento | O(n) | O(1) | Linked list |

---

## Checklist del módulo — patrones lineales

Marca cada ítem solo cuando puedas completarlo sin consultar código de referencia:

- [ ] Implemento Two Pointers converging (Two Sum II) en < 10 minutos
- [ ] Implemento Two Pointers same-direction (Remove Duplicates) en < 10 minutos
- [ ] Identifico si un problema pide ventana fija o variable en < 1 minuto
- [ ] Implemento Sliding Window fija y variable sin errores de indexación
- [ ] Implemento Minimum Window Substring (el caso más complejo) en < 25 minutos
- [ ] Implemento detección de ciclo con Fast & Slow sin consultar el template
- [ ] Encuentro el inicio del ciclo usando Floyd's Algorithm segunda fase
- [ ] Implemento Merge Intervals con el sort correcto y los 2 casos (solapa/no solapa)
- [ ] Implemento Insert Interval con las 3 fases (antes/overlap/después)
- [ ] Implemento Cyclic Sort y lo uso para encontrar faltante y duplicados
- [ ] Implemento reversal completo de linked list iterativo (no recursivo)
- [ ] Implemento reversal de segmento [left, right] con reconexión correcta

---

> **Siguiente archivo:** [02-02-patrones-no-lineales.md →](./02-02-patrones-no-lineales.md)
