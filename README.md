# Prerequisites
- [Visual Studio Code](https://code.visualstudio.com/)
- [NodeJS](https://nodejs.org/en)
- (Optional) TypeScript VSCode Plugin (may require manual installation on OSS versions on Linux)
# Code Setup
1. Open Visual Studio Code and open the folder in which you will initialize the project (CTRL + K, CTRL + O)
2. Open the terminal (CTRL + SHIFT + `)
3. Run `npm init` and follow the next steps in the terminal. Most fields, if you don't know what they do, can be left blank
4. Run `npm i -g typescript` to install the TypeScript compiler (optional if you want to compile your bot to JavaScript before deploying it)
5. Run `npm i discord.js` and `npm i -D @types/node ts-node typescript` to install the package we're using as well as the TypeScript types that allow the language to be used
6. Create a file in the root of your project named `tsconfig.json` and paste in the following template:
```
{
    "compilerOptions": {
        "target": "ESNext",
        "module": "commonjs",
        "rootDir": "./src/",
        "outDir": "./dist/",
        "strict": true,
        "moduleResolution": "node",
        "importHelpers": true,
        "experimentalDecorators": true,
        "esModuleInterop": true,
        "skipLibCheck": true,
        "allowSyntheticDefaultImports": true,
        "resolveJsonModule": true,
        "forceConsistentCasingInFileNames": true,
        "removeComments": true,
        "typeRoots": [
            "node_modules/@types"
        ],
        "sourceMap": false,
        "baseUrl": "./"
    },
    "files": [
        "src/Index.ts"
    ],
    "include": [
        "./**/*.ts"
    ],
    "exclude": [
        "node_modules",
        "dist"
    ],
}
```
7. In `package.json`, which should've been created on step 3, add the following inside the "scripts" dictionary:
```
"start": "ts-node src/Index.ts"
"build": "tsc"
```
9. Create a folder named `src` and a file inside named `Index.ts`
# Bot Setup
