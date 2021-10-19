
# SASS Guidelines
> *DISCLAIMER*
>
> These guidelines are proposals, they are not intended to mean that other guidelines are bad or whatever
> 
> These guidelines are personnal experience & parts of other guidelines as well
>
> These guidelines are not proposed as an absolute source of truth, just proposals
>
> These guidelines are open for improvement/modifications if I decide that it fits my needs (if you want, you can fork it to fit your modifications)
>
> These guidelines are meant to be improved, feel free to submit PR to suggest your point of view, I'll be glad to discuss about it !
>
> These guidelines are still partially WIP
>
> *Thanks for reading, have fun !*

## The Atomic Design
### 1. Principle
Atomic design is a way to build components based on 5 concepts
* Atoms: Standalone rendering component (you **NEVER** use them directly outside of molecules)
* Molecules : Composition of X **atoms** into a usable component
* Organisms : Composition of X **molecules** into a scalable component
* Templates : Composition of X **organisms** / **molecules** into a complexe component
* Pages : Instanciation of a **template** with real data

For a detailed (and better) explanation :
https://blog.prototypr.io/how-to-implement-atomic-design-in-your-current-project-368005f5c044
(🚨 If you didn't read the article above ☝️, **DO IT**, it's short and easely readable)

### 2. The typical folder structure
    src
	|__ assets
	    |__ styles
	    |__ components
            |__ atoms
            |__ molecules
            |__ organisms
            |__ templates
            |__ pages

## Naming Conventions

### 1. The name
Remember that a name has to be **AS SHORT AS POSSIBLE** (this will connect to  SRP chapter), `GreenRoundedGithubUserWithDescriptionCard.scss` is a pretty **bad name**.
Even though it's descriptive, it might do **way too many things** if you can't describe it shortly.
Don't over split your logic, this "thing" is actually a simple **molecule** component with modifiers.

### 2. The logic
Let's take our last example : `GreenRoundedGithubUserWithLargeDescriptionCard.scss`
If we take a closer look to this component we can extract 3 parts: 
* **The name** : *GreenRounded**GithubUser**WithLargeDescription**Card** => **GithubUserCard**
*  **The element(s)** : GreenRoundedGithubUser~~With~~Large**Description**Card => **Description**
*  **The modifiers** : **GreenRounded**GithubUserWith**Large**DescriptionCard => **Green / Rounded / Large**

The most painstaking of you might tell themself :

>I recognize it, It's my rulebook ! God save BEM (Block / Element / Modifier)"

*For the others, it's not a shame, juste take a look at this : http://getbem.com/*

Yes it's BEM **logic** but BEM didnt much found out a logic but a **syntaxe**.
And here we come to next chapter ... Syntaxe !

### 3. The syntaxe
If you strictly match BEM logic you could write the previous example as following :

**HTML**

    <div class="GithubUserCard GithubUserCard__green GithubUserCard__rounded">
        <span class="GithubUserCard__description GithubUserCard__description--large"></span>
    </div>

**SCSS**

    .GithubUserCard {
      display: block;
        
      &--description {
        font-size: 14px;
    
        &--large {
          font-weight: 700;
        }
      }
    
      &__green {
        color: green;
      }
    
      &__rounded {
        border-radius: 50%;
      }
    }

I personnally prefer another logic :
* Dashify elements (because I **HATE** underscores, it's up to you though)
* Use data-attributes as modifiers

Why ?
* It makes CSS classes shorter
* data attributes are less powerfull selectors, it then gives you a better granularity over your selectors (for overriding for example)

What doesnt it looks like then ?

**HTML**

    <div class="GithubUserCard" data-rounded data-color="green">
      <span class="GithubUserCard-description" data-size="large"> </span>
    </div>

**SCSS**
    
    .GithubUserCard {
      display: block;
    
      &-description {
        font-size: 14px;
    
        &[data-size="large"] {
          font-weight: 700;
        }
      }
    
      &[data-color="green"] {
        color: green;
      }
    
      &[data-rounded] {
        border-radius: 50%;
      } 
    }

This syntaxe is closer to a React or vueJS "Props" like syntaxe.

Remember the best syntaxe is a mix of 3 parameters :
* Take the one that fits the best for **YOU**
* Take the one that is the most **LOGICAL** to your tech'
* 👉 Don't be afraid to use a different syntaxe than the one you're used to 👈

## In a Real projet

### 1. Only styles / externally bundled styles

    src
    |__ assets
        |__ styles
	        |__ components
	            |__ atoms
	            |   |__ Label.scss
	            |__ molecules
	            |   |__ ProductMiniature.scss
	            |__ organisms
	            |   |__ ProductListing.scss
	            |__ templates
	            |   |__ ListingPage.scss
	            |__ pages
	                |__ Category.scss

### 2. Component based project (React)

> Some directories here are only intended for the example (api, lib etc ...)

#### 1st approach : 


	src
	|__ assets
	|   |__ fonts
	|   |__ images
	|   |__ styles
	|       |__ components
	|           |__ atoms
	|           |   |__ Label.scss
    |           |__ molecules
	|           |   |__ ProductMiniature.scss
	|           |__ organisms
	|           |   |__ ProductListing.scss
	|           |__ templates
	|           |   |__ ListingPage.scss
	|           |__ pages
	|               |__ Category.scss
	|__ main
	    |__ lib
        |__ components
            |__ api
            |   |__ products.js
            |__ renderers
	            |__ atoms
	            |   |__ Label.jsx
                |__ molecules
                |   |__ ProductMiniature.jsx
                |__ organisms
                |   |__ ProductListing.jsx
                |__ templates
                |   |__ ListingPage.jsx
                |__ pages
                    |__ Category.jsx

#### A better way : 

	src
	|__ assets
	|   |__ fonts
	|   |__ images
	|__ main
		|__ lib
	    |__ components
            |__ api
            |   |__ products.js
            |__ renderers
                |__ atoms
                |   |__ Label
                |       |__ index.jsx
                |       |__ styles.scss
                |__ molecules
                |   |__ ProductMiniature
                |       |__ index.jsx
                |       |__ styles.scss
                |__ organisms
                |   |__ ProductListing
                |       |__ index.jsx
                |       |__ styles.scss
                |__ templates
                |   |__ ListingPage
                |       |__ index.jsx
                |       |__ styles.scss
                |__ pages
                    |__ Category
                        |__ index.jsx
                        |__ styles.scss

**Why ?**

Because if you tend to create a "component" based application you have to think in terms of components.
Styles are no longer leading what they do, but belong to something bigger : components.
A component is then represented by  a directory and is composed by a React *(because we use React here)* component *(.jsx file)* and a style *(.scss file)*, but could also be composed by a test file or an another component for loading state.
*For example :* 

    [...]
	    |__ ProductMiniature
			|__ index.(jsx|tsx) => component
			|__ index.test.js => test file (Enzyme)
			|__ styles.(component.(scss|css)) => styles
			|__ skeleton.(jsx|tsx) => loading component

## SRP (Single-responsibility principle)
[... todo]
