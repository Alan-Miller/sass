# SASS
Sass is a CSS preprocessor. It lets you develop your styles in more interesting and efficient way and then it turns those styles into CSS your browser can read.

## Setup

#### Missy's repo
Missy's setup repository is a helpful way to understand how to set up Sass in your React app. Although I include some setup shortcuts below, I recommend reading her repo to understand the parts involved and to read some of her tips.
https://github.com/missyjeanbeutler/sass-demo

#### Terminal commands

``` bash
  npm install node-sass-chokidar
  json -If package.json -e 'this.scripts["build-css"] = "node-sass-chokidar src/ -o src/"'
  json -If package.json -e 'this.scripts["watch-css"] = "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive"'
  echo -e '\n# SASS \nsrc/**/*.css'>> .gitignore
  npm i --save npm-run-all
  json -If package.json -e 'this.scripts["start-js"] = "react-scripts start"'
  json -If package.json -e 'this.scripts["start"] = "npm-run-all -p watch-css start-js"'
  json -If package.json -e 'this.scripts["build"] = "npm run build-css && react-scripts build"'
```


#### .bash_profile (and .bashrc)
Here are some steps I took once so I could always set up Sass quickly in any React app. It involves editing a hidden file in your home folder. These steps are not required. You can simply follow the steps Missy outlines if you prefer. 

- In your Terminal (or other command line interface), use ```cd``` to go to your home folder (usually, opening a new window will start you off in the home folder). In home, you can type ```ls -al``` to see all files, including hidden files. Look to see if you have a .bash_profile or .bashrc file there.
- If you don't have a .bash_profile, you can create one using ```touch .bash_profile``` (again, make sure you are in your home folder).
- Open your .bash_profile using ```open -a Visual\ Studio\ Code ~/.bash_profile```. This tells your computer to open .bash_profile in Visual Studio Code (remember to escape spaces in the name using '\ '). If you are using a different code editor, you will have to put that name here instead.
- Once you have opened the .bash_profile in your editor, you can make aliases or functions to speed up and simplify the sorts of things you do in your command line. First of all, I recommend making a little shortcut for quickly opening your .bash_profile in the future. Try ```alias bashprofile='open -a Visual\ Studio\ Code ~/.bash_profile'```. Once you do that, you can type ```bashprofile``` in your command line—from any folder—and it will open that file in your editor. (NOTE: When you create new aliases or functions, you will have to open a new Terminal/CLI tab or window before you can use them.)

alias bashpro='open -a Visual\ Studio\ Code ~/.bash_profile'

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

#### Partials