---
Name: 4 - NInject
tags:
  - teoría
asignatura: MaD
---
***[[Marcos de Desarrollo]]***

**INTRODUCCIÓN A NINJECT**
*****
>Forma sencilla de implementar el patrón de Inversión de control e Inyección de Dependencias.

Problemas originados por las dependencias:
- Código altamente acoplado.
- Aislamiento de código se complica. Se dificulta la realización de pruebas.
- Mantenimiento complejo. Desconocimiento acerca de los efectos colaterales de la modificación de código.

***Soluciones***
1. *Uso de factorías*
	Reduce dependencias directas, pero introduce nuevas dependencias hacia las factorías.
2. *Uso de contenedores IoC*
	Permite configurar y gestionar dependencias de manera centralizada.

**CONFIGURACIÓN DE NINJECT**
****
El contenedor almacenará las referencias a las clases o instancias que se inyectarán.

***Código***
Uso de `StandardKernel` para registrar dependencias.
```csharp
IKernel kernel = new StandardKernel();
kernel.Bind<IAccountService>().To<AccountService>();
```

***Archivo de configuración (XML)***
Útil para configuraciones menos dinámicas.
```csharp
<bind service="IAccountService" to="AccountService" />
```

**INYECCIÓN DE DEPENDENCIAS**
****
***Tipos de inyección***
1. *Propiedades*
	Uso del atributo `[Inject]` en propiedades para resolver dependencias.
2. *Constructor*
	Inyección de parámetros en constructores con configuraciones específicas.

**INTERCEPCIÓN**
****
- Soporte para programación orientada a aspectos.
- Permite interceptar métodos para agregar lógica adicional (e.g., gestión de transacciones).
```csharp
kernel.Bind<IAccountService>().To<AccountService>().Intercept().With<ExceptionInterceptor>();
```

***Tipos soportados de intercepción***
1. *Miembros/Propiedades*
	```csharp
	InterceptReplace<T>(Expression<Action<T>>, Action<IInvocation>)
	InterceptAround<T>(Expression<Action<T>>, 
					Action<IInvocation>,  Action<IInvocation>) 
	InterceptBefore<T>(Expression<Action<T>>, Action<IInvocation>)
	InterceptAfter<T>(Expression<Action<T>>, Action<IInvocation>)
	```
	```csharp
	Kernel.InterceptAround<AccountService>(
		s=>s.GetAllAccounts(), 
		invocation =>logger.Info("Retrieving all accouns..."),
		invocation =>logger.Debug("Accounts retrieved")); 
	
	Kernel.InterceptAfter<AccountService>( 
		s=>s.GetAllAccounts(), 
		invocation => logger.DebugFormat(
			"Total {0} accounts retrieved", 
			(IEnumerable) invocation.ReturnValue)
			.Count()
		)
	);
	```
2. *Tipos*
	Intercepción más genérica, intercepta todos los métodos de un tipo.
	El tipo ha de implementar la interfaz `IInterceptor`.
	```csharp
	public class ExceptionInterceptor : IInterceptor 
	{ 
		<...> 
		public void Intercept(Iinvocation invocation) 
		{ 
			try 
			{ 
				invocation.Proceed(); 
			} 
			catch(Exception e) { <…> } 
		}
	}
	```
	```csharp
	kernel.Bind<IAccountService>().To<AccountService>()
	.Intercept().With<ExceptionInterceptor>(); 
	
	//u otra opción 
	
	ExceptionInterceptor interceptor = new ExceptionIterceptor();
	kernel.Bind<IAccountService>().To<AccountService>()
	.Intercept().With<interceptor>(); 
	```
	