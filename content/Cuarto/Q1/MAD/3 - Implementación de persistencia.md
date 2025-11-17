---
Name: 3 - Implementación de persistencia
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**INTRODUCCIÓN A ENTITY FRAMEWORK**
****
> Librería cuya idea es evitar pensar en la estructura de la BBDD.

El acceso a datos y el almacenamiento se hacen contra el modelo conceptual que crea el EDM (Entity Data Model).
Con Entitt Framework se recuperan directamente los objetos del dominio y se persisten los cambios en BBDD cuando es necesario.

***Entidades del modelo conceptual***
Las tablas que contiene el EDM se llaman entidades. Tienen propiedades, pero no comportamiento, excepto los métodos de seguimiento de cambios e `equals()`, `getHashCode()` y `toString()`.

***Características de EF***
- Permite una amplia serie de proveedores de BBDD, aunque se suele usar SQLServer.
- Proporciona varias APIs y herramientas.

***Metadatos***
Almacena el modelo conceptual en un XML. Contiene la definición del esquema de la BBDD y de las relaciones del modelo conceptual. Mediante los metadatos, realiza las transformaciones necesarias para pasar de modelo a BBDD y viceversa.

***Herramientas de diseño***
Proporciona una herramienta que permite trabajar de forma gráfica con el modelo de entidades sin tener que manipular el XML. También cuenta con un generador de código, que genera clases a partir del modelo.

***Object services***
Los objetos de servicio son una capa de servicios que proporciona `EntityObject`, que permite trabajar con `DbContext` para gestionar la materialización de objetos, la serialización, los cambios que se hacen sobre estos y la transmisión de consultas Linq a SQL.

***Seguimiento de cambios***
También lo hacen los objetos de servicio una vez se instancia un objeto entidad. Utilizan el seguimiento para poder crear, actualizar o borrar los datos de los objetos.

***Gestión de relaciones y claves foráneas***
EF convierte las relaciones en propiedades de navegación con **lazy loading** (retrasa la carga de datos hasta que se necesitan). 

***Entity Client***
Funciona como un puente entre `DbContext` y la BBDD. 
Permite realizar consultas, ejecutar comandos o recuperar resultados. La diferencia es que trabajando a tan bajo nivel se pueden obtener resultados en forma de tablas en vez de obtener objetos directamente.

**ENTITY DATA MODEL**
****
***Ventajas***
- Genera las clases automáticamente a partir del modelo.
- Se encarga de toda la conectividad con la BBDD.
- Proporciona consultas simples sobre el modelo.
- Realiza seguimiento de cambios.

***Creación del EDM***
El modelo de datos se puede crear de 3 formas:
- *DataBase First*: se crea la BBDD, con ella se genera el EDM y con este el código.
- *Model First*: se crea el EDM y a partir de este se genera tanto el código como la BBDD.
- *Code First*: no es recomendable. Crea la base de datos a partir del código.

***Cadena de conexión***
> La cadena de conexión es una configuración que define cómo una aplicación se conecta a una BBDD.

Al crear el EDM se genera y se guarda el archivo de configuración del proyecto.

***Metadatos del modelo***
A parte del modelo conceptual, también existen otros metadatos en EF: el mapping y el modelo lógico.
![[Pasted image 20250103125637.png]]
El modelo y los metadatos se pueden actualizar mediante el asistente de actualización si se han realizado cambios en la BBDD.

**CONSULTAS AL EDM**
****
***Formas de consultar el EDM***
Existen varias formas de consultar el EDM:
- Consultas Linq.
- EntitySQL.
- Mediante métodos especiales.
- EntityClient.

Las dos formas más a alto nivel y más cómodas son las 2 primeras, aunque cualquier forma es válida. Una consulta podría ser:
```csharp
static void Main(string[] args) 
{
    using (TestEntities context = new TestEntities()) 
    {
        DbSet accounts = context.Accounts;

        foreach (Account account in accounts) 
        {
            Console.WriteLine("Acc ID: {0}, Balance: {1}", account.accId, account.balance);
        }
    }
}
```

***Con Linq***
```csharp
using (TestEntities context = new TestEntities())
{
    List<Account> accounts = 
        (from a in context.Accounts
         where a.usrId == userId
         orderby a.accId
         select a)
        .Skip(startIndex)
        .Take(count)
        .ToList();

    foreach (Account account in accounts)
    {
        Console.WriteLine("Acc ID: {0}, Balance: {1}", account.accId, account.balance);
    }
}
```
Permite un mayor nivel de abstracción. Además tiene tipado fuerte, el compilador ya sabe que `userId` existe en el modelo.

***Con EntitySQL***
```csharp
using (TestEntities context = new TestEntities())
{
    String query = "SELECT VALUE a FROM Accounts AS a " +
                   "WHERE a.usrId = @userId " +
                   "ORDER BY a.accId";

    ObjectParameter param = new ObjectParameter("userId", userId);

    List<Account> accounts = context.Database
                                     .SqlQuery<Account>(query, param)
                                     .Skip(startIndex)
                                     .Take(count)
                                     .ToList();

    foreach (Account account in accounts)
    {
        Console.WriteLine("Acc ID: {0}, Balance: {1}", account.accId, account.balance);
    }
}
```
Su ventaja es que permite trabajar a más bajo nivel que Linq y está disponible para todos los lenguajes .NET.

**GESTIÓN DE ESTADOS**
****
Los datos de una instancia pueden pasar por los estados:
- *Detached*: no vinculados al contexto.
- *Unchanged*: vinculada al contexto y sin cambios, con valores de clave finales.
- *Added*: vinculados al contexto con valores de clave temporales.
- *Modified*: con cambios en las propiedades.
- *Deleted*: eliminados.

***Cambios entre estados***
Para pasar de un estado a otro se utilizan los métodos:
- *AddObject*: añade una instancia con el estado Added.
- *Attach*: vincula una entidad a Unchanged.
- *DeleteObject*: marca una entidad como Deleted.
- *AcceptAllChanges*: marca todas las entidades como Unchanged.
- *Detach*: elimina una entidad del contexto.
- *ChangeState y ChangeObjectState*: modifican el estado de una entidad a otro.
- *ApplyCurrentValues y ApplyOriginalValues*: cambian el estado a Modified.

![[Pasted image 20250103131126.png]]

**CONSTRUCCIÓN DE UN DAO GENÉRICO**
****
>El patrón DAO abstrae y encapsula todos los accesos al repositorio de datos, manejando en un único lugar la conexión con la fuente de datos.

Para ello, define una interfaz que proporciona acceso a las operaciones de persistente y oculta el tipo del repositorio de datos. Para cada repositorio se hace una implementación.

***Componentes***
- *BusinessObject*: servicio que necesita acceder al repositorio de datos.
- *DataAccessObject*: abstracción de los detalles de acceso al repositorio de datos.
- *DataSource*: fuente de datos.
- *TransferObject*: entidad intermedia entre el DAO y el negocio.

***Ventajas***
- Proporciona transparencia.
- Facilita la migración.
- Reduce la complejidad del código.
- Centraliza todo el acceso a datos.

***Inconvenientes***
- Añade una capa extra.
- No es útil en fuentes de datos que autogestionan la persistencia.
- Requiere jerarquía de clases.

***DAO genérico***
La interfaz del DAO genérico permite definir una serie de operaciones básicas CRUD muy utilizadas en las entidades.
```csharp
public interface IGenericDao<E, PK> {
    void Create(E entity);
    
    /// <exception cref="InstanceNotFoundException"></exception>
    E Find(PK id);
    
    Boolean Exists(PK id);
    
    void Update(E entity);
    
    /// <exception cref="InstanceNotFoundException"></exception>
    void Remove(PK id);
    
    List<E> GetAllElements();
}

----------

// Ejemplo de implementación de Create
public void Create(E entity) {
	context.AddObject(GetQualifiedEntitySetName(entityClass.Name), entity);
	context.SaveChanges();
}
```
Para implementar la persistencia se utiliza `context`, que es una propiedad obtenida mediante inyección de dependencias. Se pueden implementar operaciones más complejas con Linq o con EntitySQL.

***DAOs en entidades***
Para realizar un DAO en una entidad concreta se implementa el DAO genérico y se definen las operaciones necesarias. 
```csharp
public List<Account> FindByUserId(long userId, int startIndex, int count)
{
    DbSet<Account> accounts = Context.Set<Account>();
    
    var result = 
        (from a in accounts
         where a.usrId == userId
         orderby a.accId
         select a)
        .Skip(startIndex)
        .Take(count)
        .ToList();
        
    return result;
}
```
Otra forma es con un contexto de vida corta haciendo:
```csharp 
using (var accounts = new MiniBankEntities())
```
El problema es que hace referencia directa al nombre del contenedor y este puede variar.