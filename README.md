# Práctica XML y DTD

## Objetivo

El objetivo de esta práctica es aprender a crear documentos XML bien formados,
definir y utilizar DTD internos y externos, establecer cardinalidades y
restricciones sobre atributos, validar documentos XML y utilizar Git para
registrar los cambios realizados durante el desarrollo.
## Ejercicio 1: Pedido

### Modelo propuesto

Se creó un documento XML para representar un pedido con la siguiente información:

- Destinatario: Juan Delgado Martínez
- Artículo: Bicicleta Bianchi
- Dirección: calle Reforma 423, interior 201
- Fecha de entrega: 2021-09-19

La dirección se dividió en calle, número e interior para permitir búsquedas
independientes de cada dato.

### Decisiones de diseño

Se utilizó `pedido` como elemento raíz. Los datos principales se representaron
como elementos XML.

La dirección se separó en varios componentes para facilitar su consulta y
procesamiento.

La fecha se almacenó con el formato `AAAA-MM-DD` para facilitar su ordenamiento
y procesamiento.
## Ejercicio 2: Nota

### DTD externo

Se creó el archivo `nota.dtd` con la siguiente estructura:

```dtd
<!ELEMENT nota (para, de, titulo, contenido)>
<!ELEMENT para (#PCDATA)>
<!ELEMENT de (#PCDATA)>
<!ELEMENT titulo (#PCDATA)>
<!ELEMENT contenido (#PCDATA)>
```

El archivo `nota.xml` utiliza el DTD externo mediante:

```xml
<!DOCTYPE nota SYSTEM "nota.dtd">
```

### DTD interno

También se creó `nota-interno.xml`, donde las declaraciones DTD se encuentran
dentro del propio documento XML.

El DTD interno es conveniente cuando las reglas solamente se utilizarán en un
documento. El DTD externo facilita reutilizar las mismas reglas en varios
documentos XML.

### Pruebas realizadas

| Modificación | ¿Bien formado? | ¿Válido? | Explicación |
|---|---|---|---|
| Cambiar `para` por `destinatario` | Sí | No | `destinatario` no está declarado en el DTD. |
| Cambiar el orden de `para` y `de` | Sí | No | El DTD establece el orden `para, de, titulo, contenido`. |
| Agregar `telefono` | Sí | No | `telefono` no forma parte de la estructura definida por el DTD. |
## Ejercicio 3: Matrícula

### Modelo

Se creó un documento XML para representar la matrícula de un estudiante.

El elemento raíz es `matricula` y contiene dos elementos principales:
`personal` y `pago`.

El elemento `personal` contiene el DNI, nombre, titulación, curso académico
y los domicilios del estudiante.

### Cardinalidad

El requisito establece que debe existir al menos un domicilio.

Para representar esta condición en el DTD se utilizó el operador `+`:

```dtd
<!ELEMENT domicilios (domicilio+)>
```

El símbolo `+` significa uno o más, por lo que el documento debe contener
como mínimo un elemento `domicilio`.

Al eliminar todos los domicilios, el XML continúa estando bien formado,
pero deja de ser válido porque no cumple la cardinalidad establecida en el DTD.

### Restricción del atributo tipo

El atributo `tipo` del elemento `domicilio` es obligatorio y solamente puede
tener los valores `familiar` o `habitual`.

La restricción se definió mediante:

```dtd
<!ATTLIST domicilio tipo (familiar|habitual) #REQUIRED>
```

`#REQUIRED` indica que el atributo es obligatorio.

### DTD externo

El archivo `matricula.xml` utiliza el DTD externo mediante:

```xml
<!DOCTYPE matricula SYSTEM "matricula.dtd">
```

Las reglas de validación se encuentran en el archivo `matricula.dtd`.

### DTD interno

También se creó `matricula-interno.xml`. En esta versión, las declaraciones
DTD están incluidas directamente dentro del documento XML mediante:

```xml
<!DOCTYPE matricula [
    ...
]>
```

### Pruebas realizadas

| Caso | Predicción | Resultado | Explicación |
|---|---|---|---|
| `tipo="familiar"` | Válido | Válido | `familiar` es un valor permitido. |
| `tipo="habitual"` | Válido | Válido | `habitual` es un valor permitido. |
| `tipo="temporal"` | No válido | No válido | `temporal` no pertenece a los valores permitidos. |
| Sin atributo `tipo` | No válido | No válido | El atributo está declarado como `#REQUIRED`. |
| Sin domicilios | No válido | No válido | `domicilio+` exige al menos un domicilio. |
## Preguntas finales

### 1. ¿Cuál es la diferencia entre XML bien formado y XML válido?

Un XML bien formado cumple las reglas sintácticas de XML, como tener un único
elemento raíz, cerrar correctamente las etiquetas y mantener un anidamiento
correcto.

Un XML válido, además de estar bien formado, cumple las reglas establecidas
por su DTD.

### 2. ¿Qué función cumple un DTD?

Un DTD define la estructura que debe cumplir un documento XML. Permite
establecer qué elementos pueden existir, su orden, su cardinalidad y los
atributos permitidos.

### 3. ¿Qué diferencia existe entre DTD interno y externo?

El DTD interno se encuentra dentro del mismo documento XML.

El DTD externo se almacena en un archivo independiente con extensión `.dtd`
y puede ser reutilizado por varios documentos XML.

### 4. ¿Cómo se expresa cardinalidad en DTD?

La cardinalidad puede expresarse mediante los operadores:

- `?` = cero o una vez.
- `*` = cero o más veces.
- `+` = una o más veces.

Por ejemplo:

`<!ELEMENT domicilios (domicilio+)>`

indica que debe existir al menos un domicilio.

### 5. ¿Cómo puede restringirse un atributo a determinados valores?

Se puede utilizar una enumeración en una declaración `ATTLIST`.

Por ejemplo:

`<!ATTLIST domicilio tipo (familiar|habitual) #REQUIRED>`

Esto establece que `tipo` solamente puede ser `familiar` o `habitual` y que
el atributo es obligatorio.

### 6. ¿Qué ventaja proporcionó Git durante las pruebas?

Git permitió registrar el progreso de la práctica mediante commits y conservar
versiones correctas de los archivos antes de realizar modificaciones de prueba.

### 7. ¿Qué utilidad tuvieron `git diff` y `git restore`?

`git diff` permitió observar exactamente qué modificaciones se realizaron en
los archivos.

`git restore` permitió descartar las modificaciones de las pruebas y recuperar
la versión registrada previamente en Git.

### 8. ¿Qué ventaja proporcionó una rama para desarrollar una solución alternativa?

La rama permitió desarrollar la versión con DTD interno de matrícula de forma
independiente sin modificar directamente la rama principal. Después de
comprobar el resultado, los cambios se integraron mediante `git merge`.

## Conclusiones

La práctica permitió comprender la diferencia entre un documento XML bien
formado y un documento XML válido. También permitió utilizar DTD externos e
internos para definir la estructura y las restricciones de los documentos XML.

Se aplicaron cardinalidades mediante operadores como `+` y restricciones de
atributos mediante `ATTLIST`. Las pruebas realizadas permitieron comprobar
cómo un documento puede estar bien formado pero no ser válido respecto a su
DTD.

Finalmente, Git permitió mantener un historial del desarrollo, comparar y
restaurar cambios y utilizar ramas para trabajar con soluciones alternativas
antes de integrarlas a la rama principal.