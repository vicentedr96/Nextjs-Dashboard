## Next 13

* Nuevo sistema de enrutado ( aun esta disponible el anterior para la migracion)

* Por defecto cualquier componente que este en app, sera cargando desde el servidor, pero en caso de no querer esto podemos usar:
'use client'

* Streaming: Permite el renderizado dinamico

* TurboPack: empaquetamiento

* Fuentes: nuevo sistema de carga de fuentes, se cargaran desde el servidor (npm install @next/font)

* Generar social cards


### Estructura de carpetas
Notarás que el proyecto tiene la siguiente estructura de carpetas:

* /app: contiene todas las rutas, componentes y lógica de su aplicación; aquí es desde donde trabajará principalmente.

* /app/lib: contiene funciones utilizadas en su aplicación, como funciones de utilidad reutilizables y funciones de recuperación de datos.

* /app/ui: contiene todos los componentes de la interfaz de usuario de su aplicación, como tarjetas, tablas y formularios. Para ahorrar tiempo, hemos prediseñado estos componentes para usted.

* /public: contiene todos los activos estáticos de su aplicación, como imágenes.

* /scripts: contiene un script de inicialización que utilizará para completar su base de datos en un capítulo posterior.
Archivos de configuración: también notará archivos de configuración como next.config.js en la raíz de su aplicación. La mayoría de estos archivos se crean y preconfiguran cuando inicias un nuevo proyecto usando create-next-app. No necesitarás modificarlos en este curso.

### Estilo CSS
#### Global styles

La carpeta /app/ui, verás un archivo llamado global.css. Puede utilizar este archivo para agregar reglas CSS a todas las rutas de su aplicación, como reglas de restablecimiento de CSS, estilos de todo el sitio para elementos HTML como enlaces y más.

Puede importar global.css en cualquier componente de su aplicación, pero generalmente es una buena práctica agregarlo a su componente de nivel superior. (/app/layout.tsx)

#### CSS Modules

Los módulos CSS le permiten aplicar CSS a un componente creando automáticamente nombres de clase únicos, por lo que no tiene que preocuparse también por las colisiones de estilos

import styles from '@/app/ui/home.module.css';
```jsx
<div className={styles.shape} />;
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

***Agregar una fuente principal***

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

***Agregar una fuente secundaria***

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

### ¿Por qué optimizar imágenes? ###

Next.js puede ofrecer recursos estáticos, como imágenes, en la carpeta /public de nivel superior.

```jsx
<img
  src="/hero.png"
  alt="Screenshots of the dashboard project showing desktop version"
/>
```
Sin embargo, esto significa que tienes que hacerlo manualmente:

* Asegúrese de que su imagen responda en diferentes tamaños de pantalla.
* Especifique tamaños de imagen para diferentes dispositivos.
* Evite cambios de diseño a medida que se cargan las imágenes.
* Carga diferida de imágenes que están fuera de la ventana gráfica del usuario.

Puede utilizar el componente next/imagen para optimizar automáticamente sus imágenes.

### The Image component ###

El componente <Imagen> es una extensión de la etiqueta HTML <img> y viene con optimización automática de la imagen, como por ejemplo:

* Evitar el cambio de diseño automáticamente cuando se cargan las imágenes.
* Cambiar el tamaño de las imágenes para evitar enviar imágenes grandes a dispositivos con una ventana gráfica más pequeña.
* Carga diferida de imágenes de forma predeterminada (las imágenes se cargan a medida que ingresan a la ventana gráfica).
* Ofrecer imágenes en formatos modernos, como WebP y AVIF, cuando el navegador lo admite.

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


### Crear diseños y páginas ###
#### enrutamiento anidado ####

Next.js utiliza enrutamiento del sistema de archivos donde se usan carpetas para crear rutas anidadas. Cada carpeta representa un segmento de ruta que se asigna a un segmento de URL.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Ffolders-to-url-segments.png&w=1920&q=75&dpl=dpl_7YxV6fFeY6qNyh4dohxRZdsaFAP7 "Logo Title Text 1")

Puede crear interfaces de usuario independientes para cada ruta utilizando los archivos layout.tsx y page.tsx.

page.tsx es un archivo Next.js especial que exporta un componente de React y es necesario para que la ruta sea accesible. En su aplicación, ya tiene un archivo de página: /app/page.tsx: esta es la página de inicio asociada con la ruta /.

Para crear una ruta anidada, puede anidar carpetas una dentro de otra y agregar archivos page.tsx dentro de ellas.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fdashboard-route.png&w=1920&q=75&dpl=dpl_7YxV6fFeY6qNyh4dohxRZdsaFAP7 "Logo Title Text 1")

/app/dashboard/page.tsx está asociado con la ruta /dashboard.

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Frouting-solution.png&w=1920&q=75&dpl=dpl_7YxV6fFeY6qNyh4dohxRZdsaFAP7 "Logo Title Text 1")

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

* Un beneficio de usar diseños en Next.js es que durante la navegación, solo se actualizan los componentes de la página, mientras que el diseño no se vuelve a representar. Esto se llama renderizado parcial:

![alt text](https://nextjs.org/_next/image?url=%2Flearn%2Flight%2Fpartial-rendering-dashboard.png&w=1920&q=75&dpl=dpl_7uN5hm5ManxexfA2rBqDFeGho3if "Logo Title Text 1")

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


***Pattern: Showing active links***

Un patrón de interfaz de usuario común es mostrar un enlace activo para indicar al usuario en qué página se encuentra actualmente. Para hacer esto, necesita obtener la ruta actual del usuario desde la URL. Next.js proporciona un gancho llamado usePathname() que puede usar para verificar la ruta e implementar este patrón.

Dado que usePathname() es un enlace, deberá convertir nav-links.tsx en un componente de cliente. Agregue la directiva **"use client"**.


## Configurando su base de datos ##

