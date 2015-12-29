##Mapeo
Sección de cómo mapear los modelos.

###@Id
Cuando el id de la columna tiene un nombre diferente del defecto por ebean.
```mysql
create table monos(
    id_mono int not null primary key autoincrement,
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
```mysql
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



