---

title: "Vue3 全新的開始"

date: 2023-07-03

description: "Vue3 全新的開始"

---



設定mock



vite.config.ts



    

    

    import { viteMockServe } from 'vite-plugin-mock'

    

    export default ({ mode }) =>

      defineConfig({

        plugins: [

          vue(),

          nightwatchPlugin(),

          viteMockServe({

            logger: false,

            mockPath: './src/mock',

            localEnabled: true,

            prodEnabled: false

          })

        ],

        resolve: {

          alias: {

            '@': fileURLToPath(new URL('./src', import.meta.url))

          }

        },

        define: {

          'process.env': loadEnv(mode, process.cwd())

        }

      })

    

    

    import init from './init.json'

    import material from './material.json'

    

    export default [

      {

        type: 'post',

        url: '/chat/initializeChat',

        response: () => {

          return init

        }

      },

      {

        type: 'post',

        url: '/chat/material',

        response: () => {

          return material

        }

      }

    ]

    



設定排版



.prettierrc.json



    

    

    {

      "$schema": "https://json.schemastore.org/prettierrc",

      "semi": true,

      "tabWidth": 2,

      "singleQuote": false,

      "printWidth": 180,

      "trailingComma": "none",

      "htmlWhitespaceSensitivity": "ignore"

    }



ElementPlus



    

    

    import ElementPlus from 'element-plus'

    import 'element-plus/dist/index.css'

    import * as ElementPlusIconsVue from '@element-plus/icons-vue'

    

    app.use(ElementPlus)

    for (const [key, component] of Object.entries(ElementPlusIconsVue)) {

      app.component(key, component)

    }



env.d.ts



    

    

    /// <reference types="vite/client" />

    interface ImportMetaEnv {

      readonly VITE_BASE_API_URL: string

    }

    

    interface ImportMeta {

      readonly env: ImportMetaEnv

    }

    

    declare module '*.vue' {

      import type { DefineComponent } from 'vue'

      // eslint-disable-next-line @typescript-eslint/no-explicit-any, @typescript-eslint/ban-types

      const component: DefineComponent<{}, {}, any>

      export default component

    }



