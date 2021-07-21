## PhpLokalElectron

Instalação:https://github.com/sidneirs/PhpLokalElectron.git

[TOC]
## Requisitos
Node e Git
##  Crie as pastas e inicialize o projeto 
- projeto
   - php
   - www - Crie o index.php `<?php phpinfo() ?>`
  


- Execute 
```
npm init -y
```
>Será criado o package.json semelhante a esse

```
{
  "name": "projeto",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```
Vamos fazer algumas alterações importantes para evitar problemas futuros

```
{
  "name": "projeto", // Adicione um nome ao projeto
  "version": "1.0.0",
  "description": "",
  "main": "main.js", // Vou mudar o nome
  "scripts": {
   "start": "electron .", //  npm start
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
## Básico main.js

```
const { app, BrowserWindow } = require('electron')

function createWindow () {
  const win = new BrowserWindow({
     
    width: 800,
    height: 600,
    webPreferences: {
     contextIsolation: true,
      }
  })

  win.loadFile('www/index.php')
}

app.whenReady().then(createWindow)

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', () => {
  if (BrowserWindow.getAllWindows().length === 0) {
    createWindow()
  }
})

```
Podemos tentar rodar o sistema npm start

## Instale as dependências
Comece pelo  pacote do electron
```
npm install --save-dev electron-packager 
```
elétron-início rápido
git clone https://github.com/electron/electron-quick-start

##PHP Server Manager
Pacote para gerenciar o servidor embutido em PHP a partir do node.

```
npm i php-server-manager

```


## Configurar a Aplicação para Acessar rede local se conectado a internet
* Instalação 
```
npm install internal-ip
```  
<https://github.com/sindresorhus/internal-ip>

*  Configuração 
 
```
const internalIp = require('internal-ip');
(async () => {
	console.log(await internalIp.v4());	
})();

// Captura o Ip Local do console e  verifica se existe

var internalIp = internalIp.v4.sync();
if(internalIp === 'undefined'){
  ip =  '127.0.0.1'
  console.log('true')
}else{
  ip = internalIp.v4.sync()
  console.log(ip)
}
```


### Configurar PHP Server Manager
* **Criar o servidor**
> o host quando não for definido o servidor será iniciado em 'http://127.0.0.1

> Port quando não definido iniciara em 8000 http://127.0.0.1:8000/

> se não for definido o local php ele usara a instalada no sistema `php: './php'`,
```
// PHP Server Manager
const PHPServer = require('php-server-manager');
const server = new PHPServer({
  php: __dirname + 'php/php.exe',
    host: ip,
    port: 5555,
    directory: __dirname + '/www/',
    directives: {
        display_errors: 1,
        expose_php: 1
    },
    config:__dirname + 'php/php.ini'
});

```
Executar o servidor adicione logo após iniciar a janela da aplicação  `server.run();`

```
  mainWindow = new BrowserWindow({
    //Iniciar  PHP Server Manager 
    server.run();

```
Carrega o url do  php server
```
mainWindow.loadURL('http://'+server.host+':'+server.port+'/')

```
**Importante !!** Finalize o servidor ao fechar
```
// Finaliza o server e fecha a aplicação.
  mainWindow.on('close', function (event) {
    server.close();
    mainWindow = null
  })
```

## Baixe o php e Descompacte na pasta php

Encontre a  versão desejada que esteja compactada em zip
<https://windows.php.net/downloads/releases/archives/>

> Exemplo: php-7.4.9-Win32-vc15-x64.zip

Depois de descompacte na pasta php.

* Crie a Pasta php e baixe a versão do php e descompact dentro da pasta
> se existir php instalado no computador ele é capaz de encontrar é executar, mesmo sem a pasta, porém é preciso remover a seguinte linha:` php: __dirname + '/php/php.exe`', e adicionar uma path com caminho da pasta php, caso isso não ocorra na instalação do php.


* Crie a pasta www e adicione  um index.php
```
<?php
 phpinfo();
>
```


Adicone ao package.json essas linhas
```
"scripts": {
    "start": "electron ."
},

```


## Criador de Aplicativos
Adicione a scripts sem e defina como  --no-asar para conseguir executar

```
"package-mac": "electron-packager . --overwrite --platform=darwin --arch=x64 --no-asar --icon=icons/logo.icns --prune=true --out=release-builds",
"package-win": "electron-packager . electron-tutorial-app --overwrite --no-asar --platform=win32 --arch=ia32 --icon=icons/logo.ico --prune=true --out=release-builds --version-string.CompanyName=CE --version-string.FileDescription=CE --version-string.ProductName=\"Electron Tutorial App\"",
"package-linux": "electron-packager . electron-tutorial-app --overwrite --no-asar --platform=linux --arch=x64 --icon=icons/logo.png --prune=true --out=release-builds"

```

### Mac
```
"package-mac": "electron-packager . --overwrite --platform=darwin --arch=x64 --icon=assets/icons/mac/icon.icns --prune=true --out=release-builds",
``````
### Windows

```
"package-win": "electron-packager . electron-tutorial-app --overwrite --asar=true --platform=win32 --arch=ia32 --icon=assets/icons/win/icon.ico --prune=true --out=release-builds --version-string.CompanyName=CE --version-string.FileDescription=CE --version-string.ProductName=\"Electron Tutorial App\"",    
``````
### Linux

```
"package-linux": "electron-packager . electron-tutorial-app --overwrite --asar=true --platform=linux --arch=x64 --icon=assets/icons/png/1024x1024.png --prune=true --out=release-builds"
``` 
## Defina a Versão estável do electron e execute a instalação
"devDependencies": {
```
    "electron": "^10.1.5",

```    
"electron-packager": "^15.1.0"
  },

## Intalação

```
npm install --save-dev electron

```
## Criar Build
```
npm run package-mac
npm run package-win
npm run package-linux

```

## Configurar Php
Baixe os binários do PHP
Vá para http://windows.php.net/download/ e baixe o PHP para "x86" e "Non Thread Safe". Extraia-o e coloque os binários no diretório "src / php /".

Se você deseja oferecer suporte ao Windows XP, baixe os binários do PHP 5.4 e, adicionalmente, mova todas as extensões DLL do diretório php / ext / para o diretório raiz php /. Consulte a edição 34 para saber por que isso precisa ser feito. Você também terá que criar / modificar php.ini e definir a opção extension_dir:

extension_dir=./
Desmarque essas oções
; Directory in which the loadable extensions (modules) reside.
; http://php.net/extension-dir
extension_dir = "./"
; On windows:
extension_dir = "ext"

Ative sqlite e sqlite 3

Ative para DomPDF mbstring

## Refêrencias
https://www.christianengvall.se/electron-packager-tutorial/
https://github.com/AJ-TechSoul/ELECTRON-4-PHP

