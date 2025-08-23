# Creando una librería con React

## Instalar el paquete de ``path``:

> 👉 npm i path
> 👉 npm i --save-dev @types/path


## 📝 Configuración de vite.config.js

> 🌏 https://dev.to/receter/how-to-create-a-react-component-library-using-vites-library-mode-4lma

1. Para que vite funcione como una **librería** necesitamos modificar
ligeramente la configuración de vite.

```ts
import { defineConfig } from 'vite'
import { resolve } from 'path'
import react from '@vitejs/plugin-react'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],
  build: {
      lib: {
          entry: resolve(__dirname, 'lib/main.ts'),
          formats: ['es']
      }
  }
})
```

En esta parte que hemos añadido:

```ts
{
build: {
      lib: {
          entry: resolve(__dirname, 'lib/main.ts'),
          formats: ['es']
      }
  }
}
```

Especificamos ``formats`` y le asignamos el valor ``["es"]``. 
En esta situación podemos asignar dos valores: `es` y `umd`. 

📗 El valor``es`` significa que utiliza el formato de módulos de **ECMAScript** (el más moderno de JS). 
📘 Por otro lado, el valor ``umd`` significa **Universal Module Definition**, que es más antiguo pero más universal.

Para una librería basada en javascript moderno, es recomendable utilizar ``es`` (**ECMAScript**), pero en el caso de que busques
la máxima compatibilidad posible entre los navegadores, es recomendable **añadir también** `umd`. # calico-cat-library

2. A continuación, creamos la carpeta ``lib`` dentro de la carpeta ``src``, donde desarrollaremos los componentes de nuestra librería.
Meteremos dentro, también, el fichero ``main.ts``.

````
src/
└── lib/
    └── main.ts
````

3. Ahora debemos incluir la carpeta ``lib`` que hems creado dentro del ``tsconfig.app.json`` para poder hacer la build de la librería:

````json
{
  "compilerOptions": {
    "tsBuildInfoFile": "./node_modules/.tmp/tsconfig.app.tsbuildinfo",
    "target": "ES2022",
    "useDefineForClassFields": true,
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "verbatimModuleSyntax": true,
    "moduleDetection": "force",
    "noEmit": true,
    "jsx": "react-jsx",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "erasableSyntaxOnly": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedSideEffectImports": true
  },
  "include": ["src", "lib"]
}
````

Y sustituimos el script actual de ``build`` del ``package.json``:

```json
{
"scripts": {
    "build": "tsc -b && vite build"
  }
}
```

por:

```json
{
  "scripts": {
    "build": "tsc --p ./tsconfig-build.json && vite build"
  }
}
```

4. El fichero ``vite-env.d.ts`` que tenemos **dentro** del directorio ``src`` debemos **copiarlo**
dentro de la carpeta ``lib``. De esta manera expondremos los tipos de nuestra librería y podrán ser utilizados por el usuario que la importe. 

5. Añadimos a la configuración de vite la siguiente línea:


```ts
import { defineConfig } from 'vite'
import { resolve } from 'path'
import react from '@vitejs/plugin-react'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],
  build: {
      ...,
      copyPublicDir: false,
  }
})
```

Lo cual evitará que lo que tengamos en la carpeta ``public`` se añada a la build del proyecto.

6. Para poder añadir los tipos a la build, podemos instalar un plugin de vite que nos permita añadirlos a la build.

> 👉 https://github.com/qmhc/unplugin-dts

Escribiendo la instrucción ```npm i vite-plugin-dts -D``` instalaremos el paquete que nos permite añadir un **plugin**: 
```dts({ include: ['lib'] })```.

Por tanto la configuración de vite quedaría así:

````ts
import { defineConfig } from 'vite'
import { resolve } from 'path'
import react from '@vitejs/plugin-react'
import dts from 'vite-plugin-dts'

// https://vite.dev/config/
export default defineConfig({
  plugins: [
      react(),
      dts({ include: ['lib'] })
  ],
  build: {
      lib: {
          entry: resolve(__dirname, 'lib/main.ts'),
          formats: ['es', 'umd'],
      },
      copyPublicDir: false
  }
})
````
