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

###@OneToMany bidireccional
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
