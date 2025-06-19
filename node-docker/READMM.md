# COMECANDO O PROJETO

## INSTALACAO DAS DEPENDECIAS **typescript e @types/node** NO AMBIENTE DE DESENVOLVIMENTO:
`npm i typescript @types/node -D`

### INICIALIZANDO O PROJETO TYPESCRIPT - O comando abaixo inicializara o projeto em Typescript e cria o arquivo **tsconfig.json**:
`npx tsc --init`

Apos executar o comando acima, acesse o link **https://github.com/tsconfig/bases** e desca a pagina ate encontrar a sua versao do Node (no nosso caso: Node 18) e clique sobre o @tsconfig/node<versao> dele (no nosso caso: @tsconfig/node18). Assim, sera redirecionado para a pagina do tsconfig da sua versao.
Apos redirecionamento da pagina, procure pelo **"The tsconfig.json"** e copie o codigo, conforme abaixo, apague todo o conteudo de **tsconfig.json** e cole o codigo copiado:
```
{
  "$schema": "https://json.schemastore.org/tsconfig",
  "display": "Node 18",

  "_version": "18.2.0",

  "compilerOptions": {
    "lib": ["es2023"],
    "module": "node16",
    "target": "es2022",

    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "moduleResolution": "node16"
  }
}
```

## INSTALACAO DA DEPENDECIA **tsx** NO AMBIENTE DE DESENVOLVIMENTO (para converter os arquivos .tsx em .js):
`npm i tsx -D`
O tsx faz o processo de conversao do .tsx em .js por "debaixo dos panos", sem precisar criar no projeto uma copia do arquivo .tsx em .js. Para isto, rodamos o comando abaixo, apontando o arquivo que queremos fazer isto:
`npx tsx server.ts`
E, adicionando na linha o comando 'watch', toda vez que alteramos o arquivo e o salvamos, o processo de conversao eh realizado automaticamente:
`npx tsx watch server.ts`
Para facilitar isto, tambem podemos criar um script de comando no arquivo **package.json**:
```
"scripts": {
    "dev": "tsx watch server.ts"
},
```

## INSTALACAO DO FRAMEWORK **fastify** (faz a mesma coisa que o Express, entretanto, ainda possui suporte):
`npm i fastify`

## COMANDO PARA INICIAR O NOSSO BACK-END:
`npm run dev`

## COMANDO PARA MOSTRAR LOG DO CONTAINER QUE QUEREMOS - PASSANDO O ID
```docker logs 269cce757154```