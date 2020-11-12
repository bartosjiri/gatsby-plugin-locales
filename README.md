# gatsby-plugin-locales

A [Gatsby](https://www.gatsbyjs.com) plugin enabling an internationalization of site content and URLs.

## ⚠ Notice

The plugin is currently incomplete, still under development, and not working out-of-the box. Consider using other available [Gatsby plugins](https://www.gatsbyjs.com/plugins/) in your project if you are looking for a stable solution.

Also, please, feel free to improve the plugin through questions, suggestions and pull requests. The roadmap of planned/desired features can be found in the [projects tab](https://github.com/bartosjiri/gatsby-plugin-locales/projects).


## What it does

- Adds multi-language page content and URL routes from a single page source file. For example, create pages at `/en/page-1` and `/cs/stranka-1` routes from a single `/src/pages/page-1.js` file.
- Supports automatic redirection to user's preferred language provided by the browser settings.

## How to use

1. Install the package in your Gatsby project folder:
    ```
    npm install --save gatsby-plugin-locales
    ```

2. Add the plugin in the `gatsby-config.js` file:

	- **For example**:
      ```js
      module.exports = {
        // ...
        plugins: [
          // ...
          {
            resolve: 'gatsby-plugin-locales',
            options: {
              locales: ['en', 'cs'],
              defaultLocale: 'en',
              generalFolder: `${__dirname}/content/general`,
              pagesFolder: `${__dirname}/content/pages`
            }
          }
        ]
      }
      ```
    - **Configuration:**
    	- List all languages available for your project in the `locales` array.
    	- Set default language in the `defaultLocale`, which will be used as a fallback language and other things.
    	- Define a folder location for general langauge resource files (used in step 3).
    	- Define a folder location for page-specific language resource files (used in step 4).

3. Add a general JSON language resource files into the `generalFolder` location from previous step:

	- **For example** `content/general/en.json`:

      ```json
      {
        "code": "en",
        "name": "English"
      }
      ```
    - **Description:**
    	- The contents of this file will be accessible on every generated page of the project.
    - **Configuration:**
    	- The file is required, however it can be empty. 

4. Add a page-specific Markdown langauge resource files into the `pagesFolder` location from step 2 for every internationalized page:

	- **For example** source file located at `src/pages/page-1.js` with language file at `content/pages/page-1/en.md`:
	
      ```
      ---
      slug: "page-1"
      title: "Page 1"
      ---
      
      Lorem ipsum dolor sit amet...
      ```
   - **Description:**
   		- The contents of this file will be accessible only for the specific page.
   - **Configuration:**
   		- The `slug` field in the frontmatter is required and will be used as the path for the internationalized page:
   			- A source file located at `src/pages/page-1.js` with language file at `content/pages/page-1/en.md` and `slug: "page-1"` value will generate a page at `/en/page-1` route.
   			- A source file located at `src/pages/page-1.js` with language file at `content/pages/page-1/cs.md` and `slug: "stranka-1"` value will generate a page at `/cs/stranka-1` route.
   		- Nested pages can be generated by providing a resource file at the same folder route while following that route in the `slug` field of the frontmatter:
   			- A source file located at `src/pages/folder-name/page-2.js` with language file at `content/pages/folder-name/page-2/en.md` and `slug: "folder-name/page-2"` value will generate a page at `/en/folder-name/page-2` route.
   			- A source file located at `src/pages/folder-name/page-2.js` with language file at `content/pages/folder-name/page-2/cs.md` and `slug: "nazev-slozky/stranka-2"` value will generate a page at `/cs/nazev-slozky/stranka-2` route.

5. Access the language resources in the page source files through `pageContext`:
	- **For example** `src/pages/page-1.js`:
      ```jsx
      import React from 'react'

      const PageOne = ({pageContext}) => {
          const {locales} = pageContext
          const availableLanguages = Object.keys(locales.general).map(lang => locales[lang].name).join(", ")

        return (
          <div>
            <h1>{locales.page[language].frontmatter.title}</h1>
            <p>{locales.page[language].body}</p>

            <span>This page is available in following langauges: {availableLanguages}</span>
          </div>
        )
      }

      export default PageOne
      ```
    - **Description:**
    	- General language resources provided in step 3 will be available in the `pageContext.locales.general` object.
    	- Page-specific language resources provided in step 4 will be available in the `pageContext.locales.page` object.
    	- The current page language is provided at `pageContext.locales.locale`.

## License
Licensed under the [MIT License](./LICENSE.md).