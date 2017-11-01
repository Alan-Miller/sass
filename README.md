# SASS
Sass is a CSS preprocessor. It lets you develop your styles in more interesting and efficient way and then it turns those styles into CSS your browser can read.

## Setup

#### Missy's repo
Missy's setup repository is a helpful way to understand how to set up Sass in your React app. Although I include some setup shortcuts below, I recommend reading her repo to understand the parts involved and to read some of her tips.
https://github.com/missyjeanbeutler/sass-demo

#### Terminal commands
Below are commands I use to install Sass in my React projects. Notice there are commands for installing Sass, adding scripts to your package.json file, installing npm-run-all, and even adding a line to your .gitignore file. I wrote these commands by following Missy's tutorial to understand what things I needed to set up and then figuring out how to do all those steps from the command line. For example, ```json -If package.json -e``` lets you add a line to the JSON object in the package.json file (```-e``` takes the stuff in quotation marks that follows and evaluates it rather than simply treating it as a simple string value). If things are starting to sound too nerdy or too dense and you just came here to install Sass and get going, the commands below will install Sass in your React project (make sure you are in the main project folder). They follow the steps from Missy's tutorial but do it all from Terminal.

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


#### Editing .bash_profile (or .bashrc)
Here are some steps I took once so I could always set up Sass quickly in any React app. It involves editing a hidden file in your home folder. These steps are not required. You can simply follow the steps Missy outlines if you prefer. 

- In your Terminal (or other command line interface), use ```cd``` to go to your home folder (usually, opening a new window will start you off in the home folder). In home, you can type ```ls -al``` to see all files, including hidden files. Look to see if you have a .bash_profile or .bashrc file there.
- If you don't have a .bash_profile, you can create one using ```touch .bash_profile``` (again, make sure you are in your home folder).
- Open your .bash_profile using ```open -a Visual\ Studio\ Code ~/.bash_profile```. This tells your computer to open .bash_profile in Visual Studio Code (remember to escape spaces in the name using '\ '). If you are using a different code editor, you will have to put that name here instead.
- Once you have opened the .bash_profile in your editor, you can make aliases or functions to speed up and simplify the sorts of things you do in your command line. First of all, I recommend making a little shortcut for quickly opening your .bash_profile in the future. Try ```alias bashprofile='open -a Visual\ Studio\ Code ~/.bash_profile'```. Once you do that, you can type ```bashprofile``` in your command line—from any folder—and it will open that file in your editor. You can make aliases that jump straight to certain folders from anywhere else (e.g., ```alias 26p='cd ~/devmtn/dm26/projects'```) or to shorten common commands (e.g., ```alias gs='git status'```).
- (NOTE: When you create new aliases or functions, you will have to open a new Terminal/CLI tab or window before you can use them.)
- You can also make a function. For instance, below is the function I use to install Sass. It includes all the commands from above and runs them all at once. I called it "sassme," but you can call it whatever you want. Now, any time I need to install Sass, I just ```cd``` into the project folder and then type ```sassme``` and it's done.
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

## Partials
Sass will take your .scss files and create a .css file for each one. So if your React has many components, each with its own .scss file for styling, a .css file will be created for each, leading to many, many style files throughout your app. "Partials" are the exceptions. These are .scss files which do not get a .css copy. Partials are simply .scss files that begin with an underscore ("_").

One of the first things I do when starting a project with Sass is to create a main .scss file. I often call it "main.scss" and put it either at the top level of my project folder or inside a "styles" folder. My goal is to make one main .scss into which I import all my partials. Then I get just one .css file, which is what my app imports (in just one place) and uses everywhere.

- Create a main.scss file. When you run ```npm start```, it will create a .css file to match.
- Create partial .scss files by putting an underscore ("_") at the start of the name.
- Import all your partials into the main .scss file. The order in which you import them can sometimes matter, since the first imports are applied first and then the later imports will tweak those styles if there are any conflicts. So if you use a reset CSS file, you should usually put that at the top. Notice than when importing partial files, you do NOT include the underscore in the import statement, even though the actual file name starts with an underscore. You also do NOT include the .scss file extension. Here is an example of imports. All of them are partials. For another example, just look in the main.scss file in this repo.
```sass
  /* All of these are partials, though we do not add the "_" or the file extension. */
  @import './reset'; /* reset is first because it sets default styles */
  @import './App'; /* App might come second if it has styles that apply to other components */
  @import './components/Home'; /* The order of these 3 might not matter if the styles are modular */
  @import './components/Login';
  @import './components/Admin';
```

## Variables
Variables are great. Use them to apply a specific value to multiple properties throughout your app. Then if the value needs to change, you can simply change the definition of the variable and the change will automatically be applied throughout the app. Take this style for example. Notice there is a pixel value for the height of a nav. Notice also that the .mainSection uses CSS's native calc() tool to calculate the value of its height by subtracting 150px from the total height of the page (in this case, 100vh).
  ```sass
    .nav {
      height: 150px;
    }
    .mainSection {
      height: calc(100vh - 150px);
    }
  ```
What if we want to use that 150px value in many places within the app?
- Define a variable using "$". 
  ```$navHeight: 150px;```
- Apply the variable by simply using the variable name where the value would be.
  ```sass
    .nav {
      height: $navHeight;
    }
  ```

## Nesting
Nesting lets you create more targeted or modular styles without adding unique classes to everything or creating long, cumbersome CSS selector. You do this by mirroring (somewhat) the structure of your HTML/JSX elements within your styles.
```sass
  .App {
    background-color: indianred;
    font-size: 16px;
    .logo { 
      /* These apply to any .logo element inside .App element */
      animation: App-logo-spin infinite 20s linear;
      height: 80px;
      position: absolute;
    }
    .header { 
      /* These apply to any .header element inside .App element */
      background-color: #222;
      color: white;
      font: { /* You can also nest certain properties like this */
        family: 'Gill Sans', sans-serif; /* sets font-family */
        style: bold; /* sets font-style */
        size: 1.2rem; /* sets font-size */
      }
    }
    .Login {
      .header {
        color: black; 
        /* This applies only to .header elements inside .Login element (inside .App).
        It is similar to writing a normal CSS selector like this: 
        .App .Login .header {
          color: black;
        }
        Any .header inside .Login will inherit background-color: #222 but will have 
        color: black (because this style is more specific). Any .header not inside 
        .Login will have color: white, not color: black. */
      }
    }
  }
```
Most of the time I recommend against nesting more than three or four levels deep. You don't have to perfectly mirror the structure of your HTML/JSX elements. Just nest enough to make your app modular, applying unique styles to the more deeply nested elements. If you nest too much, you'll find your styles break every time you make a little change to the structure of your HTML/JSX.

## Evaluation with "#{}"
Sometimes you need Sass to evaluate a variable within a more complex style expression. For this, wrap the thing to be evaluated in ${}. Notice I used it below in the .mainSection height property. This way, CSS's calc() tool is not confused by the variable I put there because it is evaluated as a variable value.
  ```sass
    .nav {
      height: $navHeight;
    }
    .mainSection {
      height: calc(100vh - #{$navHeight});
    }
  ```

## Extending styles
Sass lets you borrow styles from one selector to another with @extend, removing the need for lots of copy and paste. Just use @extend and pick a selector with styles you want to borrow. Here, the .secondaryImage selector uses ```@extend: .mainImage;``` to copy all the styles from .mainImage, and then it sets a different background-image for itself.
```sass 
  .mainImage {
    display: inline-block;
    background-image: url('../../imgs/main.jpg');
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
  }

  .secondaryImage {
    @extend .mainImage;
    background-image: url('../../imgs/secondary.jpg');
  }
```
HINT: You can use "%" to create a sort of "ghost selector" where you create some styles without giving them to any existing element. In other words, the styles only exist to be extended. Then you have other selectors use @extend to copy those styles to places where you need them. The "%" tells Sass that these styles should not be rendered into CSS unless they are extended. Here is an example. The %fancyDiv selector does not by itself add any styles to any element, but .Login and .Home pull the styles off of %fancyDiv using @extend, and then each one adds some other styles.
```sass
  %fancyDiv {
    border-radius: 7px;
    border: solid medium cornflowerblue;
    background-color: tomato;
    height: 200px;
    width: 200px;
    color: white;
  }

  .Login {
    div {
      @extend %fancyDiv;
      border: solid thin black;
    }
  }

  .Home {
    .footer div {
      border: solid thin #444;
    }
    color: black;
  }
```

## Mixins are fun. They work sort of like a combination of @extend and a sort of lightweight function because they copy styles to whatever selector includes them and they allow variables to be passed in as well. 
- To define a mixin, use @mixin and give it a name.
- To apply a mixin, use @include and then use the mixin name, (optionally) passing in variables like a function.
```sass
  @mixin square($side) {
    height: $side;
    width: $side;
  }

  .box1 {
    @include square(100px);
  }

  .box2 {
    @include square(300px);
  }
```
You can also set default values for mixins using colons. Failing to pass an expected value into the mixin above will lead to an error, but failing to pass in a value to a mixin that has a default value will simply lead to the default value being used. Below is a mixin that sets default flex values for a parent container that flexes its children elements. Notice there is no variable passed into the display property because I always want it to be set to flex when this mixin is in use.
```sass
  @mixin flexo($direction: row, $justify: center, $align: center) {
    display: flex;
    flex-direction: $direction;
    justify-contents: $justify;
    align-items: $align;
  }

  /* Here, .photo-container uses the default values of flex without any changes. */
  .photo-container {
    @include flexo;
  }

  /* Here, .message-board passes a value in for the first two parameters (flex-direction and justify-contents). The result is that the parent flexes the children elements using "flex-direction: column" and "justify-contents: flex-start," as well as the default value of "align-items: center." */
  .message-board {
    @include flexo(column, flex-start);
  }  

  /* Here, all default values are kept except we change the align-items value to flex-end. Since we are only passing in one of the values and it is not the first value (i.e., we are not passing in the values in order), we need to specify which variable we are defining by using the variable name and a colon, as well as the value. */
  .bio-info {
    @include flexo($align: flex-end);
  }
  
```



 functions, and loops


Sass Basics: http://sass-lang.com/guide
How to Use Sass Mixins: https://scotch.io/tutorials/how-to-use-sass-mixins