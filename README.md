## Next 13

- Nuevo sistema de enrutado ( aun esta disponible el anterior para la migracion)

- Por defecto cualquier componente que este en app, sera cargando desde el servidor, pero en caso de no querer esto podemos usar:
  'use client'

- Streaming: Permite el renderizado dinamico

- TurboPack: empaquetamiento

- Fuentes: nuevo sistema de carga de fuentes, se cargaran desde el servidor (npm install @next/font)

- Generar social cards

### Estructura de carpetas

Notarás que el proyecto tiene la siguiente estructura de carpetas:

- /app: contiene todas las rutas, componentes y lógica de su aplicación; aquí es desde donde trabajará principalmente.

- /app/lib: contiene funciones utilizadas en su aplicación, como funciones de utilidad reutilizables y funciones de recuperación de datos.

- /app/ui: contiene todos los componentes de la interfaz de usuario de su aplicación, como tarjetas, tablas y formularios. Para ahorrar tiempo, hemos prediseñado estos componentes para usted.

- /public: contiene todos los activos estáticos de su aplicación, como imágenes.

- /scripts: contiene un script de inicialización que utilizará para completar su base de datos en un capítulo posterior.
  Archivos de configuración: también notará archivos de configuración como next.config.js en la raíz de su aplicación. La mayoría de estos archivos se crean y preconfiguran cuando inicias un nuevo proyecto usando create-next-app. No necesitarás modificarlos en este curso.

### Estilo CSS

#### Global styles

La carpeta /app/ui, verás un archivo llamado global.css. Puede utilizar este archivo para agregar reglas CSS a todas las rutas de su aplicación, como reglas de restablecimiento de CSS, estilos de todo el sitio para elementos HTML como enlaces y más.

Puede importar global.css en cualquier componente de su aplicación, pero generalmente es una buena práctica agregarlo a su componente de nivel superior. (/app/layout.tsx)

#### CSS Modules

Los módulos CSS le permiten aplicar CSS a un componente creando automáticamente nombres de clase únicos, por lo que no tiene que preocuparse también por las colisiones de estilos

import styles from '@/app/ui/home.module.css';

```jsx
<div className={styles.shape} />
```

### Usando la biblioteca clsx para alternar nombres de clases

Puede haber casos en los que necesite aplicar un estilo condicional a un elemento según el estado o alguna otra condición.

```jsx
 <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-sm',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    >
```

### Optimización de fuentes e imágenes

¿Por qué optimizar las fuentes?

**Cumulative Layout Shift** es una métrica utilizada por Google para evaluar el rendimiento y la experiencia del usuario de un sitio web. Con las fuentes, el cambio de diseño ocurre cuando el navegador inicialmente muestra el texto en una fuente alternativa o del sistema y luego lo cambia por una fuente personalizada una vez que se ha cargado. Este intercambio puede hacer que el tamaño, el espaciado o el diseño del texto cambien, desplazando elementos a su alrededor.

Next.js optimiza automáticamente las fuentes en la aplicación cuando usa el módulo next/font. (Alberga archivos de fuentes con otros recursos estáticos para que no haya solicitudes de red adicionales.)

**_Agregar una fuente principal_**

```jsx
// app/ui/fonts.ts
import { Inter } from 'next/font/google';
export const inter = Inter({ subsets: ['latin'] });
```

Al agregar Inter al elemento <body>, la fuente se aplicará en toda su aplicación.

```jsx
// app/layout.tsx

import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

**_Agregar una fuente secundaria_**

```jsx
// app/ui/fonts.ts
import { Inter, Lusitana } from 'next/font/google';

export const inter = Inter({ subsets: ['latin'] });

export const lusitana = Lusitana({
  weight: ['400', '700'],
  subsets: ['latin'],
});
```

```jsx
// app/page.tsx
import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
import { lusitana } from '@/app/ui/fonts';

export default function Page() {
  return (
    // ...
    <p
      className={`${lusitana.className} text-xl text-gray-800 md:text-3xl md:leading-normal`}
    >
      <strong>Welcome to Acme.</strong> This is the example for the{' '}
      <a href="https://nextjs.org/learn/" className="text-blue-500">
        Next.js Learn Course
      </a>
      , brought to you by Vercel.
    </p>
    // ...
  );
}
```

### ¿Por qué optimizar imágenes?

Next.js puede ofrecer recursos estáticos, como imágenes, en la carpeta /public de nivel superior.

```jsx
<img
  src="/hero.png"
  alt="Screenshots of the dashboard project showing desktop version"
/>
```

Sin embargo, esto significa que tienes que hacerlo manualmente:

- Asegúrese de que su imagen responda en diferentes tamaños de pantalla.
- Especifique tamaños de imagen para diferentes dispositivos.
- Evite cambios de diseño a medida que se cargan las imágenes.
- Carga diferida de imágenes que están fuera de la ventana gráfica del usuario.

Puede utilizar el componente next/imagen para optimizar automáticamente sus imágenes.

### The Image component

El componente <Imagen> es una extensión de la etiqueta HTML <img> y viene con optimización automática de la imagen, como por ejemplo:

- Evitar el cambio de diseño automáticamente cuando se cargan las imágenes.
- Cambiar el tamaño de las imágenes para evitar enviar imágenes grandes a dispositivos con una ventana gráfica más pequeña.
- Carga diferida de imágenes de forma predeterminada (las imágenes se cargan a medida que ingresan a la ventana gráfica).
- Ofrecer imágenes en formatos modernos, como WebP y AVIF, cuando el navegador lo admite.

```jsx
import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
import { lusitana } from '@/app/ui/fonts';
import Image from 'next/image';

export default function Page() {
  return (
    // ...
    <div className="flex items-center justify-center p-6 md:w-3/5 md:px-28 md:py-12">
      {/* Add Hero Images Here */}
      <Image
        src="/hero-desktop.png"
        width={1000}
        height={760}
        className="hidden md:block"
        alt="Screenshots of the dashboard project showing desktop version"
      />
    </div>
    //...
  );
}
```

Es una buena práctica establecer el ancho y el alto de las imágenes para evitar cambios en el diseño; estas deben tener una relación de aspecto idéntica a la imagen de origen.

### Crear diseños y páginas

#### enrutamiento anidado

Next.js utiliza enrutamiento del sistema de archivos donde se usan carpetas para crear rutas anidadas. Cada carpeta representa un segmento de ruta que se asigna a un segmento de URL.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Ffolders-to-url-segments.png&w=1920&q=75&dpl=dpl_7YxV6fFeY6qNyh4dohxRZdsaFAP7 'Logo Title Text 1')

Puede crear interfaces de usuario independientes para cada ruta utilizando los archivos layout.tsx y page.tsx.

page.tsx es un archivo Next.js especial que exporta un componente de React y es necesario para que la ruta sea accesible. En su aplicación, ya tiene un archivo de página: /app/page.tsx: esta es la página de inicio asociada con la ruta /.

Para crear una ruta anidada, puede anidar carpetas una dentro de otra y agregar archivos page.tsx dentro de ellas.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fdashboard-route.png&w=1920&q=75&dpl=dpl_7YxV6fFeY6qNyh4dohxRZdsaFAP7 'Logo Title Text 1')

/app/dashboard/page.tsx está asociado con la ruta /dashboard.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Frouting-solution.png&w=1920&q=75&dpl=dpl_7YxV6fFeY6qNyh4dohxRZdsaFAP7 'Logo Title Text 1')

```jsx
// app/dashboard/customers/page.tsx
export default function Page() {
  return <p>Customers Page</p>;
}
```

En Next.js, puede utilizar un archivo layout.tsx especial para crear una interfaz de usuario que se comparte entre varias páginas.

```jsx
// app/dashboard/layout.tsx
import SideNav from '@/app/ui/dashboard/sidenav';

export default function Layout({ children }: { children: React.ReactNode }) {
 return (
   <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
     <div className="w-full flex-none md:w-64">
       <SideNav />
     </div>
     <div className="flex-grow p-6 md:overflow-y-auto md:p-12">{children}</div>
   </div>
 );
}
```

- Un beneficio de usar diseños en Next.js es que durante la navegación, solo se actualizan los componentes de la página, mientras que el diseño no se vuelve a representar. Esto se llama renderizado parcial:

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fpartial-rendering-dashboard.png&w=1920&q=75&dpl=dpl_7uN5hm5ManxexfA2rBqDFeGho3if 'Logo Title Text 1')

Esto se llama diseño raíz y es obligatorio. Cualquier interfaz de usuario que agregue al diseño raíz se compartirá en todas las páginas de su aplicación. Puede utilizar el diseño raíz para modificar sus etiquetas <html> y <body> y agregar metadatos (aprenderá más sobre los metadatos en un capítulo posterior).

Dado que el nuevo diseño que acaba de crear (/app/dashboard/layout.tsx) es exclusivo de las páginas del panel, no necesita agregar ninguna interfaz de usuario al diseño raíz anterior.

```jsx
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';

export default function RootLayout({
 children,
}: {
 children: React.ReactNode;
}) {
 return (
   <html lang="en">
     <body className={`${inter.className} antialiased`}>{children}</body>
   </html>
 );
}
```

### Navegando entre páginas

El componente <Link>
En Next.js, puede utilizar el componente <Link /> para vincular páginas en su aplicación. <Enlace> le permite realizar navegación del lado del cliente con JavaScript.

**Automatic code-splitting and prefetching**

División automática de código y captación previa.Para mejorar la experiencia de navegación, el código de Next.js divide automáticamente su aplicación por segmentos de ruta. Esto es diferente de un React SPA tradicional, donde el navegador carga todo el código de su aplicación en la carga inicial.

Dividir el código por rutas significa que las páginas quedan aisladas. Cada vez que aparecen componentes <Link> en la ventana gráfica del navegador, Next.js busca automáticamente el código para la ruta vinculada en segundo plano. Cuando el usuario hace clic en el enlace, el código de la página de destino ya estará cargado en segundo plano

**_Pattern: Showing active links_**

Un patrón de interfaz de usuario común es mostrar un enlace activo para indicar al usuario en qué página se encuentra actualmente. Para hacer esto, necesita obtener la ruta actual del usuario desde la URL. Next.js proporciona un gancho llamado usePathname() que puede usar para verificar la ruta e implementar este patrón.

Dado que usePathname() es un enlace, deberá convertir nav-links.tsx en un componente de cliente. Agregue la directiva **"use client"**.

## Configurando su base de datos

- Se creo repositorio
- Se subio a vercel
- Se creo semilla, pára cargar datos iniciales

## Fetching Data

Las diferentes formas en que puede obtener datos para su aplicación.

Las **API** son una capa intermediaria entre el código de su aplicación y la base de datos. Hay algunos casos en los que podría utilizar una API:

- Si está utilizando servicios de terceros que proporcionan una API.
- Si está obteniendo datos del cliente, querrá tener una capa API que se ejecute en el servidor para evitar exponer los secretos de su base de datos al cliente.
- En Next.js, puede crear puntos finales de API utilizando controladores de ruta.

**Consultas de bases de datos**
Al crear sus puntos finales de API, necesita escribir lógica para interactuar con su base de datos.
Si está utilizando componentes de servidor React (obteniendo datos en el servidor), puede omitir la capa API y consultar su base de datos directamente sin correr el riesgo de exponer los secretos de su base de datos al cliente.

**Uso de componentes del servidor para recuperar datos**

De forma predeterminada, las aplicaciones Next.js utilizan componentes de React Server. Los componentes del servidor respaldan las promesas y brindan una solución más simple para tareas asincrónicas como la recuperación de datos.

Los componentes del servidor se ejecutan en el servidor, por lo que puede mantener costosas recuperaciones de datos y lógica en el servidor y solo enviar el resultado al cliente. Como se mencionó anteriormente, dado que los componentes del servidor se ejecutan en el servidor, puede consultar la base de datos directamente sin una capa API adicional.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fcomplete-dashboard.png&w=1080&q=75&dpl=dpl_7uN5hm5ManxexfA2rBqDFeGho3if 'Logo Title Text 1')

Las solicitudes de datos se bloquean entre sí sin querer, creando una cascada de solicitudes.
De forma predeterminada, Next.js prerenderiza rutas para mejorar el rendimiento, esto se denomina renderizado estático. Entonces, si sus datos cambian, no se reflejarán en su panel.

**¿Qué son las cascadas de solicitudes?**
Una "cascada" se refiere a una secuencia de solicitudes de red que dependen de la finalización de solicitudes anteriores. En el caso de la recuperación de datos, cada solicitud solo puede comenzar una vez que la solicitud anterior haya devuelto datos.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fsequential-parallel-data-fetching.png&w=1920&q=75&dpl=dpl_7uN5hm5ManxexfA2rBqDFeGho3if 'Logo Title Text 1')

Por ejemplo, debemos esperar a que se ejecute fetchRevenue() antes de que fetchLatestInvoices() pueda comenzar a ejecutarse, y así sucesivamente.

Este patrón no es necesariamente malo. Puede haber casos en los que desee cascadas porque desea que se cumpla una condición antes de realizar la siguiente solicitud. Sin embargo, este comportamiento también puede ser involuntario y afectar el rendimiento.

**Parallel data fetching**

Obtención de datos en paralelo
Una forma común de evitar cascadas es iniciar todas las solicitudes de datos al mismo tiempo, en paralelo.

En JavaScript, puede utilizar las funciones Promise.all() o Promise.allSettled() para iniciar todas las promesas al mismo tiempo.

Al usar este patrón, puedes:

Comience a ejecutar todas las recuperaciones de datos al mismo tiempo, lo que puede generar mejoras en el rendimiento. Sin embargo, existe una desventaja de confiar únicamente en este patrón de JavaScript: ¿qué sucede si una solicitud de datos es más lenta que todas las demás?

## Static and Dynamic Rendering

Representación estática y dinámica.
analizamos brevemente dos limitaciones de la configuración actual:

- Las solicitudes de datos están creando una cascada involuntaria.
- El panel es estático, por lo que las actualizaciones de datos no se reflejarán en su aplicación.

**¿Qué es el renderizado estático?**

Con la representación estática, la obtención y representación de datos se realiza en el servidor en el momento de la compilación (cuando se implementa) o durante la revalidación. Luego, el resultado se puede distribuir y almacenar en caché en una red de entrega de contenido (CDN).

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fstatic-site-generation.png&w=1920&q=75&dpl=dpl_7uN5hm5ManxexfA2rBqDFeGho3if 'Logo Title Text 1')

Cada vez que un usuario visita su aplicación, se muestra el resultado almacenado en caché. Hay un par de beneficios del renderizado estático:

**Sitios web más rápidos:** el contenido prerenderizado se puede almacenar en caché y distribuir globalmente. Esto garantiza que los usuarios de todo el mundo puedan acceder al contenido de su sitio web de forma más rápida y fiable.

**Carga reducida del servidor:** debido a que el contenido se almacena en caché, su servidor no tiene que generar contenido dinámicamente para cada solicitud de usuario.

**SEO:** el contenido prerenderizado es más fácil de indexar para los rastreadores de los motores de búsqueda, ya que el contenido ya está disponible cuando se carga la página. Esto puede conducir a una mejor clasificación en los motores de búsqueda.

La representación estática es útil para la interfaz de usuario sin datos o datos que se comparten entre usuarios, como una publicación de blog estática o una página de producto. Puede que no sea una buena opción para un panel que tenga datos personalizados que se actualicen periódicamente.

**¿Qué es el renderizado dinámico?**

Con la representación dinámica, el contenido se presenta en el servidor para cada usuario en el momento de la solicitud.

Hay un par de beneficios del renderizado dinámico:

- Datos en tiempo real: la representación dinámica permite que su aplicación muestre datos en tiempo real o actualizados con frecuencia. Esto es ideal para aplicaciones donde los datos cambian con frecuencia.
- Contenido específico del usuario: es más fácil ofrecer contenido personalizado, como paneles o perfiles de usuario, y actualizar los datos en función de la interacción del usuario.
- Solicitar información sobre el tiempo: la representación dinámica le permite acceder a información que solo se puede conocer en el momento de la solicitud, como las cookies o los parámetros de búsqueda de URL.

**Dinamizar el tablero**

De forma predeterminada, @vercel/postgres no establece su propia semántica de almacenamiento en caché. Esto permite que el marco establezca su propio comportamiento estático y dinámico.

Puede utilizar una API de Next.js llamada unstable_noStore dentro de los componentes de su servidor o funciones de recuperación de datos para optar por no participar en la representación estática. Agreguemos esto.

```jsx
// This is equivalent to in fetch(..., {cache: 'no-store'}).
noStore();
```

Adenas,sea ha agregado un retraso artificial de 3 segundos para simular una recuperación de datos lenta. El resultado es que ahora toda su página está bloqueada mientras se recuperan los datos.

Lo que nos lleva a un desafío común que los desarrolladores deben resolver:

Con el renderizado dinámico, su aplicación es tan rápida como su recuperación de datos más lenta.

## Streaming

Analizamos cómo las recuperaciones lentas de datos pueden afectar el rendimiento de su aplicación. Veamos cómo puede mejorar la experiencia del usuario cuando hay solicitudes de datos lentas.

**¿Qué es la transmisión?**
La transmisión por secuencias es una técnica de transferencia de datos que le permite dividir una ruta en "fragmentos" más pequeños y transmitirlos progresivamente desde el servidor al cliente a medida que estén listos.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fserver-rendering-with-streaming.png&w=1920&q=75&dpl=dpl_E56Ygpboctpqcw3EfeNYXzcVdRrS 'Logo Title Text 1')

Al transmitir, puede evitar que las solicitudes de datos lentas bloqueen toda su página. Esto permite al usuario ver e interactuar con partes de la página sin esperar a que se carguen todos los datos antes de poder mostrarle cualquier interfaz de usuario.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fserver-rendering-with-streaming-chart.png&w=1920&q=75&dpl=dpl_E56Ygpboctpqcw3EfeNYXzcVdRrS 'Logo Title Text 1')

La transmisión funciona bien con el modelo de componentes de React, ya que cada componente puede considerarse un fragmento.

Hay dos formas de implementar la transmisión en Next.js:

- A nivel de página, con el archivo loading.tsx.
- Para componentes específicos, con < Suspense >.
  Veamos cómo funciona esto.

**Transmitir una página completa con loading.tsx**

```jsx
// app/dashboard/loading.tsx
export default function Loading() {
  return <div>Loading...</div>;
}
```

loading.tsx es un archivo Next.js especial creado sobre Suspense, que le permite crear una interfaz de usuario alternativa para mostrarla como reemplazo mientras se carga el contenido de la página.Como < SideNav > es estático, se muestra inmediatamente.

**_Adding loading skeletons_**

Muchos sitios web los utilizan como marcador de posición (o respaldo) para indicar a los usuarios que el contenido se está cargando.

Cualquier interfaz de usuario que incorpore en loading.tsx se incrustará como parte del archivo estático y se enviará primero.

```jsx
// app/dashboard/loading.tsx
import DashboardSkeleton from '@/app/ui/skeletons';

export default function Loading() {
  return <DashboardSkeleton />;
}
```

Dado que loading.tsx tiene un nivel superior a /invoices/page.tsx y /customers/page.tsx en el sistema de archivos, también se aplica a esas páginas.

Podemos cambiar esto con Grupos de Rutas. Cree una nueva carpeta llamada /(descripción general) dentro de la carpeta del panel. Luego, mueva sus archivos loading.tsx y page.tsx dentro de la carpeta:

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Froute-group.png&w=1920&q=75&dpl=dpl_E56Ygpboctpqcw3EfeNYXzcVdRrS 'Logo Title Text 1')

**_Arreglando el error del esqueleto de carga con grupos de rutas_**

En este momento, su esqueleto de carga se aplicará también a las páginas de facturas y clientes.

Podemos cambiar esto con Grupos de Rutas. Cree una nueva carpeta llamada:

- (overview)

Luego, mueva sus archivos loading.tsx y page.tsx dentro de la carpeta. Ahora, el archivo loading.tsx solo se aplicará a la página de descripción general de su panel.

Cuando crea una nueva carpeta usando paréntesis (), el nombre no se incluirá en la ruta URL. Entonces /dashboard/(overview)/page.tsx se convierte en /dashboard.

**_Streaming a component_**

Suspense le permite diferir la renderización de partes de su aplicación hasta que se cumpla alguna condición (por ejemplo, se cargan los datos)

Si recuerda la solicitud de datos lenta, fetchRevenue(), esta es la solicitud que está ralentizando toda la página. En lugar de bloquear su página, puede usar Suspense para transmitir solo este componente y mostrar inmediatamente el resto de la interfaz de usuario de la página.

En pocas palabras se movio la peticion adentro del componente, y se agrego Suspense por fuera del componente.

**_Componentes de agrupación_**

Para crear un efecto más escalonado, puedes agrupar las tarjetas usando un componente contenedor. Esto significa que primero se mostrará el < SideNav /> estático, seguido de las tarjetas, etc.

Decidir dónde colocar tus límites de suspenso
El lugar donde coloques tus límites de suspenso dependerá de algunas cosas:

- Cómo desea que el usuario experimente la página mientras se transmite.
- Qué contenido quieres priorizar.
- Si los componentes dependen de la recuperación de datos.

## Prerenderizado parcial (opcional)

La renderización previa parcial es una función experimental introducida en Next.js 14. El contenido de esta página puede actualizarse a medida que la función avanza en estabilidad.

Así es como se crean la mayoría de las aplicaciones web en la actualidad. Puede elegir entre renderizado estático y dinámico para toda su aplicación o para una ruta específica.

Sin embargo, la mayoría de las rutas no son completamente estáticas ni dinámicas. Es posible que tenga una ruta que tenga contenido tanto estático como dinámico.Es posible que pueda prerenderizar la mayor parte de la página del producto, pero es posible que desee recuperar el carrito del usuario y los productos recomendados dinámicamente bajo demanda.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fdashboard-static-dynamic-components.png&w=1920&q=75&dpl=dpl_EowNZBxNrTjUpooTGKRUPaqD2h6A 'Logo Title Text 1')

**¿Qué es el renderizado previo parcial?**
Next.js 14 contiene una vista previa de la renderización previa parcial, una función experimental que le permite renderizar una ruta con un shell de carga estático, manteniendo algunas partes dinámicas. En otras palabras, puedes aislar las partes dinámicas de una ruta

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fthinking-in-ppr.png&w=1920&q=75&dpl=dpl_EowNZBxNrTjUpooTGKRUPaqD2h6A 'Logo Title Text 1')

Se sirve un shell de ruta estática, lo que garantiza una carga inicial rápida.
El shell deja huecos donde el contenido dinámico se cargará de forma asincrónica.
Los agujeros asíncronos se transmiten en paralelo, lo que reduce el tiempo de carga general de la página.

La representación previa parcial combina la entrega estática ultrarrápida con capacidades totalmente dinámicas y creemos que tiene el potencial de convertirse en el modelo de representación.

**¿Cómo funciona el prerenderizado parcial?**
La renderización previa parcial aprovecha las API simultáneas de React y utiliza Suspense para diferir la renderización de partes de su aplicación hasta que se cumpla alguna condición (por ejemplo, se cargan los datos).

El respaldo está incrustado en el archivo estático inicial junto con otro contenido estático. En el momento de la construcción (o durante la revalidación), las partes estáticas de la ruta se prerenderizan y el resto se pospone hasta que el usuario solicita la ruta.

Vale la pena señalar que envolver un componente en Suspense no hace que el componente en sí sea dinámico (recuerde que usó unstable_noStore para lograr este comportamiento), sino que Suspense se usa como un límite entre las partes estáticas y dinámicas de su ruta.

Lo mejor de la renderización previa parcial es que no es necesario cambiar el código para utilizarla. Siempre que utilice Suspense para ajustar las partes dinámicas de su ruta, Next.js sabrá qué partes de su ruta son estáticas y cuáles son dinámicas.

## Agregar búsqueda y paginación

Ahora pasemos a la página /facturas y aprendamos cómo agregar búsqueda y paginación

< Search/> permite a los usuarios buscar facturas específicas.
< Pagination/> permite a los usuarios navegar entre páginas de facturas.
< Table> muestra las facturas.

Su funcionalidad de búsqueda abarcará el cliente y el servidor. Cuando un usuario busca una factura en el cliente, los parámetros de URL se actualizarán, los datos se recuperarán en el servidor y la tabla se volverá a representar en el servidor con los nuevos datos.

**¿Por qué utilizar parámetros de búsqueda de URL?**
Utilizará parámetros de búsqueda de URL para administrar el estado de búsqueda. Este patrón puede ser nuevo si está acostumbrado a hacerlo con el estado del lado del cliente.

Hay un par de beneficios al implementar la búsqueda con parámetros de URL:

- URL que se pueden marcar y compartir: dado que los parámetros de búsqueda están en la URL, los usuarios pueden marcar el estado actual de la aplicación, incluidas sus consultas y filtros de búsqueda, para consultarlos o compartirlos en el futuro.
- Representación del lado del servidor y carga inicial: los parámetros de URL se pueden consumir directamente en el servidor para representar el estado inicial, lo que facilita el manejo de la representación del servidor.
- Análisis y seguimiento: tener consultas de búsqueda y filtros directamente en la URL facilita el seguimiento del comportamiento del usuario sin requerir lógica adicional del lado del cliente.

useSearchParams: le permite acceder a los parámetros de la URL actual.

usePathname: le permite leer el nombre de ruta de la URL actual.

useRouter: permite la navegación entre rutas dentro de los componentes del cliente mediante programación.

El search: este es un componente de cliente, lo que significa que puede utilizar detectores de eventos y ganchos.

**defaultValue vs. value / Controlled vs. Uncontrolled**

Si usa el estado para administrar el valor de una entrada, usará el atributo de valor para convertirlo en un componente controlado. Esto significa que React gestionaría el estado de la entrada.

Sin embargo, como no estás usando el estado, puedes usar defaultValue. Esto significa que la entrada nativa gestionará su propio estado. Esto está bien ya que está guardando la consulta de búsqueda en la URL en lugar del estado.
​
**Mejores prácticas: antirrebote**

¡Está actualizando la URL con cada pulsación de tecla y, por lo tanto, consulta su base de datos con cada pulsación de tecla!

El antirrebote es una práctica de programación que limita la velocidad a la que se puede activar una función.

Puede implementar la función antirrebote de varias maneras, incluida la creación manual de su propia función antirrebote. Para simplificar las cosas, usaremos una biblioteca llamada use-debounce

```
npm i use-debounce
```

Esta función envolverá el contenido de handleSearch y solo ejecutará el código después de un tiempo específico una vez que el usuario haya dejado de escribir (300 ms).

Al eliminar el rebote, puede reducir la cantidad de solicitudes enviadas a su base de datos, ahorrando así recursos.

**Agregar paginación**

Después de introducir la función de búsqueda, notará que la tabla muestra solo 6 facturas a la vez. Esto se debe a que la función fetchFilteredInvoices() en data.ts devuelve un máximo de 6 facturas por página.

## Mutating Data

**¿Qué son las acciones del servidor?**
Las acciones de React Server le permiten ejecutar código asincrónico directamente en el servidor. Eliminan la necesidad de crear puntos finales API para modificar sus datos. En su lugar, escribe funciones asincrónicas que se ejecutan en el servidor y pueden invocarse desde sus componentes de cliente o servidor.

La seguridad es una prioridad absoluta para las aplicaciones web, ya que pueden ser vulnerables a diversas amenazas. Aquí es donde entran en juego Server Actions. Ofrecen una solución de seguridad eficaz, que protege contra diferentes tipos de ataques, protege sus datos y garantiza el acceso autorizado. Las acciones del servidor logran esto a través de técnicas como solicitudes POST, cierres cifrados, controles de entrada estrictos, hash de mensajes de error y restricciones de host, todos trabajando juntos para mejorar significativamente la seguridad de su aplicación.

**Usar formularios con acciones del servidor**

En React, puedes usar el atributo de acción en el elemento < form > para invocar acciones. La acción recibirá automáticamente el objeto FormData nativo, que contiene los datos capturados.

```jsx
// Server Component
export default function Page() {
  // Action
  async function create(formData: FormData) {
    'use server';

    // Logic to mutate data...
  }

  // Invoke the action using the "action" attribute
  return <form action={create}>...</form>;
}
```

Una ventaja de invocar una acción del servidor dentro de un componente del servidor es la mejora progresiva: los formularios funcionan incluso si JavaScript está deshabilitado en el cliente.

**Next.js con acciones del servidor**
Las acciones del servidor también están profundamente integradas con el almacenamiento en caché de Next.js. Cuando se envía un formulario a través de una acción del servidor, no solo puede usar la acción para mutar datos, sino que también puede revalidar el caché asociado usando API como revalidatePath y revalidateTag.

Su página es un componente de servidor que busca clientes y los pasa al componente < Form >.
(http://localhost:3000/dashboard/invoices/create)

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fcreate-invoice-page.png&w=1080&q=75&dpl=dpl_EowNZBxNrTjUpooTGKRUPaqD2h6A 'Logo Title Text 1')

Ahora creemos una acción del servidor que se llamará cuando se envíe el formulario.

Navegue a su directorio lib y cree un nuevo archivo llamado action.ts. En la parte superior de este archivo, agregue la directiva de servidor de uso de React:

```jsx
'use server';

export async function createInvoice(formData: FormData) {}
```

Al agregar 'usar servidor', marca todas las funciones exportadas dentro del archivo como funciones de servidor. Estas funciones del servidor se pueden importar luego a los componentes Cliente y Servidor, lo que los hace extremadamente versátiles.

También puede escribir Acciones del servidor directamente dentro de los Componentes del servidor agregando "usar servidor" dentro de la acción.

```jsx
import { customerField } from '@/app/lib/definitions';
import Link from 'next/link';
import {
  CheckIcon,
  ClockIcon,
  CurrencyDollarIcon,
  UserCircleIcon,
} from '@heroicons/react/24/outline';
import { Button } from '@/app/ui/button';
import { createInvoice } from '@/app/lib/actions';

export default function Form({
  customers,
}: {
  customers: customerField[];
}) {
  return (
    <form action={createInvoice}>
      // ...
  )
}
```

De vuelta en su archivo action.ts, necesitará extraer los valores de formData; hay un par de métodos que puede usar.

```jsx
'use server';

export async function createInvoice(formData: FormData) {
  const rawFormData = {
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  };
  // Test it out:
  console.log(rawFormData);
}
```

Antes de enviar los datos del formulario a su base de datos, debe asegurarse de que estén en el formato correcto y con los tipos correctos.

Next.js tiene una caché de enrutador del lado del cliente que almacena los segmentos de ruta en el navegador del usuario durante un tiempo. Junto con la captación previa, este caché garantiza que los usuarios puedan navegar rápidamente entre rutas y al mismo tiempo reducir la cantidad de solicitudes realizadas al servidor.

Dado que está actualizando los datos que se muestran en la ruta de facturas, desea borrar este caché y activar una nueva solicitud al servidor. Puedes hacer esto con la función revalidatePath de Next.js:

```jsx
'use server';

import { z } from 'zod';
import { sql } from '@vercel/postgres';
import { revalidatePath } from 'next/cache';
import { redirect } from 'next/navigation';

const FormSchema = z.object({
    id: z.string(),
    customerId: z.string(),
    amount: z.coerce.number(),
    status: z.enum(['pending', 'paid']),
    date: z.string(),
});

const CreateInvoice = FormSchema.omit({ id: true, date: true });
export async function createInvoice(formData: FormData) {
    const { customerId, amount, status } = CreateInvoice.parse({
        customerId: formData.get('customerId'),
        amount: formData.get('amount'),
        status: formData.get('status'),
    });
    const amountInCents = amount * 100;
    const date = new Date().toISOString().split('T')[0];

    await sql`
    INSERT INTO invoices (customer_id, amount, status, date)
    VALUES (${customerId}, ${amountInCents}, ${status}, ${date})
  `;
  revalidatePath('/dashboard/invoices');
  redirect('/dashboard/invoices');

}
```

**El formulario de actualización** de factura es similar al formulario de creación de una factura, excepto que deberá pasar la identificación de la factura para actualizar el registro en su base de datos.

Cree un segmento de ruta dinámico con la identificación de la factura.
Next.js le permite crear segmentos de ruta dinámicos cuando no conoce el nombre exacto del segmento y desea crear rutas basadas en datos. Podrían ser títulos de publicaciones de blog, páginas de productos, etc. Puede crear segmentos de ruta dinámicos colocando el nombre de una carpeta entre corchetes. Por ejemplo, [id], [publicación] o [slug].

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fedit-invoice-route.png&w=1920&q=75&dpl=dpl_EowNZBxNrTjUpooTGKRUPaqD2h6A 'Logo Title Text 1')

```Jsx
// dashboard/invoices/[id]/edit/page.tsx
import Form from '@/app/ui/invoices/edit-form';
import Breadcrumbs from '@/app/ui/invoices/breadcrumbs';
import { fetchInvoiceById, fetchCustomers } from '@/app/lib/data';
export default async function Page({ params }: { params: { id: string } }) {
    const id = params.id;
    const [invoice, customers] = await Promise.all([
        fetchInvoiceById(id),
        fetchCustomers(),
    ]);
    return (
        <main>
            <Breadcrumbs
                breadcrumbs={[
                    { label: 'Invoices', href: '/dashboard/invoices' },
                    {
                        label: 'Edit Invoice',
                        href: `/dashboard/invoices/${id}/edit`,
                        active: true,
                    },
                ]}
            />
            <Form invoice={invoice} customers={customers} />
        </main>
    );
}
```

```jsx
// app/ui/invoices/edit-form.tsx
// ...
import { updateInvoice } from '@/app/lib/actions';

export default function EditInvoiceForm({
  invoice,
  customers,
}: {
  invoice: InvoiceForm;
  customers: CustomerField[];
}) {
  const updateInvoiceWithId = updateInvoice.bind(null, invoice.id);

  return (
    <form action={updateInvoiceWithId}>
      <input type="hidden" name="id" value={invoice.id} />
    </form>
  );
}

```

```jsx
// app/lib/actions.ts
// Use Zod to update the expected types
const UpdateInvoice = FormSchema.omit({ id: true, date: true });

// ...

export async function updateInvoice(id: string, formData: FormData) {
  const { customerId, amount, status } = UpdateInvoice.parse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });

  const amountInCents = amount * 100;

  await sql`
    UPDATE invoices
    SET customer_id = ${customerId}, amount = ${amountInCents}, status = ${status}
    WHERE id = ${id}
  `;

  revalidatePath('/dashboard/invoices');
  redirect('/dashboard/invoices');
}
```

**Eliminar una factura**
Para eliminar una factura usando una Acción del Servidor, ajuste el botón de eliminar en un elemento < form > y pase la identificación a la Acción del Servidor usando bind:

```jsx
import { deleteInvoice } from '@/app/lib/actions';

// ...

export function DeleteInvoice({ id }: { id: string }) {
  const deleteInvoiceWithId = deleteInvoice.bind(null, id);

  return (
    <form action={deleteInvoiceWithId}>
      <button className="rounded-md border p-2 hover:bg-gray-100">
        <span className="sr-only">Delete</span>
        <TrashIcon className="w-4" />
      </button>
    </form>
  );
}
```

```jsx
export async function deleteInvoice(id: string) {
  await sql`DELETE FROM invoices WHERE id = ${id}`;
  revalidatePath('/dashboard/invoices');
}
```

Dado que esta acción se llama en la ruta /dashboard/invoices, no es necesario llamar a la redirección. Llamar a revalidatePath activará una nueva solicitud del servidor y volverá a representar la tabla.

## Handling Errors

Veamos cómo puede manejar los errores con elegancia utilizando las declaraciones try/catch de JavaScript y las API de Next.js.

Agregar try/catch a las acciones del servidor
Primero, agreguemos declaraciones try/catch de JavaScript a sus acciones del servidor para permitirle manejar los errores con elegancia.

```jsx
export async function createInvoice(formData: FormData) {
  const { customerId, amount, status } = CreateInvoice.parse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });

  const amountInCents = amount * 100;
  const date = new Date().toISOString().split('T')[0];

  try {
    await sql`
      INSERT INTO invoices (customer_id, amount, status, date)
      VALUES (${customerId}, ${amountInCents}, ${status}, ${date})
    `;
  } catch (error) {
    return {
      message: 'Database Error: Failed to Create Invoice.',
    };
  }

  revalidatePath('/dashboard/invoices');
  redirect('/dashboard/invoices');
}

```

Observe cómo se llama a la redirección fuera del bloque try/catch. Esto se debe a que la redirección funciona arrojando un error, que sería detectado por el bloque catch. Para evitar esto, puede llamar a la redirección después de try/catch. Solo se podrá acceder a la redirección si el intento tiene éxito.

**_Manejando todos los errores con error.tsx_**

El archivo **error.tsx** se puede utilizar para definir un límite de interfaz de usuario para un segmento de ruta. Sirve como un todo para errores inesperados y le permite mostrar una interfaz de usuario alternativa a sus usuarios.

Dentro de su carpeta /dashboard/invoices, cree un nuevo archivo llamado error.tsx y pegue el siguiente código:

```jsx
'use client';

import { useEffect } from 'react';

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    // Optionally log the error to an error reporting service
    console.error(error);
  }, [error]);

  return (
    <main className="flex h-full flex-col items-center justify-center">
      <h2 className="text-center">Something went wrong!</h2>
      <button
        className="mt-4 rounded-md bg-blue-500 px-4 py-2 text-sm text-white transition-colors hover:bg-blue-400"
        onClick={
          // Attempt to recover by trying to re-render the invoices route
          () => reset()
        }
      >
        Try again
      </button>
    </main>
  );
}

```

Hay algunas cosas que notará sobre el código anterior:

- "use client": error.tsx debe ser un componente de cliente.
- Acepta dos accesorios:
  - error: este objeto es una instancia del objeto Error nativo de JavaScript.
  - reset: esta es una función para restablecer el límite de error. Cuando se ejecuta, la función intentará volver a representar el segmento de ruta.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Ferror-page.png&w=1080&q=75&dpl=dpl_6QRHaXj2wrFR81WjKojPAtiJFcfG 'Logo Title Text 1')

**Manejo de errores 404 con la función notFound**

Otra forma de manejar los errores con elegancia es mediante la función notFound. Si bien error.tsx es útil para detectar todos los errores, notFound se puede utilizar cuando intentas recuperar un recurso que no existe.

Por ejemplo, visite http://localhost:3000/dashboard/invoices/2e94d1ed-d220-449f-9f11-f0bbceed9645/edit.

Este es un UUID falso que no existe en su base de datos.

Inmediatamente verá que error.tsx se activa porque esta es una ruta secundaria de /invoices donde se define error.tsx.

Sin embargo, si desea ser más específico, puede mostrar un error 404 para indicarle al usuario que no se ha encontrado el recurso al que intenta acceder.

```jsx
import { fetchInvoiceById, fetchCustomers } from '@/app/lib/data';
import { updateInvoice } from '@/app/lib/actions';
import { notFound } from 'next/navigation';

export default async function Page({ params }: { params: { id: string } }) {
  const id = params.id;
  const [invoice, customers] = await Promise.all([
    fetchInvoiceById(id),
    fetchCustomers(),
  ]);

  if (!invoice) {
    notFound();
  }

  // ...
}

```

Para mostrar una interfaz de usuario de error al usuario. Cree un archivo not-found.tsx dentro de la carpeta /edit

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fnot-found-file.png&w=1920&q=75&dpl=dpl_6QRHaXj2wrFR81WjKojPAtiJFcfG 'Logo Title Text 1')

Eso es algo a tener en cuenta: notFound tendrá prioridad sobre error.tsx, por lo que puedes utilizarlo cuando quieras manejar errores más específicos.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2F404-not-found-page.png&w=1080&q=75&dpl=dpl_6QRHaXj2wrFR81WjKojPAtiJFcfG 'Logo Title Text 1')

## Mejorando la accesibilidad

Todavía tenemos que discutir otra pieza del rompecabezas: la validación de formularios. Veamos cómo implementar la validación del lado del servidor con Server Actions y cómo mostrar errores de formulario usando el gancho useFormState, ¡teniendo en cuenta la accesibilidad!

**¿Qué es la accesibilidad?**
La accesibilidad se refiere al diseño e implementación de aplicaciones web que todos puedan usar.

Usando el complemento de accesibilidad ESLint en Next.js
De forma predeterminada, Next.js incluye el complemento eslint-plugin-jsx-a11y para ayudar a detectar problemas de accesibilidad de manera temprana. Por ejemplo, este complemento advierte si tiene imágenes sin texto alternativo, usa los atributos aria-\* y role incorrectamente, y más.

```jsx
"scripts": {
    "build": "next build",
    "dev": "next dev",
    "seed": "node -r dotenv/config ./scripts/seed.js",
    "start": "next start",
    "lint": "next lint"
},
```

Sin embargo, ¿qué pasaría si tuvieras una imagen sin texto alternativo? ¡Vamos a averiguar!

```jsx
<Image
  src={invoice.image_url}
  className="rounded-full"
  width={28}
  height={28}
  alt={`${invoice.name}'s profile picture`} // Delete this line
/>
```

```terminal
./app/ui/invoices/table.tsx
45:25  Warning: Image elements must have an alt prop,
either with meaningful text, or an empty string for decorative images. jsx-a11y/alt-text
```

**Mejorar la accesibilidad de los formularios**

Hay tres cosas que ya estamos haciendo para mejorar la accesibilidad en nuestros formularios:

- Semantic HTML: uso de elementos semánticos (< input >, < option >, etc.) en lugar de < div >. Esto permite que las tecnologías de asistencia (AT) se centren en los elementos de entrada y proporcionen información contextual adecuada al usuario, haciendo que el formulario sea más fácil de navegar y comprender.

- Labelling: incluir < label > y el atributo htmlFor garantiza que cada campo del formulario tenga una etiqueta de texto descriptivo. Esto mejora el soporte de AT al proporcionar contexto y también mejora la usabilidad al permitir a los usuarios hacer clic en la etiqueta para centrarse en el campo de entrada correspondiente.

- Focus Outline: los campos tienen el estilo adecuado para mostrar un contorno cuando están enfocados. Esto es fundamental para la accesibilidad, ya que indica visualmente el elemento activo en la página, lo que ayuda a los usuarios del teclado y del lector de pantalla a comprender dónde se encuentran en el formulario.

**Validación de formulario**

Vaya a http://localhost:3000/dashboard/invoices/create y envíe un formulario vacío. ¡Obtienes un error! Esto se debe a que estás enviando valores de formulario vacíos a tu acción del servidor. Puede evitar esto validando su formulario en el cliente o en el servidor.

**Validación del lado del cliente**

Hay un par de formas de validar formularios en el cliente. Lo más sencillo sería confiar en la validación del formulario proporcionada por el navegador agregando el atributo requerido a los elementos < input > y < select > en sus formularios. Por ejemplo:

```jsx
<input
  id="amount"
  name="amount"
  type="number"
  placeholder="Enter USD amount"
  className="peer block w-full rounded-md border border-gray-200 py-2 pl-10 text-sm outline-2 placeholder:text-gray-500"
  required
/>
```

Envíe el formulario nuevamente y ahora debería ver una advertencia en el navegador si intenta enviar un formulario con valores vacíos.

Este enfoque generalmente es correcto porque algunos AT admiten la validación del navegador.

**Validación del lado del servidor**

- Asegúrese de que sus datos estén en el formato esperado antes de enviarlos a su base de datos.

- Reduzca el riesgo de que usuarios malintencionados pasen por alto la validación del lado del cliente.

- Tener una fuente de verdad para lo que se considera información válida.

Dado que useFormState es un gancho, deberá convertir su formulario en un componente de cliente usando la directiva "use client".

Nota: "use client"se utiliza para declarar un límite entre los módulos de un servidor y un componente de cliente. Esto significa que al definir a "use client"en un archivo, todos los demás módulos importados en él, incluidos los componentes secundarios, se consideran parte del paquete del cliente.

![alt text](https://nextjs-org.translate.goog/_next/image?url=%2Fdocs%2Flight%2Fuse-client-directive.png&w=1920&q=75&dpl=dpl_AvkABcZPh9CCwMBxhYr7A6976Zz6 'Logo Title Text 1')

useFormState:

Toma dos argumentos: (action, initialState)
Devuelve dos valores: [state, dispatch] el estado del formulario y una función de envío (similar a useReducer)

```jsx
// ...
import { useFormState } from 'react-dom';

export default function Form({ customers }: { customers: CustomerField[] }) {
  const initialState = { message: null, errors: {} };
  const [state, dispatch] = useFormState(createInvoice, initialState);

  return <form action={dispatch}>...</form>;
}
```

El estado inicial puede ser cualquier cosa que definas; en este caso, crea un objeto con dos claves vacías: mensaje y errores.

En su archivo action.ts, puede usar Zod para validar los datos del formulario. Actualice su FormSchema de la siguiente manera:

```jsx
const FormSchema = z.object({
  id: z.string(),
  customerId: z.string({
    invalid_type_error: 'Please select a customer.',
  }),
  amount: z.coerce
    .number()
    .gt(0, { message: 'Please enter an amount greater than $0.' }),
  status: z.enum(['pending', 'paid'], {
    invalid_type_error: 'Please select an invoice status.',
  }),
  date: z.string(),
});
// This is temporary until @types/react-dom is updated
export type State = {
  errors?: {
    customerId?: string[];
    amount?: string[];
    status?: string[];
  };
  message?: string | null;
};

export async function createInvoice(prevState: State, formData: FormData) {
  // ...
}
```

safeParse() devolverá un objeto que contiene un campo de éxito o de error. Esto ayudará a manejar la validación de manera más elegante sin tener que poner esta lógica dentro del bloque try/catch.

```jsx
export async function createInvoice(prevState: State, formData: FormData) {
  // Validate form using Zod
  const validatedFields = CreateInvoice.safeParse({
    customerId: formData.get('customerId'),
    amount: formData.get('amount'),
    status: formData.get('status'),
  });

  // If form validation fails, return errors early. Otherwise, continue.
  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
      message: 'Missing Fields. Failed to Create Invoice.',
    };
  }

  // Prepare data for insertion into the database
  const { customerId, amount, status } = validatedFields.data;
  const amountInCents = amount * 100;
  const date = new Date().toISOString().split('T')[0];

  // Insert data into the database
  try {
    await sql`
      INSERT INTO invoices (customer_id, amount, status, date)
      VALUES (${customerId}, ${amountInCents}, ${status}, ${date})
    `;
  } catch (error) {
    // If a database error occurs, return a more specific error.
    return {
      message: 'Database Error: Failed to Create Invoice.',
    };
  }

  // Revalidate the cache for the invoices page and redirect the user.
  revalidatePath('/dashboard/invoices');
  redirect('/dashboard/invoices');
}
```

Genial, ahora mostremos los errores en su componente de formulario. De vuelta en el componente create-form.tsx, puede acceder a los errores utilizando el estado del formulario.

Agregue un operador ternario que verifique cada error específico. Por ejemplo, después del campo del cliente, puedes agregar:

```jsx
<form action={dispatch}>
  <div className="rounded-md bg-gray-50 p-4 md:p-6">
    {/* Customer Name */}
    <div className="mb-4">
      <label htmlFor="customer" className="mb-2 block text-sm font-medium">
        Choose customer
      </label>
      <div className="relative">
        <select
          id="customer"
          name="customerId"
          className="peer block w-full rounded-md border border-gray-200 py-2 pl-10 text-sm outline-2 placeholder:text-gray-500"
          defaultValue=""
          aria-describedby="customer-error"
        >
          <option value="" disabled>
            Select a customer
          </option>
          {customers.map((name) => (
            <option key={name.id} value={name.id}>
              {name.name}
            </option>
          ))}
        </select>
        <UserCircleIcon className="pointer-events-none absolute left-3 top-1/2 h-[18px] w-[18px] -translate-y-1/2 text-gray-500" />
      </div>
      <div id="customer-error" aria-live="polite" aria-atomic="true">
        {state.errors?.customerId &&
          state.errors.customerId.map((error: string) => (
            <p className="mt-2 text-sm text-red-500" key={error}>
              {error}
            </p>
          ))}
      </div>
    </div>
    // ...
  </div>
</form>

```

- aria-describedby="customer-error": Esto establece una relación entre el elemento seleccionado y el contenedor del mensaje de error. Indica que el contenedor con id="customer-error" describe el elemento seleccionado.

- id="customer-error": este atributo de identificación identifica de forma única el elemento HTML que contiene el mensaje de error para la entrada seleccionada. Esto es necesario para que aria-describedby establezca la relación.

- aria-live="polite": el lector de pantalla debe notificar cortésmente al usuario cuando se actualiza el error dentro del div. Cuando el contenido cambia (por ejemplo, cuando un usuario corrige un error), el lector de pantalla anunciará estos cambios, pero sólo cuando el usuario esté inactivo para no interrumpirlos.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fform-validation-page.png&w=1080&q=75&dpl=dpl_AvkABcZPh9CCwMBxhYr7A6976Zz6 'Logo Title Text 1')

## Agregar autenticación

**¿Qué es la autenticación?**
La autenticación es una parte clave de muchas aplicaciones web actuales. Así es como un sistema verifica si el usuario es quien dice ser.

Un sitio web seguro suele utilizar varias formas de comprobar la identidad de un usuario. Por ejemplo, después de ingresar su nombre de usuario y contraseña, el sitio puede enviar un código de verificación a su dispositivo o usar una aplicación externa como Google Authenticator. Esta autenticación de 2 factores (2FA) ayuda a aumentar la seguridad. Incluso si alguien descubre su contraseña, no podrá acceder a su cuenta sin su token único.

**Authentication vs. Authorization**

En el desarrollo web, la autenticación y la autorización cumplen diferentes funciones:

- La autenticación consiste en asegurarse de que el usuario sea quien dice ser. Estás demostrando tu identidad con algo que tienes como un nombre de usuario y una contraseña.

- La autorización es el siguiente paso. Una vez que se confirma la identidad de un usuario, la autorización decide qué partes de la aplicación puede utilizar.

Entonces, la autenticación verifica quién es usted y la autorización determina qué puede hacer o acceder en la aplicación.

```jsx
import AcmeLogo from '@/app/ui/acme-logo';
import LoginForm from '@/app/ui/login-form';

export default function LoginPage() {
  return (
    <main className="flex items-center justify-center md:h-screen">
      <div className="relative mx-auto flex w-full max-w-[400px] flex-col space-y-2.5 p-4 md:-mt-32">
        <div className="flex h-20 w-full items-end rounded-lg bg-blue-500 p-3 md:h-36">
          <div className="w-32 text-white md:w-36">
            <AcmeLogo />
          </div>
        </div>
        <LoginForm />
      </div>
    </main>
  );
}
```

**NextAuth.js**

Usaremos **NextAuth.js** para agregar autenticación a su aplicación. NextAuth.js abstrae gran parte de la complejidad involucrada en la administración de sesiones, el inicio y cierre de sesión y otros aspectos de la autenticación. Si bien puede implementar estas funciones manualmente.

```Terminal
npm install next-auth@beta
```

Aquí, está instalando la versión beta de NextAuth.js, que es compatible con Next.js 14.

A continuación, genere una clave secreta para su aplicación. Esta clave se utiliza para cifrar las cookies, garantizando la seguridad de las sesiones de los usuarios. Puedes hacer esto ejecutando el siguiente comando en tu terminal:

```Terminal
openssl rand -base64 32
```

Luego, en su archivo .env, agregue su clave generada a la variable AUTH_SECRET:

```.env
AUTH_SECRET=your-secret-key
```

Para que la autenticación funcione en producción, también deberá actualizar las variables de entorno en su proyecto Vercel

Cree un archivo auth.config.ts en la raíz de nuestro proyecto que exporte un objeto authConfig. Este objeto contendrá las opciones de configuración para NextAuth.js.

```jsx
import type { NextAuthConfig } from 'next-auth';

export const authConfig = {
  pages: {
    signIn: '/login',
  },
};
```

Puede utilizar la opción de páginas para especificar la ruta para las páginas personalizadas de inicio de sesión, cierre de sesión y error. Esto no es obligatorio, pero al agregar la opción iniciar sesión: '/login' en nuestras páginas, el usuario será redirigido a nuestra página de inicio de sesión personalizada, en lugar de a la página predeterminada de NextAuth.js.

**Protegiendo sus rutas con Next.js Middleware**

agregue la lógica para proteger sus rutas. Esto evitará que los usuarios accedan a las páginas del panel a menos que hayan iniciado sesión.

```jsx
import type { NextAuthConfig } from 'next-auth';

export const authConfig = {
  pages: {
    signIn: '/login',
  },
  callbacks: {
    authorized({ auth, request: { nextUrl } }) {
      const isLoggedIn = !!auth?.user;
      const isOnDashboard = nextUrl.pathname.startsWith('/dashboard');
      if (isOnDashboard) {
        if (isLoggedIn) return true;
        return false; // Redirect unauthenticated users to login page
      } else if (isLoggedIn) {
        return Response.redirect(new URL('/dashboard', nextUrl));
      }
      return true;
    },
  },
  providers: [], // Add providers with an empty array for now
} satisfies NextAuthConfig;
```

The authorized callback se utiliza para verificar si la solicitud está autorizada para acceder a una página a través del Middleware Next.js.

Se llama antes de que se complete una solicitud y recibe un objeto con las propiedades de auth y y solicitud.La propiedad de autenticación contiene la sesión del usuario y la propiedad de solicitud contiene la solicitud entrante.

La opción de proveedores es una matriz donde enumera diferentes opciones de inicio de sesión. Por ahora, es una matriz vacía para satisfacer la configuración de NextAuth.

A continuación, deberá importar el objeto authConfig a un archivo de middleware. En la raíz de su proyecto, cree un archivo llamado middleware.ts

```ts
import NextAuth from 'next-auth';
import { authConfig } from './auth.config';
 
export default NextAuth(authConfig).auth;
 
export const config = {
  // https://nextjs.org/docs/app/building-your-application/routing/middleware#matcher
  matcher: ['/((?!api|_next/static|_next/image|.*\\.png$).*)'],
};
```

Aquí está inicializando NextAuth.js con el objeto authConfig y exportando la propiedad de autenticación. También está utilizando la opción de comparación de Middleware para especificar que debe ejecutarse en rutas específicas.

La ventaja de emplear Middleware para esta tarea es que las rutas protegidas ni siquiera comenzarán a procesarse hasta que el Middleware verifique la autenticación, lo que mejora tanto la seguridad como el rendimiento de su aplicación.

**Password hashing**

Es una buena práctica aplicar hash a las contraseñas antes de almacenarlas en una base de datos. El hashing convierte una contraseña en una cadena de caracteres de longitud fija, que parece aleatoria, lo que proporciona una capa de seguridad incluso si los datos del usuario están expuestos.

En su archivo seed.js, utilizó un paquete llamado bcrypt para codificar la contraseña del usuario antes de almacenarla en la base de datos.

Sin embargo, necesitarás crear un archivo separado para el paquete bcrypt. Esto se debe a que bcrypt se basa en las API de Node.js que no están disponibles en Next.js Middleware.

```ts
import NextAuth from 'next-auth';
import { authConfig } from './auth.config';
 
export const { auth, signIn, signOut } = NextAuth({
  ...authConfig,
});
```

**Agregar el proveedor de credenciales**

Deberá agregar la opción de proveedores para NextAuth.js. proveedores es una matriz donde enumera diferentes opciones de inicio de sesión, como Google o GitHub. Para este curso, nos centraremos en utilizar únicamente el proveedor de Credenciales.

El proveedor de Credenciales permite a los usuarios iniciar sesión con un nombre de usuario y una contraseña.

```ts
import NextAuth from 'next-auth';
import { authConfig } from './auth.config';
import Credentials from 'next-auth/providers/credentials';
 
export const { auth, signIn, signOut } = NextAuth({
  ...authConfig,
  providers: [Credentials({})],
});
```

**Agregar la funcionalidad de inicio de sesión**
Puede utilizar la función de autorización para manejar la lógica de autenticación. De manera similar a las Acciones del servidor, puedes usar zod para validar el correo electrónico y la contraseña antes de verificar si el usuario existe en la base de datos:

```ts
import NextAuth from 'next-auth';
import { authConfig } from './auth.config';
import Credentials from 'next-auth/providers/credentials';
import { z } from 'zod';
 
export const { auth, signIn, signOut } = NextAuth({
  ...authConfig,
  providers: [
    Credentials({
      async authorize(credentials) {
        const parsedCredentials = z
          .object({ email: z.string().email(), password: z.string().min(6) })
          .safeParse(credentials);
      },
    }),
  ],
});
```

Después de validar las credenciales, cree una nueva función getUser que consulte al usuario desde la base de datos.

```ts
import NextAuth from 'next-auth';
import Credentials from 'next-auth/providers/credentials';
import { authConfig } from './auth.config';
import { z } from 'zod';
import { sql } from '@vercel/postgres';
import type { User } from '@/app/lib/definitions';
import bcrypt from 'bcrypt';
 
async function getUser(email: string): Promise<User | undefined> {
  try {
    const user = await sql<User>`SELECT * FROM users WHERE email=${email}`;
    return user.rows[0];
  } catch (error) {
    console.error('Failed to fetch user:', error);
    throw new Error('Failed to fetch user.');
  }
}
 
export const { auth, signIn, signOut } = NextAuth({
  ...authConfig,
  providers: [
    Credentials({
      async authorize(credentials) {
        const parsedCredentials = z
          .object({ email: z.string().email(), password: z.string().min(6) })
          .safeParse(credentials);
 
        if (parsedCredentials.success) {
          const { email, password } = parsedCredentials.data;
          const user = await getUser(email);
          if (!user) return null;
        }
 
        return null;
      },
    }),
  ],
});
```

Luego, llame a bcrypt.compare para verificar si las contraseñas coinciden:

```ts
import NextAuth from 'next-auth';
import Credentials from 'next-auth/providers/credentials';
import { authConfig } from './auth.config';
import { sql } from '@vercel/postgres';
import { z } from 'zod';
import type { User } from '@/app/lib/definitions';
import bcrypt from 'bcrypt';
 
// ...
 
export const { auth, signIn, signOut } = NextAuth({
  ...authConfig,
  providers: [
    Credentials({
      async authorize(credentials) {
        // ...
 
        if (parsedCredentials.success) {
          const { email, password } = parsedCredentials.data;
          const user = await getUser(email);
          if (!user) return null;
          const passwordsMatch = await bcrypt.compare(password, user.password);
 
          if (passwordsMatch) return user;
        }
 
        console.log('Invalid credentials');
        return null;
      },
    }),
  ],
});
```

**Actualizando el formulario de inicio de sesión**
Ahora necesitas conectar la lógica de autenticación con tu formulario de inicio de sesión. En su archivo action.ts, cree una nueva acción llamada autenticar. Esta acción debería importar la función de inicio de sesión desde auth.ts:

```ts
import { signIn } from '@/auth';
import { AuthError } from 'next-auth';
 
// ...
 
export async function authenticate(
  prevState: string | undefined,
  formData: FormData,
) {
  try {
    await signIn('credentials', formData);
  } catch (error) {
    if (error instanceof AuthError) {
      switch (error.type) {
        case 'CredentialsSignin':
          return 'Invalid credentials.';
        default:
          return 'Something went wrong.';
      }
    }
    throw error;
  }
}
```

Si hay un error 'CredentialsSignin', desea mostrar un mensaje de error apropiado. Puede obtener información sobre los errores de NextAuth.js en la documentación.

Finalmente, en su componente login-form.tsx, puede usar useFormState de React para llamar a la acción del servidor y manejar errores del formulario, y usar useFormStatus para manejar el estado pendiente del formulario:

```jsx
'use client';
 
import { lusitana } from '@/app/ui/fonts';
import {
  AtSymbolIcon,
  KeyIcon,
  ExclamationCircleIcon,
} from '@heroicons/react/24/outline';
import { ArrowRightIcon } from '@heroicons/react/20/solid';
import { Button } from '@/app/ui/button';
import { useFormState, useFormStatus } from 'react-dom';
import { authenticate } from '@/app/lib/actions';
 
export default function LoginForm() {
  const [errorMessage, dispatch] = useFormState(authenticate, undefined);
 
  return (
    <form action={dispatch} className="space-y-3">
      <div className="flex-1 rounded-lg bg-gray-50 px-6 pb-4 pt-8">
        <h1 className={`${lusitana.className} mb-3 text-2xl`}>
          Please log in to continue.
        </h1>
        <div className="w-full">
          <div>
            <label
              className="mb-3 mt-5 block text-xs font-medium text-gray-900"
              htmlFor="email"
            >
              Email
            </label>
            <div className="relative">
              <input
                className="peer block w-full rounded-md border border-gray-200 py-[9px] pl-10 text-sm outline-2 placeholder:text-gray-500"
                id="email"
                type="email"
                name="email"
                placeholder="Enter your email address"
                required
              />
              <AtSymbolIcon className="pointer-events-none absolute left-3 top-1/2 h-[18px] w-[18px] -translate-y-1/2 text-gray-500 peer-focus:text-gray-900" />
            </div>
          </div>
          <div className="mt-4">
            <label
              className="mb-3 mt-5 block text-xs font-medium text-gray-900"
              htmlFor="password"
            >
              Password
            </label>
            <div className="relative">
              <input
                className="peer block w-full rounded-md border border-gray-200 py-[9px] pl-10 text-sm outline-2 placeholder:text-gray-500"
                id="password"
                type="password"
                name="password"
                placeholder="Enter password"
                required
                minLength={6}
              />
              <KeyIcon className="pointer-events-none absolute left-3 top-1/2 h-[18px] w-[18px] -translate-y-1/2 text-gray-500 peer-focus:text-gray-900" />
            </div>
          </div>
        </div>
        <LoginButton />
        <div
          className="flex h-8 items-end space-x-1"
          aria-live="polite"
          aria-atomic="true"
        >
          {errorMessage && (
            <>
              <ExclamationCircleIcon className="h-5 w-5 text-red-500" />
              <p className="text-sm text-red-500">{errorMessage}</p>
            </>
          )}
        </div>
      </div>
    </form>
  );
}
 
function LoginButton() {
  const { pending } = useFormStatus();
 
  return (
    <Button className="mt-4 w-full" aria-disabled={pending}>
      Log in <ArrowRightIcon className="ml-auto h-5 w-5 text-gray-50" />
    </Button>
  );
}
```

**Agregar la funcionalidad de cerrar sesión**
Para agregar la funcionalidad de cierre de sesión a < SideNav />, llame a la función signOut desde auth.ts en su elemento < form >:

```jsx
import Link from 'next/link';
import NavLinks from '@/app/ui/dashboard/nav-links';
import AcmeLogo from '@/app/ui/acme-logo';
import { PowerIcon } from '@heroicons/react/24/outline';
import { signOut } from '@/auth';
 
export default function SideNav() {
  return (
    <div className="flex h-full flex-col px-3 py-4 md:px-2">
      // ...
      <div className="flex grow flex-row justify-between space-x-2 md:flex-col md:space-x-0 md:space-y-2">
        <NavLinks />
        <div className="hidden h-auto w-full grow rounded-md bg-gray-50 md:block"></div>
        <form
          action={async () => {
            'use server';
            await signOut();
          }}
        >
          <button className="flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3">
            <PowerIcon className="w-6" />
            <div className="hidden md:block">Sign Out</div>
          </button>
        </form>
      </div>
    </div>
  );
}
```

Email: user@nextmail.com
Password: 123456

## Adding Metadata ##
Los metadatos son cruciales para el SEO y la capacidad de compartir. En este capítulo, analizaremos cómo puede agregar metadatos a su aplicación Next.js.

**¿Qué son los metadatos?**
En el desarrollo web, los metadatos proporcionan detalles adicionales sobre una página web. Los metadatos no son visibles para los usuarios que visitan la página. En cambio, funciona detrás de escena, incrustado en el HTML de la página, generalmente dentro del elemento < head >. Esta información oculta es crucial para los motores de búsqueda y otros sistemas que necesitan comprender mejor el contenido de su página web.

**¿Por qué son importantes los metadatos?**
Los metadatos desempeñan un papel importante en la mejora del SEO de una página web, haciéndola más accesible y comprensible para los motores de búsqueda y las plataformas de redes sociales. Los metadatos adecuados ayudan a los motores de búsqueda a indexar páginas web de forma eficaz, mejorando su clasificación en los resultados de búsqueda. Además, los metadatos como Open Graph mejoran la apariencia de los enlaces compartidos en las redes sociales, haciendo que el contenido sea más atractivo e informativo para los usuarios.

**Tipos de metadatos**
Hay varios tipos de metadatos y cada uno tiene un propósito único. Algunos tipos comunes incluyen:

**Metadatos del título**: Responsable del título de una página web que se muestra en la pestaña del navegador. Es crucial para el SEO, ya que ayuda a los motores de búsqueda a comprender de qué trata la página web.

```html
<title>Page Title</title>
```

**Metadatos de descripción**: estos metadatos proporcionan una breve descripción general del contenido de la página web y, a menudo, se muestran en los resultados de los motores de búsqueda.

```html
<meta name="description" content="A brief description of the page content." />
```

**Metadatos de palabras clave**: estos metadatos incluyen las palabras clave relacionadas con el contenido de la página web, lo que ayuda a los motores de búsqueda a indexar la página.

```html
<meta name="keywords" content="keyword1, keyword2, keyword3" />
```

**Metadatos de Open Graph**: estos metadatos mejoran la forma en que se representa una página web cuando se comparte en plataformas de redes sociales, proporcionando información como el título, la descripción y la imagen de vista previa.

```html
<meta property="og:title" content="Title Here" />
<meta property="og:description" content="Description Here" />
<meta property="og:image" content="image_url_here" />
```

**Metadatos de Favicon**: estos metadatos vinculan el favicon (un pequeño icono) a la página web, que se muestra en la barra o pestaña de direcciones del navegador.

```html
<link rel="icon" href="path/to/favicon.ico" />
```

**Agregar metadatos**
Next.js tiene una API de metadatos que se puede utilizar para definir los metadatos de su aplicación. Hay dos formas de agregar metadatos a su aplicación:

* Config-based: exporte un objeto de metadatos estático o una función dinámica generateMetadata en un archivo layout.js o page.js.
​
* File-based: Next.js tiene una variedad de archivos especiales que se utilizan específicamente con fines de metadatos:
  * favicon.ico, apple-icon.jpg e icon.jpg: se utilizan para favicons e íconos
  * opengraph-image.jpg y twitter-image.jpg: empleados para imágenes de redes sociales
  * robots.txt: proporciona instrucciones para el rastreo de motores de búsqueda.
  * sitemap.xml: Ofrece información sobre la estructura del sitio web.

Tiene la flexibilidad de utilizar estos archivos para metadatos estáticos o puede generarlos mediante programación dentro de su proyecto.

Con ambas opciones, Next.js generará automáticamente los elementos < head > relevantes para sus páginas.

**Imagen de Favicon y Open Graph**

En su carpeta /public, notará que tiene dos imágenes: favicon.ico y opengraph-image.jpg.

Mueva estas imágenes a la raíz de su carpeta /app.

Después de hacer esto, Next.js identificará y utilizará automáticamente estos archivos como su favicon y su imagen OG. Puede verificar esto verificando el elemento < head > de su aplicación en las herramientas de desarrollo.

**Título de la página y descripciones.**
También puede incluir un objeto de metadatos de cualquier archivo layout.js o page.js para agregar información adicional de la página, como título y descripción. Cualquier metadato en layout.js será heredado por todas las páginas que lo utilicen.

En su diseño raíz, cree un nuevo objeto de metadatos con los siguientes campos:

```ts
//app/layout.tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: 'Acme Dashboard',
  description: 'The official Next.js Course Dashboard, built with App Router.',
  metadataBase: new URL('https://next-learn-dashboard.vercel.sh'),
};
 
export default function RootLayout() {
  // ...
}
```

Next.js agregará automáticamente el título y los metadatos a su aplicación.

Pero ¿qué pasa si deseas agregar un título personalizado para una página específica? Puede hacer esto agregando un objeto de metadatos a la propia página. Los metadatos de las páginas anidadas anularán los metadatos de la página principal.

Por ejemplo, en la página /dashboard/invoices, puede actualizar el título de la página:

```ts
// app/dashboard/invoices/page.tsx
export const metadata: Metadata = {
  title: 'Invoices | Acme Dashboard',
};
```

Esto funciona, pero repetimos el título de la aplicación en cada página. Si algo cambia, como el nombre de la empresa, deberá actualizarlo en cada página.

En su lugar, puede utilizar el campo title.template en el objeto de metadatos para definir una plantilla para los títulos de sus páginas. Esta plantilla puede incluir el título de la página y cualquier otra información que desee incluir.

En su diseño raíz, actualice el objeto de metadatos para incluir una plantilla:

```ts
// app/layout.tsx
import { Metadata } from 'next';
 
export const metadata: Metadata = {
  title: {
    template: '%s | Acme Dashboard',
    default: 'Acme Dashboard',
  },
  description: 'The official Next.js Learn Dashboard built with App Router.',
  metadataBase: new URL('https://next-learn-dashboard.vercel.sh'),
};
```

El %s en la plantilla se reemplazará con el título de la página específica.

Ahora, en su página /dashboard/invoices, puede agregar el título de la pagina.

```ts
export const metadata: Metadata = {
  title: 'Invoices',
};
```

https://nextjs-dashboard-zeta-ochre-50.vercel.app/

## Otras fuentes ##
Turbopack se puede utilizar en Next.js tanto en las páginas como en los directorios de aplicaciones para un desarrollo local más rápido. Para habilitar Turbopack, use el indicador --turbo cuando ejecute el servidor de desarrollo Next.js.

Los **componentes del servidor** pueden utilizar la directiva "usar servidor" a nivel de función en línea o a nivel de módulo. Para insertar una acción del servidor, agregue "use server" en la parte superior del cuerpo de la función:

```js
// Server Component
export default function Page() {
  // Server Action
  async function create() {
    'use server'
 
    // ...
  }
 
  return (
    // ...
  )
}
```

Componentes del cliente, Los componentes del cliente sólo pueden importar acciones que utilicen la directiva "use server" a nivel de módulo.
Todas las funciones dentro del archivo se marcarán como Acciones del servidor que se pueden reutilizar tanto en los componentes del cliente como en los del servidor:

```js
'use client'
 
export default function ClientComponent({ updateItem }) {
  return <form action={updateItem}>{/* ... */}</form>
}
```