## Introduction to Babel
* **Babel** is one of the most popular **tools** for **transpiling ES6** code.
* **Babel** was created by **Sebastian McKenzie**(an Australian developer who works at Facebook).
* Especially, **Babel** is the preferred transpiling tool when working with **React**.
* This chapter introduces a few methods of working with **Babel**.
* Since this course has been recorded for more than a year there is some changes. Looking at [ECMAScript compatibility table](https://kangax.github.io/compat-table/es6/), We can notice that browsers now support mostly all features.  


## In-browser Babel transpiling
* The first **method** of working with **Babel** is **in-browser transpiling** which means that your **script** is **transpiled** at **Runtime** in the **browser**. We use this method during **development and testing**. However, it is not preferred on **real projects** as it slows down the website.
* **Script tag** in HTML has an **attribute called** ` type `.
    * This attribute was **mandatory** in **HTML4** but now it is **optional** in **HTML5** since it has a **default value**.
    * It is used to **specify** the **language** of the **script** and **tell** the **browser** how to **interpret** it.
    * The value of ` type ` attribute must be a **MIME** type(internet media type).
    * The default value of ` type ` attribute is **text/javascript** which means that it is a **classic** JS script.

* To use **Babel in-browser transpiling** we need two steps:
    1. Reference **Babel** in an independent script by setting the ` src ` attribute to the [link](https://cdnjs.cloudflare.com/ajax/libs/babel-core/6.1.19/browser.js) for **Babel in-browser**.
    2. Set the ` type ` attribute of your script to ` text/babel `.

* Now we present our first **ES6** feature and see how to perform those previous two steps to use **Babel in-browser transformer**.
    * Our 1st feature is **function default args**. This feature allows you to specify default values for function arguments so that if you invoked the function without passing any arguments, it should use those default values.
    * We use this feature by just assigning the default values to function arguments in function definition.
    ```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="UTF-8" />
            <title>Working with Babel</title>
            <!-- step 1 -->
            <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/6.1.19/browser.js"></script>
            <!-- step 2 -->
            <script type="text/babel">
                // default args feature
                function nameBuilder( fName="Hos", lName="Abdelnaser" ) {
                    console.log( fName + " " + lName );
                }
                nameBuilder();
            </script>
        </head>

        <body>
        </body>    
    </html>
    ```
    * Notes on this example:
        * Script tag can not be self closed like this ` <script src=".../browser.js" />` It does not work. It must be ` <script src=".../browser.js"></script> `.
        * The script runs with no need for **Babel** which means that the browser now supports default args feature.
        * This old version of babel script works but including the latest version of it which is 6.1.19 introduces error in its execution. Instead you can use this link https://unpkg.com/@babel/standalone/babel.min.js.


## Transpiling with webpack
* As we said, **In-browser transpiling** is not a good choice in real projects. Instead we wanna automate the process of **transpiling**. One of the tools used for this purpose is **webpack** which we can install and use through **NPM(Node Package Manager)** from the **terminal**.
* To install webpack:
    1. make sure that **npm** is installed.
    2. install webpack by hitting ` sudo npm install -g webpack `.
* **Webpack** is a great tool used for loading various types of dependencies from CSS, images, to scripts.
* Now we have **webpack** installed. To use it in our project we should first create ` package.json ` file which will hold information about our project and its dependencies. To create this file after changinging directory to your project folder hit ` npm init ` and then fill some info about the project or skip them if it is just a practice.
* Now We need to install **Babel loader** to be used alongside **Webpack** in our project. To instll **Babel loader** hit ` npm install --save-dev babel-loader `.
* Before you begin using **webpack** for loading dependencies you must create a file called ` webpack.config.js ` which contains configurations defining what webpack should do on your project(You can find out how to specify this configurations on webpack official website).
```js
module.exports = {
    entry: '/script.js',
    output: {filename: 'bundle.js'},
    module: {
        loaders: [
            {test: /\.js?/, loader: 'babel-loader', exclude: /node_module}
        ]
    }  
};
```
Hints:
  * ` module.exports ` object contains information about our project.
  * ` module ` object nested in ` module.exports ` contains information about which loaders will be used. In this example we added a loader called ` test ` which looks for .js files and load it using **bable-loader** without **node modules**.

* Now we can run **webpack** simply by hitting ` webpack ` and see the result for our configurations(In this case a new file ` bundle.js ` will be created) which is transpiled from the file ` script.js `).
* Now we can reference this new file ` bundle.js ` in our HTML.
* Here are some additional installs on dev dependencies(I do not know why? We already installed them):
    *  ` npm install webpack@1.12.2 --save-dev `.
    * ` npm install babel-core@5.8.29 --save-dev `
    * ` npm install babel-loader@5.3.2 --save-dev `


## Migrating to webpack 3
* To migarate from webpack 2 to webpack 3 a few changes have to be made:
    1. install **webpack** at a later version.  
    ` npm install webpack@3.1.0 --save-dev `
    2. install  **babel-core** at a later version.  
    ` npm install babel-core@6.25.0 --save-dev `
    3. install **babel-loader** at a newer version.  
    ` npm install babel-loader@7.1.1 --save-dev `
    4. install **babel-present-env**.  
    ` npm install babel-present-env@1.6.0 --save-dev `
    5. Adjust the webpack configurations file ` webpack.config.js `.
    ```js
    var webpack = require('webpack');
    module.exports = {
        entry: '/script.js',
        output: { filename: 'bundle.js'},
        module: {
            rules: [
                {
                  test: /\.js?/,
                  loader: 'babel-loader',
                  exclude: /node_module,
                  query: {
                      preset: ['env']
                    }
                }
            ]
        }  
    };
    ```
    6. Create a new file related to **presets** called ` .babelrc `.  
    ```js
    {
        "presets": ["env"]
    }
    ```  

* Now you can run webpack as before by hitting ` webpack `. If it does not work try this ` ./node_modules/.bin/webpack `.


## Chapter quiz
1. When you use Babel, what is the output?  
code that works in a browser.
2. What attribute must you add to a script tag when using the in-browser transpiler?  
type="text/babel".
3. What does webpack help developers to do?  
Automate processes like transpiling and sass to css conversion.
4. What is the most important change in the webpack config file from webpack 1 to webpack 3?  
the object key called "loaders" becomes "rules".
