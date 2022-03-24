This is a reduced test case for the issue raised about `npm install` timing out inside node 16 docker images.

My machine is an amd64 machine running Ubuntu 20.04.

## Test node 16 (times out)
`docker build .`

Produces:

```shell
Sending build context to Docker daemon  2.276MB
Step 1/4 : FROM node:16-alpine
 ---> 633ac691384c
Step 2/4 : WORKDIR /app
 ---> Using cache
 ---> 3756f06ddb74
Step 3/4 : COPY package.json ./
 ---> a165797b481b
Step 4/4 : RUN npm install
 ---> Running in 09e15582a3b6
npm notice 
npm notice New patch version of npm available! 8.5.0 -> 8.5.5
npm notice Changelog: <https://github.com/npm/cli/releases/tag/v8.5.5>
npm notice Run `npm install -g npm@8.5.5` to update!
npm notice 
npm ERR! code ERR_SOCKET_TIMEOUT
npm ERR! errno ERR_SOCKET_TIMEOUT
npm ERR! network Invalid response body while trying to fetch https://registry.npmjs.org/semver: Socket timeout
npm ERR! network This is a problem related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network 
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2022-03-24T16_24_21_529Z-debug-0.log
```

## Test node 14 (doesn't time out)
`docker build . -f Dockerfile-14`

Produces:
```
Sending build context to Docker daemon  2.277MB
Step 1/4 : FROM node:14-alpine
 ---> 1e6c9ff4db81
Step 2/4 : WORKDIR /app
 ---> Running in 28a0b00dd44e
Removing intermediate container 28a0b00dd44e
 ---> 602715a2a6ac
Step 3/4 : COPY package.json ./
 ---> 1bc7fcf069b4
Step 4/4 : RUN npm install
 ---> Running in e48cf6aea4d6

> prisma@3.11.0 preinstall /app/node_modules/prisma
> node scripts/preinstall-entry.js


> cpu-features@0.0.3 install /app/node_modules/cpu-features
> node buildcheck.js > buildcheck.gypi && node-gyp rebuild

/app/node_modules/buildcheck/lib/index.js:115
  throw new Error('Unable to detect compiler type');
  ^

Error: Unable to detect compiler type
    at BuildEnvironment.getKind (/app/node_modules/buildcheck/lib/index.js:115:9)
    at BuildEnvironment.tryCompile (/app/node_modules/buildcheck/lib/index.js:537:15)
    at BuildEnvironment.checkHeader (/app/node_modules/buildcheck/lib/index.js:423:25)
    at Object.<anonymous> (/app/node_modules/cpu-features/buildcheck.js:16:4)
    at Module._compile (internal/modules/cjs/loader.js:1085:14)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1114:10)
    at Module.load (internal/modules/cjs/loader.js:950:32)
    at Function.Module._load (internal/modules/cjs/loader.js:790:12)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:75:12)
    at internal/main/run_main_module.js:17:47

> ssh2@1.8.0 install /app/node_modules/ssh2
> node install.js

gyp ERR! find Python 
gyp ERR! find Python Python is not set from command line or npm configuration
gyp ERR! find Python Python is not set from environment variable PYTHON
gyp ERR! find Python checking if "python" can be used
gyp ERR! find Python - "python" is not in PATH or produced an error
gyp ERR! find Python checking if "python2" can be used
gyp ERR! find Python - "python2" is not in PATH or produced an error
gyp ERR! find Python checking if "python3" can be used
gyp ERR! find Python - "python3" is not in PATH or produced an error
gyp ERR! find Python 
gyp ERR! find Python **********************************************************
gyp ERR! find Python You need to install the latest version of Python.
gyp ERR! find Python Node-gyp should be able to find and use Python. If not,
gyp ERR! find Python you can try one of the following options:
gyp ERR! find Python - Use the switch --python="/path/to/pythonexecutable"
gyp ERR! find Python   (accepted by both node-gyp and npm)
gyp ERR! find Python - Set the environment variable PYTHON
gyp ERR! find Python - Set the npm configuration variable python:
gyp ERR! find Python   npm config set python "/path/to/pythonexecutable"
gyp ERR! find Python For more information consult the documentation at:
gyp ERR! find Python https://github.com/nodejs/node-gyp#installation
gyp ERR! find Python **********************************************************
gyp ERR! find Python 
gyp ERR! configure error 
gyp ERR! stack Error: Could not find any Python installation to use
gyp ERR! stack     at PythonFinder.fail (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/find-python.js:307:47)
gyp ERR! stack     at PythonFinder.runChecks (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/find-python.js:136:21)
gyp ERR! stack     at PythonFinder.<anonymous> (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/find-python.js:179:16)
gyp ERR! stack     at PythonFinder.execFileCallback (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/find-python.js:271:16)
gyp ERR! stack     at exithandler (child_process.js:390:5)
gyp ERR! stack     at ChildProcess.errorhandler (child_process.js:402:5)
gyp ERR! stack     at ChildProcess.emit (events.js:400:28)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:280:12)
gyp ERR! stack     at onErrorNT (internal/child_process.js:469:16)
gyp ERR! stack     at processTicksAndRejections (internal/process/task_queues.js:82:21)
gyp ERR! System Linux 5.13.0-35-generic
gyp ERR! command "/usr/local/bin/node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "--target=v14.19.1" "--real_openssl_major=1" "rebuild"
gyp ERR! cwd /app/node_modules/ssh2/lib/protocol/crypto
gyp ERR! node -v v14.19.1
gyp ERR! node-gyp -v v5.1.0
gyp ERR! not ok 
Failed to build optional crypto binding

> prisma@3.11.0 install /app/node_modules/prisma
> node scripts/install-entry.js


> @prisma/engines@3.11.0-48.b371888aaf8f51357c7457d836b86d12da91658b postinstall /app/node_modules/@prisma/engines
> node download/index.js


> protobufjs@6.11.2 postinstall /app/node_modules/protobufjs
> node scripts/postinstall


> @nestjs/core@8.4.2 postinstall /app/node_modules/@nestjs/core
> opencollective || exit 0

                           Thanks for installing nest 
                 Please consider donating to our open collective
                        to help us maintain this package.
                                         
                            Number of contributors: 0
                              Number of backers: 740
                              Annual budget: $88,539
                             Current balance: $3,476
                                         
             Become a partner: https://opencollective.com/nest/donate
                                         

> @prisma/client@3.11.0 postinstall /app/node_modules/@prisma/client
> node scripts/postinstall.js

prisma:warn The postinstall script automatically ran `prisma generate` and did not find your `prisma/schema.prisma`.
If you have a Prisma schema file in a custom path, you will need to run
`prisma generate --schema=./path/to/your/schema.prisma` to generate Prisma Client.
If you do not have a Prisma schema file yet, you can ignore this message.

npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@~2.3.2 (node_modules/chokidar/node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.3.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})
npm WARN @firebase/database-compat@0.1.6 requires a peer of @firebase/app-compat@0.x but none is installed. You must install peer dependencies yourself.
npm WARN docker-node-repro@0.0.1 No description
npm WARN docker-node-repro@0.0.1 No repository field.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: cpu-features@0.0.3 (node_modules/cpu-features):
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: cpu-features@0.0.3 install: `node buildcheck.js > buildcheck.gypi && node-gyp rebuild`
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: Exit status 1

added 947 packages from 681 contributors and audited 955 packages in 106.191s

88 packages are looking for funding
  run `npm fund` for details

found 1 high severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
Removing intermediate container e48cf6aea4d6
 ---> 3e6f398c5a9c
Successfully built 3e6f398c5a9c
```
