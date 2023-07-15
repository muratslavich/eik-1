# npm

[https://www.robinwieruch.de/npm-crash-course/](https://www.robinwieruch.de/npm-crash-course/)

**node package manager**

You can either install these packages to your global node package folder or to your local project folder.

Global node packages are accessible from everywhere in the terminal, and only need to be installed to the global directory once. Install a global package by typing the following into a terminal:

```
$ npm install -g <package>
-g install globally
```

* global mode - packages are installed in
* $ npm config get prefix
* {prefix} -&#x20;

The installed package will automatically appear in a folder called _node\_modules/_ and will be listed in the _package.json_ and _package-lock.json_ files next to your other dependencies.

Initialize the _node\_modules/_ folder and the _package.json_ file for a project.

```
$ npm init -y
-y flag initializes all the defaults in your package.json
```

The _package.json_ and _package-lock.json_ files allow you to share your project with other developers without sharing all the node packages from the _node\_modules/_ folder. It will contain references to all node packages used in your project, called dependencies. Other users can copy a project without the actual dependencies using the references in _package.json_, where the references make it easy to install all packages using npm install. A npm install script will take all the dependencies listed in the _package.json_ file and install them in the _node\_modules/_ folder of your project.

```
$ npm install --save-dev <package>
--save-dev flag indicates that the node package is only used in the development environment
```

If you want to uninstall a node package, type the following command and it will disappear from your _node\_modules/_ folder and _package.json_ file:

```
$ npm uninstall <package>
```

**Yarn** is a dependency manager that works similar to **npm**. It has its own list of commands, but you still have access to the same npm registry. Yarn was created to solve issues npm couldn't, but both tools have evolved to the point where either will suffice today.
