# SASS
Sass is a CSS preprocessor. It lets you develop your styles in more interesting and efficient way and then it turns those styles into CSS your browser can read.

## Setup

#### Missy's repo
Missy's setup repository is a helpful way to understand how to set up Sass in your React app. Although I include some setup shortcuts below, I recommend reading her repo to understand the parts involved and to read some of her tips.
https://github.com/missyjeanbeutler/sass-demo

#### Terminal commands

``` bash
sassme() {
    npm install node-sass-chokidar && 
    json -If package.json -e 'this.scripts["build-css"] = "node-sass-chokidar src/ -o src/"' &&
    json -If package.json -e 'this.scripts["watch-css"] = "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive"' &&
    echo -e '\n# SASS \nsrc/**/*.css'>> .gitignore && 
    npm i --save npm-run-all
    json -If package.json -e 'this.scripts["start-js"] = "react-scripts start"' &&
    json -If package.json -e 'this.scripts["start"] = "npm-run-all -p watch-css start-js"' &&
    json -If package.json -e 'this.scripts["build"] = "npm run build-css && react-scripts build"'
}
```


#### .bash_profile (and .bashrc)
Here are some steps I took to set up Sass quickly in any React app. These steps are not required. You can simply follow the steps Missy outlines if you prefer. These steps involve editing a hidden file in your home folder.

- In your Terminal (or other command line interface), use ```cd``` to go to your home folder (usually, opening a new window will start you off in the home folder). In home, you can type ```ls -al``` to see all files, including hidden files. Look to see if you have a .bash_profile or .bashrc file there.

alias bashpro='open -a Visual\ Studio\ Code ~/.bash_profile'
ls -al



#### Partials