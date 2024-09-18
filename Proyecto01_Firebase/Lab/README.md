# Laboratorio Fire Base Real Time Database

## Instrucciones

Siga los siguientes pasos y tome evidencia (screenshoots) de la realización y resultado de cada uno:

1. **Ver y seguir los pasos del video [*insertar video*] para configurar el proyecto de Firebase.**
2. **Crear múltiples registros en la base:**
   - Crear una función que inserte `n` elementos (al menos 5) de lo que quiera (por ejemplo, pueden ser productos). Cada registro debe tener al menos tres propiedades y una de ellas debe ser numérica (por ejemplo, para producto pueden ser: nombre, categoría y precio).
3. **Leer registros:**
   - Cree una función que lea y muestre los registros que acaba de crear.
4. **Filtrar registros de acuerdo a una de sus propiedades:**
   - Cree una función que obtenga solo aquellos registros que pertenezcan a alguna de sus propiedades. (Por ejemplo, para los productos, solo los que pertenecen a la categoría "Electrónica").
   - **Pista:** Ver cómo usar `equalTo()`.
5. **Obtener los primeros 3 registros:**
   - Modifique la función anterior para obtener solo los primeros 3 registros.
   - **Pista:** Ver cómo usar `limitToFirst()`.
6. **Actualizar un registro específico:**
   - Cree una función para seleccionar un registro al azar y actualizar uno de sus campos. Por ejemplo, cambiar el nombre del producto o el precio.
   - Usar `set()` o `update()` para realizar la actualización.
7. **Ordenar los registros de acuerdo a la propiedad numérica:**
   - Crear una función que obtenga los elementos ordenados de menor a mayor de acuerdo a la propiedad numérica que escogió. Por ejemplo, los productos ordenados de mayor a menor precio.
   - **Pista:** Ver cómo usar `orderByChild()`.
8. **Eliminar un registro:**
   - Cree una función que elimine uno de los registros por su clave.
   - **Pista:** Usar `set()` con `null`.
9. **Ordenar la evidencia en un documento y entregarla.**
