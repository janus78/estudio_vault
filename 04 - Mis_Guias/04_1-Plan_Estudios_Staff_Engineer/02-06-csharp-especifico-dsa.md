# 02-06 — C# Específico para DSA

> **Propósito de este archivo:** Cerrar el gap entre "sé algoritmos" y "sé algoritmos en C# a nivel Staff". Tienes 10 años de C# — no necesitas que te expliquen List o Dictionary. Lo que necesitas es saber **cuándo cada colección es la elección correcta en un contexto algorítmico**, qué operaciones tienen complejidades distintas a las que intuitivamente esperas, y cómo articular esas decisiones en una entrevista cuando el entrevistador también conoce .NET.
>
> La diferencia entre un Senior y un Staff en C# para DSA no es cuántas APIs conocen. Es que el Staff sabe exactamente qué pasa debajo del hood y lo articula sin dudar.

---

## Sección 1 — Colecciones de .NET: complejidades reales

Antes de la tabla: el error más común con las complejidades de colecciones .NET es asumir que "es lo mismo que en otros lenguajes" o que "probablemente es O(1)". No siempre. Las sorpresas están en los detalles.

### Tabla de referencia por operación

| Colección | Add/Enqueue | Remove | Contains | Peek/First | Indexer `[i]` | Orden |
|---|---|---|---|---|---|---|
| `List<T>` | O(1) amort.¹ | O(n)² | O(n) | O(1)³ | O(1) | Inserción |
| `LinkedList<T>` | O(1) | O(1)⁴ | O(n) | O(1)⁵ | N/A | Inserción |
| `Dictionary<K,V>` | O(1) amort. | O(1) amort. | O(1) amort. | N/A | O(1) amort. | No garantizado |
| `HashSet<T>` | O(1) amort. | O(1) amort. | O(1) amort. | N/A | N/A | No garantizado |
| `SortedSet<T>` | O(log n) | O(log n) | O(log n) | O(log n)⁶ | N/A | Ascendente |
| `SortedDictionary<K,V>` | O(log n) | O(log n) | O(log n) | N/A | O(log n) | Ascendente por key |
| `Stack<T>` | O(1) amort. | O(1) | N/A | O(1) | N/A | LIFO |
| `Queue<T>` | O(1) amort. | O(1) | N/A | O(1) | N/A | FIFO |
| `PriorityQueue<T,P>` | O(log n) | O(log n) | N/A | O(1) | N/A | Por prioridad |

**Notas que importan:**

¹ `List<T>.Add`: amortizado O(1) porque cuando el array interno se llena, se duplica la capacidad (O(n) en esa operación). Si sabes el tamaño final, `new List<T>(capacity)` evita todos los resizes.

² `List<T>.Remove(value)`: O(n) porque busca primero (O(n)) y luego desplaza elementos (O(n)). `RemoveAt(index)` también es O(n) por el desplazamiento.

³ `List<T>` no tiene `Peek` — usar `list[^1]` (C# 8+ range operator) para el último elemento en O(1).

⁴ `LinkedList<T>.Remove(node)` es O(1) si tienes el `LinkedListNode<T>`. `Remove(value)` es O(n) porque busca el nodo primero.

⁵ `LinkedList<T>.First.Value` y `.Last.Value` son O(1) — los nodos extremos se mantienen como referencias directas.

⁶ `SortedSet<T>.Min` y `.Max` son O(log n), no O(1). La estructura es un árbol rojo-negro, no un heap.

### La elección correcta por escenario algorítmico

**Usa `Dictionary<K,V>` cuando:**
Necesitas mapear valor→frecuencia, valor→índice, o cualquier relación clave-valor con lookup posterior. El patrón más frecuente en entrevistas: Two Sum, Anagram Groups, Subarray Sum Equals K.

```csharp
// Two Sum — la razón concreta de usar Dictionary
public int[] TwoSum(int[] nums, int target)
{
    var seen = new Dictionary<int, int>(); // valor → índice

    for (int i = 0; i < nums.Length; i++)
    {
        int complement = target - nums[i];
        if (seen.ContainsKey(complement))
            return new[] { seen[complement], i };
        seen[nums[i]] = i; // Guardar para lookups futuros
    }

    return Array.Empty<int>();
}
// Dictionary da O(1) lookup — sin él, necesitarías O(n) búsqueda → O(n²) total
```

**Usa `HashSet<T>` cuando:**
Solo necesitas saber si un elemento existe — sin asociar ningún valor. Contains Duplicate, Longest Consecutive Sequence. Si también necesitas el índice u otro valor, usa `Dictionary`.

**Usa `SortedSet<T>` cuando:**
Necesitas mantener un conjunto ordenado con inserciones dinámicas Y necesitas operaciones de orden (mínimo, máximo, elementos en rango). Diferencia crítica con `List` + sort: `SortedSet` mantiene el orden en cada inserción en O(log n). Útil en Sliding Window Maximum y problemas de mediana estática.

⚠️ **`SortedSet<T>` no permite duplicados.** Si tu problema tiene elementos repetidos, necesitas `SortedDictionary<T, int>` (elemento → frecuencia) o un enfoque diferente.

**Usa `PriorityQueue<T,P>` cuando:**
Necesitas extraer el elemento de mayor/menor prioridad repetidamente. Top K, K-way Merge, Dijkstra, Scheduling. El heap de .NET 6+ es min-heap por defecto — para max-heap, usa prioridad negada o un `Comparer<T>` invertido.

```csharp
// Min-heap (default) — extrae el MENOR primero
var minHeap = new PriorityQueue<int, int>();
minHeap.Enqueue(5, 5);
minHeap.Enqueue(1, 1);
minHeap.Enqueue(3, 3);
int smallest = minHeap.Dequeue(); // 1

// Max-heap — negando la prioridad
var maxHeap = new PriorityQueue<int, int>();
maxHeap.Enqueue(5, -5); // Prioridad negada
maxHeap.Enqueue(1, -1);
maxHeap.Enqueue(3, -3);
int largest = maxHeap.Dequeue(); // 5
```

**Usa `Array` en lugar de `List<T>` para tablas de DP cuando:**
El tamaño es conocido en tiempo de compilación o al inicio del problema. Arrays tienen menos overhead que `List<T>` — sin capacidad dinámica, sin padding, acceso más directo. En DP donde la tabla tiene dimensiones fijas `n × m`, usa `int[,]` o `int[][]`.

```csharp
// DP table — Array vs List
int[,] dp = new int[m + 1, n + 1]; // Correcto para DP 2D — tamaño fijo, sin overhead
var dpList = new List<List<int>>();  // Innecesario — tamaño conocido de antemano
```

---

## Sección 2 — Span<T> y ReadOnlySpan<T> en algoritmos de strings

### Por qué importa en DSA

`string.Substring()` en .NET es O(n) — crea una nueva string con allocación de memoria. En algoritmos de strings con muchos slices (palíndromos, anagramas, parsing), cada `Substring` es una allocación. Con inputs grandes, el garbage collector se convierte en el cuello de botella.

`Span<T>` y `ReadOnlySpan<char>` son vistas sobre la misma memoria — sin copias, sin allocaciones, sin GC pressure. O(1) para crear un "slice".

```csharp
// ❌ Patrón común — O(n) allocations por cada llamada
bool IsPalindromeNaive(string s, int left, int right)
{
    string sub = s.Substring(left, right - left + 1); // Allocation nueva aquí
    int l = 0, r = sub.Length - 1;
    while (l < r)
    {
        if (sub[l] != sub[r]) return false;
        l++; r--;
    }
    return true;
}

// ✅ Con ReadOnlySpan<char> — cero allocations
bool IsPalindromeSpan(ReadOnlySpan<char> s)
{
    int left = 0, right = s.Length - 1;
    while (left < right)
    {
        if (s[left] != s[right]) return false;
        left++; right--;
    }
    return true;
}

// Uso — AsSpan crea la vista en O(1), sin copiar la string
string original = "racecar";
bool result = IsPalindromeSpan(original.AsSpan(0, 7));      // Toda la string
bool result2 = IsPalindromeSpan(original.AsSpan(1, 5));     // Substring sin copia
```

### Cuándo usar Span en DSA

✅ **Usar cuando:**
- Algoritmos de strings con múltiples slices recursivos (palíndromos, divide and conquer en strings)
- Parsing de input con múltiples separadores sin necesitar las substrings como objetos
- Inner loops donde `Substring` aparecería repetidamente

❌ **No usar cuando:**
- Necesitas pasar el slice a un método que acepta `string` — debes hacer `.ToString()`, que crea la copia de todos modos
- El algoritmo solo hace un slice — el beneficio no justifica la complejidad del código
- En una entrevista donde el entrevistador no conoce Span — puede generar confusión innecesaria (prioriza claridad sobre micro-optimización)

⚠️ **Limitación crítica:** `Span<T>` no puede ser un campo de clase, no puede ser capturado en closures, y no puede ser usado con `async/await`. Es solo para uso en métodos síncronos locales.

---

## Sección 3 — LINQ en algoritmos: cuándo ayuda y cuándo daña

LINQ es la herramienta más peligrosa en DSA cuando se usa sin pensar en las complejidades subyacentes. El problema no es LINQ en sí — es que esconde la complejidad detrás de una sintaxis elegante.

### Los errores más costosos

```csharp
// ─── ERROR 1: Contains en List — O(n) disfrazado ───
var lista = new List<int> { 1, 2, 3, 4, 5 };

// ❌ Esto parece O(1) pero es O(n) — itera toda la lista
if (lista.Contains(target)) { ... }

// ✅ Si necesitas Contains frecuente, usa HashSet — O(1)
var conjunto = new HashSet<int>(lista);
if (conjunto.Contains(target)) { ... }

// ─── ERROR 2: OrderBy().First() cuando solo necesitas el mínimo ───
int[] nums = { 5, 2, 8, 1, 9 };

// ❌ O(n log n) — ordena todo para tomar solo el primero
int minimo = nums.OrderBy(x => x).First();

// ✅ O(n) — una sola pasada
int minimo2 = nums.Min();

// ─── ERROR 3: Count() en IEnumerable — puede ser O(n) ───
IEnumerable<int> secuencia = ObtenerSecuencia();

// ❌ Count() itera toda la secuencia (O(n)) si no implementa ICollection
if (secuencia.Count() > 0) { ... }

// ✅ Any() cortocircuita en el primer elemento — O(1) en práctica
if (secuencia.Any()) { ... }

// ─── ERROR 4: Múltiples iteraciones sobre la misma IEnumerable ───
// Si ObtenerSecuencia() es lazy (yield return), se ejecuta CADA VEZ que iteras
var data = ObtenerSecuencia();
int count = data.Count();     // Primera iteración — O(n)
int sum = data.Sum();         // Segunda iteración — O(n)
// Total: O(2n) con dos pasadas completas

// ✅ Materializar una vez con ToList() o ToArray()
var data2 = ObtenerSecuencia().ToList(); // Una iteración
int count2 = data2.Count;   // O(1) — accede a la propiedad de List, no itera
int sum2 = data2.Sum();     // O(n) — pero solo una iteración total

// ─── ERROR 5: SelectMany / nested LINQ — complejidad oculta ───
// Parece "una línea" pero puede ser O(n²)
var result = matrix.SelectMany(row => row).Where(x => x > 0);
// Si matrix es n×n: SelectMany es O(n²), Where es O(n²) — total O(n²)
// Nada malo, pero hay que saberlo — no es mágicamente más rápido
```

### La regla del entrevistador

En una entrevista donde el entrevistador mide complejidad, toda operación LINQ tiene su complejidad y no puedes esconderla detrás de sintaxis elegante. Si usas `OrderBy`, la solución es O(n log n). Si usas `Where` sobre una lista, es O(n). El entrevistador lo sabe — y espera que tú también lo sepas.

**Cuándo LINQ sí está bien en DSA:**
- Operaciones de inicialización (fuera del loop principal): `Enumerable.Range(0, n)`, `Array.Fill` equivalente
- Aggregations simples sobre colecciones ya materializadas: `.Sum()`, `.Max()`, `.Min()`, `.Count` (propiedad, no método)
- `.ToHashSet()`, `.ToDictionary()` para crear estructuras eficientes a partir de arrays

---

## Sección 4 — Articular decisiones de C# en entrevista Staff

La diferencia entre una respuesta de Senior y una de Staff no es el código — es la articulación. El mismo código con justificación diferente produce percepciones completamente distintas en el entrevistador.

### Contraste directo — tres decisiones concretas

#### Decisión 1: PriorityQueue vs SortedSet para un heap

**Nivel promedio:**
> "Usé PriorityQueue porque es más rápido que SortedSet."

**Nivel Staff:**
> "Usé `PriorityQueue<int, int>` en lugar de `SortedSet<int>` porque este problema tiene elementos repetidos — el mínimo de temperatura que aparece múltiples veces. `SortedSet` no permite duplicados: agregaría el segundo 42°C y silenciosamente lo ignoraría. `PriorityQueue` los maneja correctamente. Además, las operaciones Enqueue/Dequeue son O(log n) en ambas, pero `PriorityQueue` tiene un factor constante menor porque un heap binario es más simple de mantener que un árbol rojo-negro."

---

#### Decisión 2: Stack vs List para DFS iterativo

**Nivel promedio:**
> "Usé Stack para DFS porque Stack es lo que corresponde."

**Nivel Staff:**
> "Usé `Stack<T>` en lugar de `List<T>` por semántica explícita — cuando alguien lee el código, `Push` y `Pop` comunican inmediatamente que estamos haciendo DFS con un stack de llamadas. `List` con `Add` y `RemoveAt(Count-1)` hace exactamente lo mismo en términos de complejidad, pero requiere que el lector razone sobre el acceso al final para entender la intención. En producción, la claridad de intención es igual de importante que la performance — y aquí tenemos ambas sin costo adicional."

---

#### Decisión 3: Array vs List para tabla de DP

**Nivel promedio:**
> "Usé un array porque es más rápido."

**Nivel Staff:**
> "Usé `int[] dp = new int[n + 1]` en lugar de `List<int>` porque en DP el tamaño de la tabla es determinístico — es función directa del input. `List<T>` reserva capacidad extra y tiene un layer de indirección para el array interno. Con n hasta 10⁴, eso es overhead sin beneficio. Más importante: en DP 2D, `int[,]` tiene mejor localidad de caché que `List<List<int>>` porque `int[,]` es un bloque contiguo en memoria — el acceso secuencial por filas tendrá cache hits consistentes."

---

### Más decisiones para tener listas

**¿Por qué `StringBuilder` en lugar de concatenación de strings?**
> "String concatenation en un loop es O(n²) en total — cada `+` crea una nueva string. `StringBuilder` amortiza el costo: append es O(1) amortizado y `.ToString()` final es O(n). Para n iteraciones, la diferencia es O(n) vs O(n²) — no estético, matemático."

**¿Por qué `Array.Fill` en lugar de un loop de inicialización?**
> "Funcionalmente equivalente, pero `Array.Fill` usa `Buffer.MemoryCopy` internamente en el JIT para arrays de tipos primitivos — vectorizado con SIMD en hardware moderno. En entrevista no cambia el resultado, pero en producción con arrays grandes es una optimización real. Lo uso por hábito."

**¿Por qué `TryGetValue` en lugar de `ContainsKey` + indexer?**
```csharp
// ❌ Dos lookups en el Dictionary
if (dict.ContainsKey(key))
    value = dict[key];

// ✅ Un solo lookup
if (dict.TryGetValue(key, out int value))
    // usar value
```
> "Con `ContainsKey` + indexer hago dos lookups en el hash table — calcular el hash, encontrar el bucket, verificar igualdad... dos veces. `TryGetValue` hace un solo lookup y devuelve el valor si existe. En loops donde busco millones de veces, esto es 2x de overhead evitable."

---

## Sección 5 — Patrones de inicialización que marcan la diferencia

Pequeños detalles de inicialización que un candidato Staff hace automáticamente y que un Junior ignora:

```csharp
// ─── Inicialización con capacidad conocida ───
// Si sabes que tendrás n elementos, evita los resizes de List
var result = new List<int>(n);           // Reserva capacidad, O(1) todos los Add
var dict = new Dictionary<int, int>(n);  // Evita rehashing

// ─── Inicializar array con valor no-default ───
var dist = new int[n];
Array.Fill(dist, int.MaxValue); // Más limpio que un loop — vectorizado internamente
dist[source] = 0;

// ─── Inicializar List of Lists ───
// ❌ Esto crea una lista de referencias a la MISMA lista vacía
var adj = new List<int[]>(Enumerable.Repeat(new List<int>(), n)); // BUG

// ✅ Correcto — una lista nueva por índice
var adj = new List<int>[n];
for (int i = 0; i < n; i++)
    adj[i] = new List<int>();

// ─── Deconstructing tuples en foreach ───
var edges = new List<(int from, int to, int weight)>();
foreach (var (from, to, weight) in edges) // Deconstruction elegante
{
    adj[from].Add((to, weight));
}

// ─── Pattern matching en early returns ───
public int[] TwoSum(int[] nums, int target)
{
    if (nums is null or { Length: 0 }) return Array.Empty<int>(); // C# 9+ pattern
    // ...
}

// ─── Uso de índices negativos (C# 8+) ───
int last = nums[^1];           // Último elemento — O(1), equivalente a nums[nums.Length - 1]
int secondLast = nums[^2];     // Penúltimo
var lastTwo = nums[^2..];      // Range — crea una copia, O(n) — cuidado en loops
```

---

## Checklist de "dominas C# para DSA"

Con criterios medibles — no "creo que lo sé":

- [ ] Puedo decir sin dudar la complejidad de `Contains` en `List<T>`, `HashSet<T>` y `SortedSet<T>` (son distintas)
- [ ] Sé por qué `SortedSet<T>` no es un reemplazo de `PriorityQueue<T,P>` para elementos con duplicados
- [ ] Puedo explicar la diferencia entre `Count()` (método LINQ) y `.Count` (propiedad) en términos de complejidad
- [ ] Uso `TryGetValue` en lugar de `ContainsKey` + indexer de forma automática
- [ ] Inicializo `List<T>` con capacidad cuando conozco el tamaño final
- [ ] Uso `Array.Fill` en lugar de loops de inicialización
- [ ] Puedo articular por qué uso `Array` en lugar de `List<T>` para tablas de DP de tamaño fijo
- [ ] Sé en qué casos `Span<T>` mejora los algoritmos de strings y en cuáles no aplica
- [ ] Puedo identificar LINQ que es O(n²) en disguise vs LINQ que es genuinamente O(n)
- [ ] Para cualquier decisión de colección que tomo, puedo dar la justificación de Staff en 2-3 oraciones concretas

---

> **Recursos para este archivo:**
> - **Pluralsight → "C# Performance"** path — consumir después de completar este archivo, para profundizar en `Span<T>`, Memory<T>, y optimizaciones de GC
> - **Microsoft Docs → Collections and Data Structures** — referencia de complejidades oficiales para verificar
> - **BenchmarkDotNet** — si quieres medir las diferencias en tu máquina (recomendado como ejercicio opcional)

> **Siguiente archivo:** [02-07-simulacion-entrevistas-coding.md →](./02-07-simulacion-entrevistas-coding.md)
