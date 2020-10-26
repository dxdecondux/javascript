

# TypeScript Style Guide() {

*Un enfoque altamente razonable para TypeScript*


## Tabla de Contenido

  

1.  [Tipos](#tipos)
2.  [Referencias](#referencias)
3.  [Objetos](#objetos)
4.  [Arreglos](#arreglos)
5.  [Destructuring](#destructuring)
6.  [Cadenas de Texto](#cadenas-de-texto)
7.  [Funciones](#funciones)
8.  [Notación de Funciones de Flecha](#notación-de-funciones-de-flecha)
9.  [Clases y Constructores](#clases-y-constructores)
10.  [Módulos](#módulos)
11.  [Iteradores y Generadores](#iteradores-y-generadores)
12.  [Propiedades](#propiedades)
13.  [Variables](#variables)
14.  [Expresiones de comparación e igualdad](#expresiones-de-comparación-e-igualdad)
15.  [Bloques](#bloques)
16.  [Comentarios](#comentarios)
17.  [Espacios en blanco](#espacios-en-blanco)
18.  [Comas](#comas)
19.  [Puntos y Comas](#puntos-y-comas)
20.  [Convenciones de nomenclatura](#convenciones-de-nomenclatura)
21.  [Funciones de Acceso](#funciones-de-acceso) 
22.  [Captura de errores](#captura-de-errores)
23.  [Licencia](#licencia)



## Tipos

  

-  **Primitivos**: Cuando accedes a un tipo primitivo, manejas directamente su valor

  

+  `string`

+  `number`

+  `boolean`

+  `null`

+  `undefined`

  
```typescript
const  foo  :  number  =  1;
let bar :  number  = foo;
bar =  9;
console.log(foo, bar);  // => 1, 9
```

-  **Complejo**: Cuando accedes a un tipo complejo, manejas la referencia a su valor.


+  `object`

+  `array`

+  `function`

  

```javascript
const  foo  :  number  =  [1,  2];
const  bar  = foo;
bar[0]  =  9;
console.log(foo[0], bar[0]);  // => 9, 9
```

  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  

## Referencias

- Usa `const` para todas tus referencias; evita usar `var`.

> ¿Por qué? Esto asegura que no reasignes tus referencias, lo

que puede llevar a bugs y dificultad para comprender el código.


```javascript
// mal
var a : number = 1;
var b : number = 2;
 
// bien
const a: number = 1;
const b: number = 2;
```

  

- Si vas a reasignar referencias, usa `let` en vez de `var`.

> ¿Por qué? El bloque `let` es de alcance a nivel de bloque a

diferencia del alcance a nivel de función de `var`.

```javascript
// mal
var count: number = 1;
if  (true)  {
	count +=  1;
}

// bien, usa el let
let count: number = 1;
if (true) {
	count +=  1;
}
```

- Nota que tanto `let` como `const` tienen alcance a nivel de bloque.

```javascript
// const y let solo existen en los bloques donde
// estan definidos
{
	let a : number = 1;
	const b : number = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```

  

## Objetos

  

- Para los objetos definir una interfaz que haga referencia a sus propiedades y tipos

```javascript
interface SupermanInterface {
	defaults: {};
	hidden:boolean;
}

const superman: SupermanInterface = {
	defaults: { clark: 'kent' },
	hidden: true,
};
```

- Usa la sintaxis literal para la creación de un objeto.

```javascript
// mal
const item = new Object();
// bien
const item: Object = {};
```

- No uses [palabras reservadas](http://es5.github.io/#x7.6.1) para nombres de propiedades. No funciona en IE8 [Más información](https://github.com/airbnb/javascript/issues/61). No hay problema de usarlo en módulos de ES6 y en código de servidor.

  

```javascript
// mal
const superman: SupermanInterface = {
	default:  { clark:  'kent'  },
	private:  true
};

// bien
const superman: SupermanInterface = {
	defaults:  { clark:  'kent'  },
	hidden:  true
};
```

- Usa sinónimos legibles en lugar de palabras reservadas.


```javascript
// mal
const superman: SupermanInterface = {
	class: 'alien'
};

// mal
const superman: SupermanInterface = {
	klass:  'alien'
};

// bien
const superman: SupermanInterface = {
	type:  'alien'
};
```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  

## Arreglos

  

- Usa la sintaxis literal para la creación de arreglos


```javascript
// mal
const items = new Array();

// bien
const items: string[] = [];
```
- Al declarar matrices, se debe agregar un espacio al final después de cada delimitador de coma para mejorar la legibilidad.
```javascript
// mal
objetos = [1,2,3,4];

// bien
objetos = [1, 2, 3, 4];
```
- Al declarar una matriz indexada de varias líneas, el elemento de la matriz inicial debe comenzar en la siguiente línea. Si es así, debe manejar 2 tabs de espacio adicionales a la línea que contiene la declaración de la matriz, y todas las líneas sucesivas deben tener la misma sangría; el paréntesis de cierre debe estar en una línea por sí mismo en el mismo nivel de sangría que la línea que contiene la declaración de la matriz, por línea debemos como manejar como máximo 3 elementos.
```javascript
// bien
sampleArray = [
	1, 2, 3, 
	'Zend', 'Studio', 'Name',
	a, b, c,
	56.44, d, 500,
];
```

- Usa Array#push, en vez de asignación directa, para agregar elementos a un arreglo.

  
```javascript
const someStack: string[] = [];

// mal
someStack[someStack.length] = 'abracadabra';

// bien
someStack.push('abracadabra');
```

- Usa [spread de arrays](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Operadores/Spread_operator) para copiar arreglos.

 
```javascript
const items: number[] = [1,  2,  3,  4];
const len = items.length;
const itemsCopy: number[] = [];

let i :  number;

// mal
for (i = 0; i < len; i++)  {
	itemsCopy[i] = items[i];
}

// bien
const  itemsCopy: number[] = [...items];
```
- Para convertir un objeto ["array-like" (similar a un arreglo)](https://www.inkling.com/read/javascript-definitive-guide-david-flanagan-6th/chapter-7/array-like-objects) a un arreglo, usa Array#from.

 
```javascript
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```

  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  

## Destructuring

  

- Usa object destructuring cuando accedas y uses múltiples propiedades de un objeto.

  

> ¿Por qué? Destructuring te ahorra crear referencias temporales para esas propiedades.

 
```javascript
// mal
const getFullName = (user: string) => {
	const firstName = user.firstName;
	const lastName = user.lastName;
	return `${firstname} ${lastName}`;
}

// bien
const getFullName = ( user: UserInterface) => {
	const { firstName, lastName } = user;
	return `${firstName} ${lastName}`;
};

// mejor
const getFullName = ({ firstName, lastName }: UserInterface) => `${firstName} ${lastName}`;
```

  
- Usa array destructuring.

```javascript
const arr: number[] = [1, 2, 3, 4];

// mal
const first = arr[0];
const second = arr[1];

// bien
const [first, second] = arr;
```

- Usa object destructuring para múltiple valores de retorno de una función.

```javascript
// el que llama elige solo la data que necesita
const { left, top  } = processInput(input);
```
  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  
  

## Cadenas de Texto

  

- Usa comillas simples `''` para las cadenas de texto

  
```javascript
// mal
const name: string = "Bob Parr";

// bien
const name: string = 'Bob Parr';
```

  

- Las cadenas de texto con una longitud mayor a 100 caracteres deben ser escritas en múltiples líneas usando concatenación.

  

> **Nota:** Cuando se usa sin criterio, las cadenas de texto largas pueden impactar en el desempeño. [jsPerf](http://jsperf.com/ya-string-concat) & [Discusión](https://github.com/airbnb/javascript/issues/40)

  

```javascript
// mal
const errorMessage = 'This is a super long error that was thrown because'
+ 'of Batman. When you stop to think about how Batman had anything to do '
+ 'with this, you would get nowhere fast.';

// bien
const errorMessage = 'This is a super long error that was thrown because\
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere fast.';

// bien
const errorMessage = 'This is a super long error that was thrown because' +
'of Batman. When you stop to think about how Batman had anything to do ' +
'with this, you would get nowhere fast.';
```

  

- Cuando se crean cadenas de texto de forma programática, usa template strings (cadena de plantillas) en vez de concatenación.

  

> ¿Por qué? Los template strings te dan mayor legibilidad, sintaxis concisa con nuevas líneas apropiadas y capacidades de interpolación.

  

```javascript
// mal
const sayHi = (name: string) => 'How are you, ' + name + '?';

// mal
const sayHi = (name: string) => ['How are you, ', name, '?'].join();

// bien
const sayHi = (name:  string) => `How are you, ${name}?`;
```

- Nunca uses `eval()` en una cadena de texto, abre una caja de Pandora de vulnerabilidades.

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Funciones
- Usa expresiones de función en vez de declaración de función haciendo uso de las funciones de flecha.

```javascript
// mal
function foo() {}

// bien
const foo = () => {};
```

- Nunca declares una función en un bloque que no sea de función (if, while, etc). En vez de ello, asigna la función a una variable. Los navegadores te permitirán hacerlo pero todos ellos lo interpretarán de modo diferente, lo que es lamentable.

  
>  **Nota:** ECMA-262 define un bloque como una lista de sentencias. Una declaración de función no es una sentencia. [Lee la nota de ECMA-262 sobre este inconveniente](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

  
```javascript
// mal
if  ( currentUser ) {
	const test = () => {
		console.log('Nope.');
	}
}

// bien
let test: Function;
if ( currentUser ) {
	test = () => {
		console.log('Yup.');
	};
}
```

- Nunca nombres a un parámetro como `arguments`, esto tendrá precedencia sobre el objeto `arguments` que es brindado en cada ámbito de función.

```javascript

// mal
const nope = (name: string, options: string[], arguments: string[]) => {
	// ...algo...
}

// bien
const yup = (name: string, options: string[], args: string[]) => {
	// ...algo...
}
```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Notación de Funciones de Flecha

- Cuando debas usar funciones anónimas (como cuando pasas un callback inline), usa la notación de funciones de flecha.

> ¿Por qué? Crea una versión de la función que ejecuta en el contexto de `this`, lo que usualmente es lo que deseas, además que tiene una sintaxis más concisa.

> ¿Por qué no? Si tienes una función complicada, debes mover esa lógica fuera de su expresión de función nombrada.

```javascript
// mal
[1, 2, 3].map(function (x) {
	const y = x + 1;
	return x * y;
});

// bien
[1, 2, 3].map((x: number) => {
	const y = x + 1;
	return x * y;
});
```

- Si el cuerpo de la función consiste en una sola sentencia retornando una [expresión](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) sin efectos colaterales, omite las llaves y usa el retorno implícito.
De otro modo, mantén las llaves y usa una sentencia de retorno.
  
> ¿Por qué? Un edulcorante sintáctico. Se lee bien cuando múltiples funciones están encadenadas entre sí.

```javascript
// mal
[1, 2, 3].map((number: number) => {
	const  nextNumber: number = number + 1;
	`A string containing the ${nextNumber}.`;
});

// bien
[1, 2, 3].map((number: number) => `A string containing the ${number}.`);

// bien
[1, 2, 3].map((number: number) => {
	const nextNumber: number = number + 1;
	return `A string containing the ${nextNumber}.`;
});

// bien
[1, 2, 3].map((number: number, index: number) => ({
	[index]: number,
}));

// Sin efectos colaterales para retorno implícito
function foo(callback: Function) {
	const val = callback();
	if  (val === true)  {
	// Do something if callback returns true
	}
}

let bool = false;

// mal
foo(() => bool = true);

// bien
foo(() => {
	bool =  true;
});

```
- En caso que la expresión se expanda en varias líneas, envuélvela en paréntesis para una mejor legibilidad.

> ¿Por qué? Se observa claramente dónde empieza y termina la función.

```javascript
// mal
['get', 'post', 'put'].map((httpMethod: string) => Object.prototype.hasOwnProperty.call(
		httpMagicObjectWithAVeryLongName,
		httpMethod,
	)
);
  
// bien
['get', 'post', 'put'].map((httpMethod: string) => (
	Object.prototype.hasOwnProperty.call(
		httpMagicObjectWithAVeryLongName,
		httpMethod,
	)
));
```

- Definir en las funciones el tipo de dato que se retorna en la función, de lo contrario espeficiar void como valor de retorno.

 
```javascript
//Mal
const funcion = (name: string) => name;

//Bien
const funcion = (name: string) : string => `Hola ${name}`;

//Bien
const funcion = (name: string) : void => {
	console.log(`Hola ${name}`);
};
```

- Evita confundir la sintaxis de función de flecha (`=>`) con los operadores de comparación (`<=`, `>=`).

```javascript

// mal
const itemHeight = (item: ItemInterface) => item.height > 256 ? item.largeSize : item.smallSize;

// mal
const itemHeight = (item:  ItemInterface) => item.height >  256 ? item.largeSize : item.smallSize;

// bien
const itemHeight = (item:  ItemInterface) => (item.height > 256 ? item.largeSize : item.smallSize);

// bien
const itemHeight = (item:  ItemInterface) => {
	const { height, largeSize, smallSize } = item;
	return height > 256 ? largeSize : smallSize;
};
```
  

**[⬆ regresar a la Tabla de Contenido](#tabla-de-contenido)**

  

## Clases y Constructores

- Los nombres de las clases deben iniciar de manera capitalizada y el nombre debe hacer referencia a la clase.
```javascript
//mal
class casesLogic {
}
//bien
class CasesLogic {
}
```
- Siempre usa `class`. Evita manipular `prototype` directamente.

> ¿Por qué? La sintaxis `class` es más concisa y fácil con la cual lidiar.


```javascript
class Queue {
	queue: number[];

	constructor(contents = [])  {
		this.queue = [...contents];
	}

	pop() {
		const value = this.queue[0];
		this.queue.splice(0, 1);
		return value;
	}
}
```

  

- Métodos pueden retornar `this` para ayudar con el encadenamiento de métodos (*chaining*).


```javascript
// mal
Jedi.prototype.jump = function () {
	this.jumping = true;
	return  true;
};

Jedi.prototype.setHeight = function (height) {
	this.height = height;
};

const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// bien
class Jedi {
	jumping:boolean = false;

	height:number = 0;

	jump() {
		this.jumping = true;
		return  this;
	}

	setHeight(height) {
		this.height = height;
		return this;
	}
}

const luke = new Jedi();
luke.jump().setHeight(20);
```


- Está bien escribir un método `toString()` personalizado, solo asegúrate que funcione correctamente y no cause efectos colaterales.

  
```javascript
class Jedi {
	name: string;

	constructor(options?: Jedi)  {
		this.name = options.name || 'no name';
	}

	getName() {
		return this.name;
	}

	toString() {
		return `Jedi - ${this.getName()}`;
	}
}
```

  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  
## Módulos

- Siempre usa módulos (`import`/`export`) antes que un sistema de módulos no estándar. Siempre puedes transpilar a tu sistema de módulos preferido.

 
```javascript
// mal
const AirbnbStyleGuide = require('./AirbnbStyleGuide');
module.exports = AirbnbStyleGuide.es6;
  
// bien
import AirbnbStyleGuide from './AirbnbStyleGuide';
export default AirbnbStyleGuide.es6;

// mejor
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

  

- No uses imports con comodines (asterisco).

  

> ¿Por qué? Esto te asegura de tener una única exportación por defecto.

```javascript
// mal
import * as AirbnbStyleGuide from './AirbnbStyleGuide';

// bien
import AirbnbStyleGuide from './AirbnbStyleGuide';
```

- Y no exportes directamente lo que traigas de un import.

> ¿Por qué? A pesar que hacer las cosas en una línea es conciso, tener un modo claro de importar y un modo claro de exportar, hace las cosas consistentes.

```javascript
// mal
// filename es6.js
export { es6 as  default  } from './AirbnbStyleGuide';

// bien
// filename es6.js
import { es6 } from './AirbnbStyleGuide';
export default es6;
```

  

- Solo importa de una ruta en un mismo lugar.

> ¿Por qué? Tener varias líneas que importan de una misma ruta hace al código difícil de mantener.

```javascript
// mal
import foo from 'foo';

// … some other imports … //
import { named1, named2 } from 'foo';

// bien
import foo, { named1, named2 } from 'foo';

// bien
import foo, {
	named1,
	named2,
} from 'foo';
```

  

- No exportes las asociaciones (bindings) mutables.

> ¿Por qué? La mutación debe ser evitada en general, pero en particular cuando se exportan asociaciones (bindings) mutables. Mientras esta técnica puede ser necesaria para algunos casos especiales, en general solo referencias constantes deben ser exportadas.

  

```javascript
// mal
let foo: number = 3;
export { foo };

// bien
const foo: number = 3;
export foo;
```


- En módulos con una única exportación, prefiere la exportación por defecto sobre la exportación nombrada.


> ¿Por qué? Para forzar a que más archivos solo exporten una sola cosa, lo que es mejor para la legibilidad y mantenibilidad.

```javascript
// mal
export function foo() {}

// bien
const foo = () => { };
export default foo;
```

- Pon todos los `import`s encima de las sentencias de no importación.


> ¿Por qué? Desde que los `import`s son elevados (hoisted), mantenerlos en el inicio previene comportamientos sorpresivos.

  

```javascript
// mal
import foo from 'foo';
foo.init();
import bar from 'bar';

// bien
import foo from 'foo';
import bar from 'bar';

foo.init();
```

  

- Imports de multi-línea deben ser indentados como los arreglos multi-línea y literales de objeto.

  

> ¿Por qué? Las llaves deben seguir las mismas reglas de indentación como en otros bloques de llaves en la guía de estilos, así como las comas finales.

```javascript
// mal
import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

// bien
import {
	longNameA,
	longNameB,
	longNameC,
	longNameD,
	longNameE,
} from 'path';
```
  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  
  

## Propiedades

  
- Usa la notación de punto `.` cuando accedas a las propiedades.
  
```javascript

const luke: Jedinterface = {
	jedi: true,
	age: 28
};

// mal
const isJedi: boolean = luke['jedi'];

// bien
const isJedi: boolean = luke.jedi;
```

- Usa la notación subscript `[]` cuando accedas a las propiedades con una variable.


```javascript
const luke: Jedinterface  =  {
	jedi:  true,
	age:  28
};

function getProp(prop:  string)  {
	return luke[prop];
}

const isJedi = getProp('jedi');
```

  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Variables


- Siempre usa `const` para declarar constantes o `let` para declarar variables. No hacerlo resultará en variables globales. Debemos evitar contaminar el espacio global (global namespace).

  
```javascript
// mal
superPower:SuperPower = new SuperPower();

// bien
const  superPower: SuperPower= new SuperPower();
o
// bien
let aPower: string;
aPower = "fly"; // esto puede cambiar a otro poder posteriormente
```

- Siempre al declarar variables, definir el tipo de dato de la variable, para facilitar su comprensión y uso.

 
```javascript
// mal
let superPower = 'fly';
o
// bien
let superPower: string = 'fly';
```

 
- Usa una declaración `const` o `let` por variable.

 
> ¿Por qué? Es más fácil agregar nuevas declaraciones de variables de este modo, y no tendrás que preocuparte por reemplazar `;` por `,` o introducir diffs de sólo puntuación .


```javascript
// mal
const items: Items = getItems(),
	goSportsTeam: boolean = true,
	dragonball:string = 'z';

// mal
// (compara con lo de arriba y encuentra el error)
const items: Items = getItems(),
	goSportsTeam: boolean = true;
	dragonball:string = 'z';
  
// bien
const items: Items = getItems();
const goSportsTeam: boolean = true;
const dragonball:string = 'z';
```

  

- Agrupa tus `const`s y luego agrupa tus `let`s.

> ¿Por qué? Esto es útil cuando necesites asignar una variable luego dependiendo de una de las variables asignadas previamente.


```javascript
// mal
let i:number, len:number, dragonball:string,
items:Items = getItems(),
goSportsTeam:boolean = true;


// mal
let i:number;
const items:Items = getItems();
let dragonball:string;
const goSportsTeam:boolean = true;
let len;

// bien
const goSportsTeam:boolean = true;
const items:Items = getItems();
let dragonball:string;
let i:number;
let length:number;
```


**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Expresiones de comparación e igualdad

  

- Usa `===` y `!==` en vez de `==` y `!=` respectivamente.

- Las expresiones condicionales son evaluadas usando coerción con el método `ToBoolean` y siempre obedecen a estas reglas sencillas:

+  **Objects** son evaluados como **true** (se considera así al objeto vacío `{}` y arreglos sin contenido `[]`)

+  **Undefined** es evaluado como **false**

+  **Null** es evaluado como **false**

+  **Booleans** son evaluados como **el valor del booleano**

+  **Numbers** son evaluados como **false** si su valor es **+0**, **-0**, o **NaN**, de otro modo **true**

+  **Strings** son evaluados como **false** si es una cadena de texto vacía `''`, de otro modo son **true**

```javascript
if ([0] && []) {
	// true
	// un arreglo es un objeto (incluso uno vacío), los objetos son evaluados como true
}
```

 
- Usa atajos.


```javascript
// mal
if (name !== '') {
	// ...cosas...
}

// bien
if (name) {
	// ...cosas...
}
  
// mal
if (collection.length > 0) {
	// ...cosas...
}
 
// bien
if (collection.length) {
	// ...cosas...
}
```

- Para más información revisa [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) por Angus Croll

  
- Usa llaves para crear bloques en cláusulas `case` y `default` que contengan
 declaraciones léxicas (e.g. `let`, `const`, `function` y `class`).
  

> ¿Por qué? La declaración léxica es visible en todo el bloque `switch` pero solo se inicializa al ser asignado, lo que solo ocurre cuando el bloque `case` donde es declarado es alcanzado. Esto causa problemas cuando múltiples bloques `case` intentan definir la misma variable.

 
```javascript
// mal
switch (foo) {
	case 1:
		let x = 1;
		break;
	case 2:
		const y = 2;
		break;
	case 3:
		function f() {}
		break;
	default:
		bar();
}

// bien
switch (foo) {
	case 1: {
		let x: number = 1;
		break;
	}
	case 2: {
		const y: number = 2;
		break;
	}
	case 3: {
		const f = () => {}
		break;
	}
	default: {
		class C {} bar();
	}
}
```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**


## Bloques
  
- Usa llaves con todos los bloques, así no sea necesario, para facilitar la lectura.
  
```javascript
// mal
if (test)
return false;
  
// mal
if (test) return false;

// bien
if (test) {
	return false;
}
```
- Se debe especificar la definición de un bloque de código con un corchete al final y espacio para
generar nuestro código
```javascript
// mal
const funcion = (param: string) =>
{
}

// bien
const funcion = (param: string) => {
}
```

- Si estás usando bloques con ```if``` y ```else```, pon el ```else``` en la misma línea que el ```if```.

```javascript
// mal
if (test) {
	thing1();
	thing2();
}
else {
	thing3();
}

// bien
if (test) {
	thing1();
	thing2();
} else {
	thing3();
}
```


**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  
## Comentarios


- Usa `/** ... */` para comentarios de múltiples líneas. Incluye una descripción, especificación de tipos y valores para todos los parámetros y valores de retorno.

```javascript
/**
* Descripción de la función
*
* Larga descripción de la función
*
* @param {type} tag - Un parámetro tipo string
* @param {type=} tag - Un parámetro opcional
* @param {type} [tag] - Otra forma de parámetro opcional (JSDoc sintaxis)
* @author Name <email@email.com>
* @return {type} tag
*/
const make = (tag: string) : Element {
	// ...stuff...
	return element;
}
```

  

- Usa `//` para comentarios de una sola línea. Ubica los comentarios de una sola línea encima de la sentencia comentada. Deja una línea en blanco antes del comentario, a menos que sea la primera línea de un bloque.

```javascript
// mal
const active: boolean = true;  // is current tab

// bien
// is current tab
const active: boolean = true;

// bien
function getType()  {
	console.log('fetching type...');
	// set the default type to 'no type'
	const type:string = 'no type';
	return type;
}
```
  

- Agregando a tus comentarios los prefijos `FIXME` o `TODO`, ayudará a otros desarrolladores a entender rápidamente si estás apuntando a un problema que precisa ser revisado o si estás sugiriendo una solución al problema que debería ser implementado. Estos son diferentes a comentarios regulares en el sentido que requieren alguna acción. Las acciones son `FIXME -- necesito resolver esto` o `TODO -- necesita implementarse`.


- Usa `// FIXME:` para anotar problemas.

```javascript
class Calculator extends Abacus {
	constructor() {
		super();
		// FIXME: shouldn't use a global here
		total:number = 0;
	}
}
```

- Usa `// TODO:` para anotar soluciones a los problemas.


```javascript
class Calculator extends Abacus  {
	constructor() {
		super();
		// TODO: total should be configurable by an options param
		this.total = 0;
	}
}
```
  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  
  

## Indentación y espacios en blanco

  
- Usa indentaciones con dos tabs.

```javascript
// mal
const foo = () : void => {
∙∙const name;
}

// mal
function bar() : void {
∙∙∙∙const name;
}

// bien
const baz = () : void => {
\t\tconst name;
}

```

- Deja un espacio antes de la llave de apertura.

```javascript
// mal
dog.set('attr',{
	age: '1 year',
	breed: 'Bernese Mountain Dog'
});

// bien
dog.set('attr', {
	age: '1 year',
	breed: 'Bernese Mountain Dog'
});
```

- Deja un espacio antes del paréntesis de apertura en las sentencias de control (```if```, ```while```, etc.). No dejes espacios antes de la lista de argumentos en las invocaciones y declaraciones de funciones.

```javascript
// mal
if(isJedi){
	fight  ();
}

// bien
if (isJedi) {
	fight();
}
```

- Separa a los operadores con espacios.

```javascript
// mal
const x:number=y+5;

// bien
const x: number = y + 5;
```

- Usa indentación cuando uses métodos largos con 'chaining' (más de dos métodos encadenados). Emplea un punto adelante en cada nueva línea, lo que enfatiza que es un método llamado no una nueva sentencia.

```javascript
// mal
const  leds: string[] = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
.attr('width', (radius + margin) * 2).append('svg:g')
.attr('transform',  'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
.call(tron.led);

// bien
const  leds: string[] = stage.selectAll('.led')
	.data(data)
	.enter().append('svg:svg')
	.class('led', true)
	.attr('width', (radius + margin) * 2)
	.append('svg:g')
	.attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
	.call(tron.led);
```

- Deja una línea en blanco luego de los bloques y antes de la siguiente sentencia.
 
```javascript
// mal
if (foo) {
	return bar;
}
return baz;

// bien
if (foo) {
	return bar;
}

return baz;
 
// bien
const obj = {
	foo() {
	},

	bar() {
	}
};

return obj;
```
  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  
## Comas

  
- No se deben utilizar Comas al inicio de línea:

```javascript
// mal
const story = [
	once
	, upon
	, aTime
];

// bien
const story = [
	once,
	upon,
	aTime,
];

// mal

const hero = {
	firstName: 'Ada'
	, lastName: 'Lovelace'
	, birthYear: 1815
	, superPower: 'strength'
};

// bien
const hero = {
	firstName: 'Ada',
	lastName: 'Lovelace',
	birthYear: 1815,
	superPower: 'computers',
};
```

  

- Coma adicional al final: **Sip.**

> ¿Por qué? Esto lleva a diferenciales en git más claros. Además los transpiladores como Babel removerán la coma del final en el código transpilado lo que significa que no te tendrás que preocupar del [problema de la coma adicional al final](es5/README.md#commas) en navegadores antiguos.

  
```javascript
// mal
const creator: CreatorInterface = {
	firstName:  'Florence',
	lastName: 'Nightingale'
	inventorOf: ['coxcomb chart', 'modern nursing']
};

// bien
const creator: CreatorInterface = {
	firstName: 'Florence',
	lastName: 'Nightingale'
	inventorOf: ['coxcomb chart', 'modern nursing'],
};

//mal
const heroes: HeroInterface = [
	'Batman',
	'Superman'
];

// bien
const heroes: HeroInterface = [
	'Batman',
	'Superman',
];
```

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  

## Puntos y Comas

  
- Se debe usar siempre punto y coma para finalizar un bloque.

  
```javascript
// mal
const saludar = () => {
	return  'hola'
}

// bien
const saludar = () => {
	return  'hola';
};
```

  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  

## Convenciones de nomenclatura

  

- Evita nombres de una sola letra y abreviaciones. Sé descriptivo con tus nombres.

 
```javascript
// mal
const q = () => {
	// ...algo...
}

// bien
const query = () => {
	// ...algo...
}
```

- Usa camelCase cuando nombres tus objetos, funciones e instancias.


```javascript
// mal
const OBJEcttsssss = {};
const this_is_my_object = {};
const o = {};
function c() {}

// bien
var thisIsMyObject = {};
const thisIsMyFunction = () => {}
```

- Usa PascalCase cuando nombres constructores o clases.

```javascript
// mal
class user {
	constructor(options) {
		this.name = options.name;
	}
}

const bad = new user({
	name: 'nope'
});

// bien
class User {
	constructor(options: User) {
		this.name = options.name;
	}
}
const good = new User({
	name:  'yup',
});
```


- No uses prefijos ni sufijos de guiones bajo.

> ¿Por qué? JavaScript no tiene el concepto de privacidad en términos de propiedades o métodos. A pesar que un guión bajo como prefijo es una convención común para indicar que son "privados", la realidad es que estas propiedades son absolutamente públicas, y por ello, parte de tu contrato público de API. La convención del prefijo de guión bajo podría orientar a los desarrolladores a pensar erróneamente que un cambio a aquellos no será de impacto o que los tests no son necesarios.

```javascript

// mal
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
this._firstName = 'Panda';

// bien
this.firstName = 'Panda';
```

- Nunca guardes referencias a `this`. Usa funciones arrow o la función [#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)



```javascript
// mal
const funcion = () => {
	const self = this;

	return () => {
		console.log(self);
	};
}

// bien
const foo = () => {
	return ()  => {
		console.log(this);
	};
}
```


- El nombre del archivo base debe corresponder exactamente con el nombre de su export por defecto, o en su defecto, se le debe añadir el nombre de la carpeta y la convención del archivo con PascalCase.

```javascript
// contenido archivo 1
class CheckBox {
	// ...
}
export default CheckBox;

// contenido archivo 2
export default function fortyTwo() { return  42; }

// contenido archivo 3
class UsersController {
}

// en algún otro archivo
// mal
import CheckBox from './checkBox'; // importacion/exportacion PascalCase, nombre de archivo camelCase
import FortyTwo from './FortyTwo'; // importacion/nombre de archivo PascalCase, exportacion camelCase
import controller from './Users/controller'; // importacion/nombre de archivo PascalCase, exportacion camelCase

// bien
import CheckBox from './CheckBox'; // importacion/exportacion/nombre de archivo PascalCase
import fortyTwo from './fortyTwo'; // importacion/exportacion/nombre de archivo camelCase
// ^ soporta tanto insideDirectory.js e insideDirectory/index.js
import UsersController from './Users/controller'; //carpeta/archivo
```

  

- Usa camelCase cuando exportes por defecto una función. Tu nombre de archivo debe ser idéntico al nombre de tu función.

```javascript
function makeStyleGuide() {
}

export  default makeStyleGuide;
```

  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  
  

## Funciones de Acceso

  

- Funciones de acceso para las propiedades no son requeridas.

- No uses getters/setters de JavaScript ya que causan efectos colaterales no esperados y son difíciles de probar, mantener y razonar. En vez de ello, si creas funciones de acceso usa ```getVal()``` y ```setVal('hello')```.

  
```javascript
// Maintainable-JavaScript-Nicholas-C-Zakas
// mal
class Dragon {
	get age() {
		// ...
	}

	set age(value: number) {
		// ...
	}
}

// bien
class Dragon {
	getAge() {
		// ...
	}
	 
	setAge(value: number)  {
		// ...
	}
}
```
  

- Si la propiedad es un booleano, usa ```isVal()``` o ```hasVal()```.

  

```javascript
// mal
if  (!dragon.age())  {
	return  false;
}

// bien
if  (!dragon.hasAge())  {
	return  false;
}
```

- Está bien crear funciones ```get()``` y ```set()```, pero sé consistente.

 
```javascript
class Jedi {

	constructor(options = {})  {
		const lightsaber = options.lightsaber || 'blue';
		this.set('lightsaber', lightsaber);
	}

	set(key: string, val: string)  {
		this[key] = val;
	}

	get(key: string)  {
		return this[key];
	}
}
```

  

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  
## Captura de errores

- Siempre se debe encapsular todo el código dentro de una sentencia try catch.

```javascript
const functionName = () => {
	try {
		const getUsers = await this.getUsers();	
	} catch ( error ) {
		throw new Error('Mensaje');
	}
};
```
**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

## Recursos

  

**Learning ES6**

  

-  [Vistazo comprensivo de las nuevas características de ES6](http://es6-features.org/)

  

**Lecturas más profundas**

  

-  [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban (Características de ES6)

  

## Licencia

  

(The MIT License)

  
Copyright (c) 2020 Decondux


Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:


The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[[⬆ regresar a la Tabla de Contenido]](#tabla-de-contenido)**

  

# };
