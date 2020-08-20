# amy

[![Build Status](https://travis-ci.org/stfsy/node-amy.svg)](https://travis-ci.org/stfsy/node-amy)
[![Dependency Status](https://img.shields.io/david/stfsy/node-amy.svg)](https://github.com/stfsy/node-amy/blob/master/package.json)
[![DevDependency Status](https://img.shields.io/david/dev/stfsy/node-amy.svg)](https://github.com/stfsy/node-amy/blob/master/package.json)
[![Npm downloads](https://img.shields.io/npm/dm/node-amy.svg)](https://www.npmjs.com/package/node-amy)
[![Npm Version](https://img.shields.io/npm/v/node-amy.svg)](https://www.npmjs.com/package/node-amy)
[![Git tag](https://img.shields.io/github/tag/stfsy/node-amy.svg)](https://github.com/stfsy/node-amy/releases)
[![Github issues](https://img.shields.io/github/issues/stfsy/node-amy.svg)](https://github.com/stfsy/node-amy/issues)
[![License](https://img.shields.io/npm/l/node-amy.svg)](https://github.com/stfsy/node-amy/blob/master/LICENSE)

### A HTML template framework **without** client-side dependencies. 

**amy** allows you to split up your web app in small components. **amy** will merge these components at runtime and replace variables. 

### Example
In the example below you can see, that the file index.html contains various **import** commands. At runtime these commands will add
* Meta Tags,
* CSS,
* JavaScript,
* and of course the content of the page
#### index.html
```HTML
<!DOCTYPE html>
<html>

<head>
    <!-- @amy import amy/views/base/meta.html-->
    <!-- @amy import amy/views/base/css.html-->
</head>

<body>
    ...
    <!-- @amy import amy/views/experience/card.html with stfsy-->
    ...
</body>
<!-- @amy import amy/views/base/scripts.html -->
</html>
```

### Commands
#### import
- Description: Will import a component into the current html page
- Syntax: `<!-- @amy import path/to/file.html [with contextName [as contextAlias]]-->`
- Requirements: 
  - path/to/file.html must be a valid relative path to a file
  - contextName is a property in the current rendering context

#### forEach
```HTML
<div>
    <div class="mdl-card__title">
        <h2 class="mdl-card__title-text">Work Experience</h2>
    </div>
    <div class="mdl-card__supporting-text">
    <!-- @amy import amy/views/experience/blocks/listItem.html forEach experience as experience -->
    </div>
</div>
```
- Description: Will import a component multiple times into the current html page
- Syntax: `<!-- @amy import path/to/file.html forEach context [as contextAlias]-->`
- Requirements: 
  - path/to/file.html must be a valid relative path to a file
  - contextName is a property in the current rendering context
  - the value of context[contextName] must be of type Array

#### add
- Description: Will import a component into another component
- Syntax: `<!-- @amy import path/to/file.html [with contextName [as contextAlias]] and add path/to/another/file.html [with contextName [as contextAlias]] -->`

#### interpolation
``` HTML
<li class="mdl-list__item">
    <span class="mdl-chip__contact">{{ experience.label }}</span>
    </span>
    <span>{{ experience.text }}</span>
    </span>
</li>
```
- Description: Will declare variables in a component that will be replaced at runtime
- Syntax: `{{ variableName }}`

###  import
```HTML
<div>
    <div class="mdl-card__title">
        <h2 class="mdl-card__title-text">Experience</h2>
    </div>
    <div class="mdl-card__supporting-text">
       <!-- @amy import amy/views/experience/blocks/list.html with experience as experience-->
    </div>
</div>
```
Before importing **amy/views/experience/card.html**, gets the value (*v1*) of the key _stfsy_ of the current context. All commands inside the **card.html** file will only get access to the value *v1*. Inside the **card.html** file is another **import** 
command, that will get the value (*v2*) of the key _experience_ of the value *v1* and so on..
#### amy/views/experience/blocks/list.html
```HTML
<ul class="mdl-list stfsy-list-no-margin">
    <!-- @amy import amy/views/experience/blocks/listItem.html forEach experience as experience -->
</ul>
```
The **forEach** command will add **amy/views/experience/blocks/listItem.html** for each element in the array that is the value 
of the key _experience_ of the current context. Each import is invoked with a context object that contains a single element
of the array. Each context element will be accessible through the key **experience** as stated by the **as** command.

#### amy/views/experience/blocks/listItem.html
``` HTML
<li class="mdl-list__item">
    <span class="mdl-list__item-primary-content">
    <span class="mdl-chip stfsy-chip mdl-chip--contact">
    <span class="mdl-chip__contact mdl-color--secondary mdl-color-text--primary">{{ experience.label }}</span>
    </span>
    <span>{{ experience.text }}</span>
    </span>
</li>
```
Expecting the current context to contain an object named __experience__ the **import** command will replace 
both placeholders with the actual values.

## Installation

```
npm install node-amy --save
```

## Documentation

[node-amy JSDoc](https://stfsy.github.io/node-amy)

## License

This project is distributed under the MIT license.