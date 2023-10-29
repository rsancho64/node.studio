# node.studio

## ubuntu setup: node, nvm, npm, n

[recipe digitalocean](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-22-04)

Option? : 3 using the **nvm** (user mode, not root) ... `~./nvm` folder ...

## 1: instalando nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh > install.sh
sh install.sh
```

**log:**

```bash
=> Downloading nvm from git to '/home/ray/.nvm'
=> Clonando en '/home/ray/.nvm'...
remote: Enumerating objects: 360, done.
remote: Counting objects: 100% (360/360), done.
remote: Compressing objects: 100% (308/308), done.
remote: Total 360 (delta 40), reused 166 (delta 27), pack-reused 0
Recibiendo objetos: 100% (360/360), 191.90 KiB | 629.00 KiB/s, listo.
Resolviendo deltas: 100% (40/40), listo.
* (HEAD desacoplado en FETCH_HEAD)
  master
=> Compressing and cleaning up git repository

=> Appending nvm source string to /home/ray/.bashrc
=> Appending bash_completion source string to /home/ray/.bashrc
**=> You currently have modules installed globally with `npm`.** These will no
=> longer be linked to the active version of Node when you install a new node
=> with `nvm`; and they may (depending on how you construct your `$PATH`)
=> override the binaries of modules installed with `nvm`:
... # must clean previous
```

### CLEAN previous

[recipe](https://stackoverflow.com/questions/32426601/how-can-i-completely-uninstall-nodejs-npm-and-node-in-ubuntu)

1. eliminamos la instalacion de usuario de nvm: `rm -rf /home/ray/.nvm/`
2. eliminamos lo que queda de node en el usuario: `~/.node_repl_history`
3. eliminamos lo que queda de npm en el sistema, a nivel global:

En synaptic no se ve.
... npm de usuario? si: existe `/home/ray/.npm*` ... eliminado
... npm de de sistema? si: whereis npm: `/usr/local/bin/npm`

`file: /usr/local/bin/npm`: symbolic link to `../lib/node_modules/npm/bin/npm-cli.js`

rm /usr/local/bin/node

```bash
# @ /usr/local/bin/ :
node
corepack -> ../lib/node_modules/corepack/dist/corepack.js
npm -> ../lib/node_modules/npm/bin/npm-cli.js
npx -> ../lib/node_modules/npm/bin/npx-cli.js
```

`sudo rm -Rf node corepack npm npx`

```bash
# @ /usr/local/lib/ :

sudo rm -Rf /node_modules

whereis npm: `/usr/local/bin/npm`
```

### nuevo lanzamiento

`sh install.sh`

y tenemos un log limpio:

```
=> Downloading nvm from git to '/home/ray/.nvm'
=> Clonando en '/home/ray/.nvm'...
remote: Enumerating objects: 360, done.
remote: Counting objects: 100% (360/360), done.
remote: Compressing objects: 100% (308/308), done.
remote: Total 360 (delta 40), reused 168 (delta 27), pack-reused 0
Recibiendo objetos: 100% (360/360), 191.90 KiB | 982.00 KiB/s, listo.
Resolviendo deltas: 100% (40/40), listo.

* (HEAD desacoplado en FETCH_HEAD)
  master
=> Compressing and cleaning up git repository

=> Appending nvm source string to /home/ray/.bashrc
=> Appending bash_completion source string to /home/ray/.bashrc
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"                   # loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # loads nvm bash_completion
```

Y ahora tenemos: **nvm** (si) pero **ningun node** -ni **npm**- 
(npm (npmjs, the Node.js package manager) is included with the Node.js runtime.)

## 2: instalando el primer node

`nvm list` ... en local, nada disponible (todo **ROJO**):

```bash
            N/A
iojs -> N/A (default)
node -> stable (-> N/A) (default)
unstable -> N/A (default)
lts/* -> lts/iron (-> N/A)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3 (-> N/A)
lts/gallium -> v16.20.2 (-> N/A)
lts/hydrogen -> v18.18.2 (-> N/A)
lts/iron -> v20.9.0 (-> N/A)
```

`nvm list-remote` ... `v20.9.0   (Latest LTS: Iron)`

`nvm install lts/iron`   Que el ultimo sea el primero.
`nvm list` ... en local: las versiones disponible se ven en **VERDE**:

```bash
->  v20.9.0
default -> lts/iron (-> v20.9.0) #VERDE
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v20.9.0) (default) #VERDE
stable -> 20.9 (-> v20.9.0) (default) #VERDE
lts/* -> lts/iron (-> v20.9.0)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3 (-> N/A)
lts/gallium -> v16.20.2 (-> N/A)
lts/hydrogen -> v18.18.2 (-> N/A)
lts/iron -> v20.9.0  #VERDE
```

`node -v` : v20.9.0
`whreis node` : /home/ray/.nvm/versions/node/v20.9.0/bin/node

If the version you would like to remove is the current active version,
you’ll first need to deactivate nvm to enable your changes: `nvm deactivate`

# manejo de las versiones de node: paquete n (ene)

`npm install n`

Installing Node.js Versions with n:
Simply execute n <version> to download n install

see [n site](https://www.npmjs.com/package/n)

`npm install carbon` :: instala en la carpeta local, y no en ~/.nvm/versions/

## try node in CLI

```js
$ node
> .help
```

## holamundo de node con en vs code

**node.org** gettingstarted : [**here**](https://nodejs.org/en/docs/guides/getting-started-guide)

helloworld @  [MS site](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)

touch app.js ...

## holamundo angular en vs code

[tuto](https://code.visualstudio.com/docs/nodejs/angular-tutorial)
