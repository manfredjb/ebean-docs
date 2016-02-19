##Mapeo
Sección de cómo mapear los modelos.

###@Id
Cuando el id de la columna tiene un nombre diferente del defecto por ebean.
```sql
create table monos(
    id_mono int not null primary key auto_increment,
```

Definición:
```java
@Id
@Column(name="id_mono")
private int id;
```


##Relaciones
Ejemplos de relaciones.

###@OneToOne
Un país tiene una bandera.
```sql
-- tabla de banderas
create table banderas(
    id_bandera int not null primary key auto_increment,
    
-- tabla de países
create table paises(
    id_pais int not null primary key auto_increment,
    bandera int not null
```

Definición:
```java
/**
 * Clase Bandera
 */
public class Bandera{

  @Id
  @Column(name="id_bandera")
  private int id;
  
/**
 * Clase Pais
 */
public class Pais{

    @Id
    @Column(name="id_pais")
    private int id;
    
    @OneToOne
    @JoinColumn(name="bandera", referencedColumnName="id_bandera")
    private Bandera bandera;
  ```

###@OneToMany
Una automóvil tiene muchos pasajeros.
```sql
-- tabla de automóviles
create table automoviles(
    id_automovil int not null primary key auto_increment,
    
-- tabla de pasajeros
create table pasajeros(
    id_pasajero int not null primary key auto_increment,
    automovil int not null,
);
```

Definición:
```java
/**
 * Clase Automovil
 */
public class Automovil{

    @Id
    @Column(name="id_automovil")
    private int id;
    
    @OneToMany
    @JoinColumn(name = "automovil", referencedColumnName = "id_automovil")
    private List<Pasajero> pasajeros;
```
    
###@OneToMany bidireccional
Usando el mismo ejemplo y estructura de la base de datos del ejemplo pasado.
```java
/**
 * Clase Automovil
 */
public class Automovil{

    @Id
    @Column(name="id_automovil")
    private int id;
    
    @OneToMany(mappedBy = "automovil")
    private List<Pasajero> pasajeros;
```

> **mappedBy = "automovil"** contiene el nombre del columna que referencia al padre desde la tabla hija.
 
```java   
/**
 * Clase Pasajero
 */
public class Pasajero{

    @Id
    @Column(name="id_pasajero")
    private int id;
    
    @ManyToOne
    @JoinColumn(name = "automovil", referencedColumnName = "id_automovil")
    private Automovil automovil;
```

###@ManyToMany bidireccional
Una lista de reproducción de puede tener muchas canciones, y una canción puede pertenecer a muchas listas de reproducción.
```sql
-- tabla de listas de reproducción
create table listas_reproduccion(
    id_lista int not null primary key auto_increment,
    
-- tabla de listas de canciones
create table canciones(
    id_cancion int not null primary key auto_increment,
    
-- table intermedia de referencias
create table canciones_enlistadas(
    id_cancion_enlistada int not null primary key auto_increment, -- evitar duplicados
    cancion int not null,
    lista int not null
);
```
Definición:
```java   
/**
 * Clase Canción
 */
public class Cancion{

    @Id
    @Column(name="id_cancion")
    
    @ManyToMany(mappedBy = "lista")
    private List<ListaReproduccion> listas;
```
> **mappedBy = "canciones"** contiene en padre el nombre de la lista de los hijos

```java 
/**
 * Clase ListaReproduccion
 */
public class ListaReproduccion{

    @Id
    @Column(name="id_lista_reproduccion")
    
    @ManyToMany(cascade=CascadeType.ALL)
    @JoinTable(name="canciones_enlistadas",
        joinColumns=
            @JoinColumn(name="lista", referencedColumnName="id_lista"),
        inverseJoinColumns=
            @JoinColumn(name="cancion", referencedColumnName="id_cancion"))
    private List<Cancion> canciones;
```

##Consideraciones
Las siguientes consideraciones previenen errores y dan robustez al proyecto.

1. Siempre llamar a los valores de los atributos por los métodos set y get, en especial cuando se tratan de relaciones.

##Manejo de excepciones
Causas de algunas excepciones.

* La clase/modelo Join no tiene el contructor por defecto:
    
    > java.lang.RuntimeException: java.lang.NoSuchMethodException: com.monkey.models.Join.<init>()

* La clase/modelo Preference no tiene el contructor por defecto o su constructor por defecto está vacío:

    > Caused by: com.avaje.ebean.ValidationException: validation failed for: com.monkey.models.Preference


