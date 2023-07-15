# Setup react project

## Enviroment

* node
* npm

[install using nvm](https://github.com/nvm-sh/nvm#system-version-of-node)

```
$ brew install nvm
$ mkdir ~/.nvm

//add to ~/.zshrc
export NVM_DIR="$HOME/.nvm"
[ -s "/usr/local/opt/nvm/nvm.sh" ] && \. "/usr/local/opt/nvm/nvm.sh"  # This loads nvm
[ -s "/usr/local/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/usr/local/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion

// install latest lts and npm versions
$ nvm install --lts --latest-npm
```

## create-react-app

```
$ npx create-react-app hacker-test
$ cd hacker-test
```

## Project structure

```
hacker-test/
--node_modules/
--public/
--src/
----App.css
----App.js
----App.test.js
----index.css
----index.js
----logo.svg
--.gitignore
--package-lock.json
--package.json
--README.md
```

* node\_modules/ : contains all node packages installed via npm
* public/ : holds development files
* package.json : list of dependencies
* package-lock.json : This file indicates npm how to break down all node package versions.

package.json

```
{ ... 
  
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
}
```

These scripts are executed with `npm run <script>` . The `run` can be omitted for the start and test scripts.

```
$ npm start
$ npm test
$ npm run build
```

`eject`  It’s a one way operation. Once you eject, you can’t go back. Essentially this command is only there to make all the build tool and configuration from create-react-app accessible if you are not satisfied with the choices or if you want to change something.&#x20;
