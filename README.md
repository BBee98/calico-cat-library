# Creando una librería con React

## Instalar el paquete de ``path``:

> 👉 npm i path
> 👉 npm i --save-dev @types/path


## Configuración de vite.config.js

- Para que vite funcione como una **librería** necesitamos modificar
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

- En esta parte que hemos añadido:

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
