# node.studio @ubuntu

## setup: node, nvm, npm, n

recipe: [**digitalocean**](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-22-04)

W Option **3**: using the **nvm** (user mode, not root) ... `~./nvm` folder ...

## nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh > install.sh
sh install.sh
```

***log:***

*=> Downloading nvm from git to '/home/ray/.nvm'*
*=> Clonando en '/home/ray/.nvm'...*
*remote: Enumerating objects: 360, done.*
*remote: Counting objects: 100% (360/360), done.*
*remote: Compressing objects: 100% (308/308), done.*
*remote: Total 360 (delta 40), reused 166 (delta 27), pack-reused 0*
*Recibiendo objetos: 100% (360/360), 191.90 KiB | 629.00 KiB/s, listo.*
*Resolviendo deltas: 100% (40/40), listo.*
*(HEAD desacoplado en FETCH_HEAD)*
*  master*
*=> Compressing and cleaning up git repository*

*=> Appending nvm source string to /home/ray/.bashrc*
*=> Appending bash_completion source string to /home/ray/.bashrc*
*=> You currently have modules installed globally with `npm`.** These will no*
*=> longer be linked to the active version of Node when you install a new node*
*=> with `nvm`; and they may (depending on how you construct your `$PATH`)*
*=> override the binaries of modules installed with `nvm`:*
...
**#must clean previous**

### CLEAN previous

[**recipe**](https://stackoverflow.com/questions/32426601/how-can-i-completely-uninstall-nodejs-npm-and-node-in-ubuntu)

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
[npx?](https://tincode.es/blog/diferencia-entre-los-comandos-npm-y-npx)

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

`nvm install lts/iron` Que el ultimo sea el primero.

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

see [**n site**](https://www.npmjs.com/package/n)

`npm install carbon` :: instala en la carpeta local, y no en ~/.nvm/versions/

## try node in CLI

```js
ray@silver:~/node.studio$ node
Welcome to Node.js v20.9.0.
Type ".help" for more information.
> .help
.break    Sometimes you get stuck, this gets you out
.clear    Alias for .break
.editor   Enter editor mode
.exit     Exit the REPL
.help     Print this help message
.load     Load JS from a file into the REPL session
.save     Save all evaluated commands in this REPL session to a file

Press Ctrl+C to abort current expression, Ctrl+D to exit the REPL
> console.log("hellopues");
hellopues
undefined
> 2+3
5
> nombre = "ramon";
'ramon'
> nombre
'ramon'
> .exit
```

## Tasks n to do's:

- [x] holamundo @[MS site](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial) ; touch app.js ...

- [x] holamundo de Node.js en vs code: **node.org** gettingstarted : [**here**](https://nodejs.org/en/docs/guides/getting-started-guide)

- [ ] holamundo angular en vs code: [**tuto**](https://code.visualstudio.com/docs/nodejs/angular-tutorial)

- [ ] **BROKEN LINK** dokerizando una web app Node.js: [**tuto**](https://nodejs.org/en/docs/guides/nodejs-docker-webapp)
  ,, try (google nodejs.org nodejs docker webapp)
  - [ ] https://www.digitalocean.com/community/tutorials/how-to-build-a-node-js-application-with-docker

- [ ] nodejs-redis-caching [**video**](https://www.youtube.com/watch?v=QUmM8jdviLg) n [**repo**](https://github.com/FaztWeb/nodejs-redis-caching)https://github.com/FaztWeb/nodejs-redis-caching

******

## doker 101

[**https://docs.docker.com/get-started/**](https://docs.docker.com/get-started/) ...
[**https://docs.docker.com/get-started/02_our_app/**](https://docs.docker.com/get-started/02_our_app/) ,, 
- [x] install [latest**Docker Desktop**](https://docs.docker.com/get-docker/)

https://docs.docker.com/desktop/install/linux-install/ :

```bash
modprobe kvm ,,
modprobe kvm_intel  # Intel processors ,,
sudo apt install cpu-checker
kvm-ok #: out: /dev/kvm exists ; KVM acceleration can be used
lsmod | grep kvm #: INFO:
  kvm_intel             503808  0
  kvm                  1347584  1 kvm_intel
  irqbypass              16384  1 kvm
```

```bash
file /dev/kvm # /dev/kvm: character special (10/232) 
ls -al /dev/kvm #: out: crw-rw----+ 1 root kvm *10, 232 nov  1 23:21 /dev/kvm  ,,:
sudo usermod -aG kvm $USER
```

prev clean if any:
`rm -rf $HOME/.docker/desktop` 
`sudo rm -f /usr/local/bin/com.docker.cli` 
`sudo apt purge docker-desktop`

Add Docker's official GPG key:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Add the repository to Apt sources:

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

INSTALL DOCKER SW -NOW-

`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

DOCKER HELLOWORLD:

`sudo docker run hello-world`
```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:88ec0acaa3ec199d3b7eaf73588f4518c25f9d34f58ce9a0df68429c5af48e8d
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 
1. The Docker client contacted the Docker daemon.
2. The Docker daemon:
   1. **pulled the "hello-world" image** from the Docker Hub. (amd64)
   2. **created a new container from that image** which 
      1. runs the executable that produces the output you are currently reading.
   3. streamed that output to the Docker client, which sent it to your terminal.
```

- [x] more ambitious: run an *Ubuntu container* with `docker run -it ubuntu bash`

```bash
root@01368d9ea090:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@01368d9ea090:/# exit
exit
```

- [x] allow normal user to run Docker commands and optional config steps: [**linux postinstall**](https://docs.docker.com/engine/install/linux-postinstall) 

```bash
sudo groupadd docker # create the docker group.
sudo usermod -aG docker $USER # add your user to the docker group.
# now: Log in to the new docker group (to avoid having to log out / log in again; 
# but if not enough, try to reboot):
#- [ ] Logout n log-back-in so that your group membership is re-evaluated.
newgrp docker
#sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
#sudo chmod g+rwx "$HOME/.docker" -R
#try no-sudo:
docker run hello-world # ok!
```

### docker to start on boot (w systmd)

[**see config daemon**](https://docs.docker.com/config/daemon/systemd/)

## utileria utileria utileria utileria utileria 

- sudo systemctl enable docker.service
- sudo systemctl enable containerd.service

vs

- sudo systemctl disable docker.service
- sudo systemctl disable containerd.service


- [?] [Configure default logging driver](https://docs.docker.com/engine/install/linux-postinstall/#configure-default-logging-driver)

## xtras docker

- [ ] get a free Docker ID: [**https://hub.docker.com/**](https://hub.docker.com/) to share images, automate workflows, and more. 

  - [X] get [**docker desktop app**](https://docs.docker.com/desktop/install/linux-install/)  ... ubuntu ... 
  
  `.deb` ... `dpkg -i` :

  ```bash

  sudo dpkg -i /home/ray/Descargas/docker-desktop-4.25.0-amd64.deb
  ...
  dpkg: problemas de dependencias impiden la configuración de docker-desktop:
  docker-desktop depende de qemu-system-x86 (>= 5.2.0); sin embargo:
  El paquete `qemu-system-x86' no está instalado.
  docker-desktop depende de pass; sin embargo:
  El paquete `pass' no está instalado.
  docker-desktop depende de uidmap; sin embargo:
  El paquete `uidmap' no está instalado.

  sudo apt install qemu-system-x86 # ...
  sudo apt install pass # ya esta!?
  sudo apt install uidmap # ya esta!?

  sudo dpkg -i /home/ray/Descargas/docker-desktop-4.25.0-amd64.deb # ... ... OK NOW
  ```

  Docker Desktop inicia pero no conecta. link -> [**howto fix credentials**](https://docs.docker.com/desktop/get-started/#credentials-management-for-linux-users) :

  ```bash

  gpg --generate-key  # set:  user+email+pass
  ...
  gpg: clave 482E3B59F0E48E2F marcada como de confianza absoluta
  gpg: creado el directorio '/home/ray/.gnupg/openpgp-revocs.d'
  gpg: certificado de revocación guardado como '/home/ray/.gnupg/openpgp-revocs.d/<<gpg-id_public_key>>.rev'
  claves pública y secreta creadas y firmadas.

  pub   rsa3072 2023-11-02 [SC] [caduca: 2025-11-01]
        <<gpg-id_public_key>>
  uid                      ramon sancho <rsancho@iesrioarba.es>
  sub   rsa3072 2023-11-02 [E] [caduca: 2025-11-01]

  # now:
  `pass init <your_generated_gpg-id_public_key>`
  mkdir: se ha creado el directorio '/home/ray/.password-store/'
  Password store initialized for <<gpg-id_public_key>>

  ```
  Docker Desktop credentials fixed


- [ ] visit: [**https://docs.docker.com/get-started/**](https://docs.docker.com/get-started/) 4 more examples and ideas. 


## Construir imagen Docker desde un proyecto en vscode

- [ ] **M.W.E.**: [https://code.visualstudio.com/docs/containers/quickstart-node)]

Para hacerse idea del encademamiento de fases y dependencias:

- [ ] see ytb: ["Node y Docker: Crear y subir imagen a Docker Hub" (t:2:47)](https://youtu.be/l-UZQjdPUAI?t=247)
