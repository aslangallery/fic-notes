---
Name: 5 - Desarrollo Componentes React
tags:
  - teoría
asignatura: PA
---
***[[Programación Avanzada]]***

**DESARROLLO BASADO EN COMPONENTES**
****
> Consiste en dividir la interfaz en partes más pequeñas llamados componentes. Cada uno de ellos genera una parte de la UI y responde a los eventos que produce el usuario cuando interactúa con esa parte.

***VENTAJAS***
- [p] *Divide y vencerás*: permite crear una UI compleja implementando partes mucho más simples.
- [p] *Reusabilidad*: permite reusar componentes dentro de la misma aplicación o en otras aplicaciones.

***PLATAFORMAS BASADAS EN COMPONENTES***
- SDK de plataformas de desarrollo de aplicaciones nativas de escritorio y móvil.
- Frameworks de aplicaciones web SPA (Angular, React).
- Frameworks de aplicaciones web del lado servidor (Tapestry, Wicket).

**REACT**
****
> Librería de JS que sigue el paradigma del desarrollo en componentes. Se adapta a diferentes entornos y tecnologías.

Sigue un enfoque minimalista, solo aporta lo mínimo necesario para el desarrollo del frontend, pero se complementa con la gran variedad de librerías desarrolladas por la comunidad.

***COMPONENTES***
Se puede implementar como una clase que hereda de `React.Component`. Puede redefinir los métodos que necesite, pero el obligatorio es `render`, que devuelve un markup del componente.

1. *Estado de los componentes*
	Se inicializa en el constructor y es una propiedad del objeto. Se puede leer con `this.state` y se puede actualizar con `this.setState`. Recibe un objeto con las propiedades que se desean modificar. Cuando se llama a este método, el componente se vuelve a renderizar.
2. *Ejemplo*
	```javascript
	// App.jsx
	import React from 'react';
	
	class App extends React.Component {
	
	    constructor(props) {
	        super(props);
	        this.state = { value: 0 };
	    }
	
	    handleIncrement() {
	        this.setState({ value: this.state.value + 1 });
	    }
	
	    handleDecrement() {
	        this.setState({ value: this.state.value - 1 });
	    }
	
	    handleReset() {
	        this.setState({ value: 0 });
	    }
	
	    render() {
	        return (
	            <div>
	                {this.state.value + ' '}  // Expresión JS
	                <button onClick={() => this.handleIncrement()}>+</button>
	                {' '}
	                <button onClick={() => this.handleDecrement()}>-</button>
	                {' '}
	                <button onClick={() => this.handleReset()}>Reset</button>
	            </div>
	        );
	    }
	
	}
	
	export default App;
	```

***JSX***
La implementación de `render` se hace en JSX. Es una extensión de JS que permite escribir código HTML de manera similar a la sintaxis de XML.
- *Expresiones JSX*
	Funcionan igual que una de JS.
	```jsx
	const element=<h1>Hola mundo!</h1>
	
	//Equivale a 
	const elemento=React.createElement("h1", null, "Hola mundo!");
	```
	Permite incluir expresiones JS dentro de ellas entre llaves.
	```jsx
	<button onClick={() => this.handleReset()}>Reset</button>
	```

**VITE**
****
> Herramienta que facilita el desarrollo de frontend en JS proporcionando plantillas para frameworks de JS, por ejemplo para aplicaciones SPA con React.

***ESTRUCTURA DE FICHEROS VITE***
1. *`index.html`*
	Es la base de la página web, contiene el `div` raíz.
	```html
	<html lang="en">
	  <head>
	    <meta charset="UTF-8" />
	    <link rel="icon" type="image/svg+xml" href="/favicon.ico" />
	    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
	    <title>PA Counter</title>
	  </head>
	  <body>
	    <div id="root"></div>
	    <script type="module" src="/src/main.jsx"></script>
	  </body>
	</html>
	```
2. *`main.jsx`*
	Renderiza la aplicación React en el elemento HTML con el ID `root`. Utiliza `React.StrictMode` para activar el modo estricto de React, que ayuda a identificar y evitar problemas en la aplicación durante el desarrollo.
	```jsx
	import React from 'react';
	import ReactDOM from 'react-dom/client';
	import App from './App';
	
	/* Render application. */
	const root = ReactDOM.createRoot(document.getElementById('root'));
	root.render(
	  <React.StrictMode> // Advertencias sobre fallos en React
	    <App/>
	  </React.StrictMode>
	);
	```

***MODOS DE TRABAJO***
1. *Modo desarrollo*
	Se hace con `npm run dev`. Arranca un servidor web local, que abre la página `index.html`. El resto de ficheros JS se van sirviendo al navegador a medida que sea necesario. Vite transforma los ficheros JSX en JS para que se puedan ejecutar en el navegador.
	Al modificar el código del frontend, se recarga automáticamente en el navegador.
2. *Modo producción*
	Se hace con `npm run build`. Genera una carpeta llamada `dist` que contiene el código transformado en JS minimizado en un solo fichero. También minimiza todo el CSS y lo incluye en `index.html`.

**HOOKS**
****
> Funciones que permiten acceder a características de React, como el estado de los componentes.

***VENTAJAS***
- [p] Reducir la dificultad de la lógica del estado entre componentes.
- [p] Evitar redefinir métodos de `React.Component`.
- [p] Evitar el uso de clases en JS.

***REGLAS DE USO***
Solo se pueden invocar desde componentes función o desde hooks a medida. Estos últimos son funciones que encapsulan varios hooks para encapsular lógica específica y reutilizable en componentes de React.

Los hooks se deben invocar siempre en el mismo orden. Para garantizarlo se debe evitar su uso en bucles, condicionales y funciones anidadas.

**VIRTUAL DOM**
****
> Contiene el árbol de elementos renderizados por los componentes.

Los elementos HTML de los JSX corresponden con los HTML del mismo nombre. Los nombres de los componentes creados por el desarrollador tienen que empezar por mayúscula. Los atributos van en camelCase. Existen algunas diferencias, como el uso de `className` en lugar de `class` de HTML para evitar conflictos con palabras reservadas en el lenguaje.

***VENTAJAS***
- [p] Evitar realizar actualizaciones completas del DOM.
- [p] Mantener siempre la UI sincronizada con el desarrollo.

***FUNCIONAMIENTO***
Cada vez que se renderiza un componente, React calcula la diferencia entre el resultado anterior y el nuevo. Después aplica las diferencias en el DOM del navegador.

**INMUTABILIDAD**
****
En una aplicación real no existe un único componente que mantiene todo el estado. Se renderizan de forma eficiente en el DOM del navegador, pero producen renderizaciones innecesarias en el Virtual DOM.

***MECANISMOS DE OPTIMIZACIÓN***
Asumen que el estado y las propiedades son inmutables. Se provoca que un componente solo se vuelva a renderizar si el valor de las propiedades es diferente al de la última renderización.

Se hace con la igualdad referencial o shallow comparison. Solo compara el nivel más superficial. Esto hace que el software sea muy eficiente.
