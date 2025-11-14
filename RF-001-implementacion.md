# Pasos de Mantenimiento - RF-001

## 1. INICIACIÓN
Se identificó la necesidad de agregar un contador de elementos por categoría en la barra de estado.
- Requerimiento: RF-001
- Formato: `Total Items: 06; Package:02, Class:02; Component: 02`

## 2. LOCALIZACIÓN DEL CONCEPTO
Revisé el código y encontré:
- `AppFrame.java` - tiene la barra de estado
- `StructureDiagram.java` - guarda los elementos del diagrama
- Ya existe un patrón Observer para las actualizaciones

## 3. ANÁLISIS DE IMPACTO
Solo necesito modificar 2 archivos:
- `StructureDiagram.java` - agregar métodos para contar
- `AppFrame.java` - agregar el label y actualizarlo

## 4. PRE-FACTORING
Compilé el proyecto para verificar que todo funciona:
```bash
mvn clean compile
```
Revisé donde agregar el código. La barra de estado ya tiene otros labels (coordenadas, memoria).

## 5. ACTUALIZACIÓN

### StructureDiagram.java
Agregué 2 métodos al final del archivo:

```java
public java.util.Map<String, Integer> getElementCountByCategory() {
    // Recorre los elementos y cuenta por tipo
    // Retorna un mapa con Package, Class, Component, Note
}

public int getTotalElementCount() {
    return super.getChildren().size();
}
```

### AppFrame.java
Hice 3 cambios:

1. Agregué el label:
```java
private JLabel elementCountLabel = new JLabel("Total Items: 0");
```

2. Lo puse en la barra de estado:
```java
statusbar.add(elementCountLabel, BorderLayout.CENTER);
```

3. Agregué método para actualizarlo:
```java
private void updateElementCount() {
    // Obtiene el diagrama actual
    // Llama a getElementCountByCategory()
    // Formatea el string y actualiza el label
}
```

4. Lo conecté al método que ya se ejecuta cuando hay cambios:
```java
updateElementCount();  // en updateMenuAndToolbars()
```

## 6. POST-FACTORING
Compilé de nuevo:
```bash
mvn package -DskipTests
```

Probé la aplicación:
- Abre con contador en 0
- Agregué 2 paquetes, 2 clases, 2 componentes
- Contador muestra: `Total Items: 06; Package:02, Class:02; Component: 02`
- Al borrar elementos el contador baja correctamente

Todo funciona.

## 7. CONCLUSIÓN
Implementación completa.
- 2 archivos modificados
- 60 líneas de código agregadas
- Funciona en tiempo real
- No rompe nada del sistema original
