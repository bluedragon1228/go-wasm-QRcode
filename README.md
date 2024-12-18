# go-wasm-qrcode

A sample project that uses WebAssembly by Go.

Wasm:

- Go

Front-end app:

- TypeScript
- React
    - Create-React-App
    - Material-UI

## TODO

- Provides and use decralation file (`.d.ts`)

## Requirement

- Go (>= 1.16)
- Node.js (>= 14.15.0)
- Yarn 

and UNIX like shell environment which can use `make` and some commands used in `Makefile`.

## Project layouts

```
.
├── app         ... front-end app
│   ├── build   ... production build
│   ├── public  ... public assets
│   └── src     ... front-end codes
└── wasm        ... WebAssembly codes
```

## Run application

`make run` builds wasm binary and run app by `yarn start`.

```sh
# build wasm and run development app
make run
```

### Windows (PowerShell)

```sh
# build wasm
cd wasm
$Env:GOOS = "js"
$Env:GOARCH = "wasm"
go build -o qrcode.wasm

# copy .wasm into app's assets directory
cp qrcode.wasm ../app/public

# go to app dir
cd ../app

# install dependencies
yarn

# run
yarn start
```

### Windows (cmd.exe)

```sh
# build wasm
cd wasm
set GOOS=js
set GOARCH=wasm
go build -o qrcode.wasm

# copy .wasm into app's assets directory
copy qrcode.wasm ..\app\public

# go to app dir
cd ..\app

# install dependencies
yarn

# run
yarn start
```

## Inside

### WebAssembly part (`./wasm`)

`wasm` directory contains WebAssembly code that provides `generateQRCode()` function. `wasm` directory is Go project.

```sh
cd wasm
```

#### Build

```sh
# build
make
```

or just use `go build`.

```sh
GOOS=js GOARCH=wasm go build -o qrcode.wasm
```

##### Windows (PowerShell)

```sh
# build
$Env:GOOS = "js"
$Env:GOARCH = "wasm"
go build -o qrcode.wasm
```

##### Windows (cmd.exe)

```sh
# build
set GOOS=js
set GOARCH=wasm
go build -o qrcode.wasm
```

#### Run standalone web server

```sh
# run instant web server
make run-server
```

or use your favorite web server.

```sh
# Node.js
npx http-server -p 8080 -c-1

# Python3
python3 -m http.server 8080
```

Open http://localhost:8080/wasm_exec.html

If you can't see a QR Code image, open browser's developer tools panel and check `Content-Type` of `qrcode.wasm`.
`.wasm` files must be `application/wasm`, not `application/octet-stream`.
Try to use another web server that handle `.wasm` MIME type correctly.

### Front-end app part (`./app`)

`app` directory contains front-end application code that uses `qrcode.wasm` built at previous part.
`app` directory is CRA (Create-React-App) project with TypeScript.

```sh
cd app
```

#### Install dependencies

```sh
# install dependencies
yarn
```

#### Copy `qrcode.wasm`

Copy `qrcode.wasm` that was built at wasm part.

```sh
# copy .wasm into public directory
cp ../wasm/qrcode.wasm ./public/
```

#### Run

```sh
# run development app
yarn start
```

#### Build production application

```sh
# build
yarn build

# deploy ./build dir
```

## License

MIT

## Author

Diego (@bluedragon1228)
