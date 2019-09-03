#react-bootstrap-theme-switcher

A React component for dynamically switching between Bootstrap and [Bootswatch](https://bootswatch.com/) themes. Two lines of code and copy themes to your Web server.

<!-- Update This image -->
<img src="http://demo.ray3.io/theme-switcher/theme-switcher.png" />

<!-- Update Demos
[A live demo is here](http://demo.ray3.io/theme-switcher)

[Code for demo here](https://github.com/raythree/theme-switcher-demo)
-->

### Install
```
npm install react-bootstrap-theme-switcher
```
### Setup
The theme switcher works by dynamically modifying the document's style element to switch between the Bootstrap themes. There are two components:

 * A ```ThemeSwitcher``` component that wraps your top-level component. This is responsible for theme loading and hiding your app during the load.
 * A ```ThemeChooser``` component that displays a dropdown button select menu allowing the user to choose a theme.

The ThemeSwitcher will make sure your app is not displayed until the selected theme is loaded, and will also hide it whenever the ThemeChooser selects a new theme. Here is an example of an app that uses the Redux Provider and React Router rendered in index.js:

```javascript
import { ThemeSwitcher } from 'react-bootstrap-theme-switcher';
const store = configureStore();
const history = syncHistoryWithStore(browserHistory, store);

render(
    <Provider store={store}>
      <ThemeSwitcher defaultTheme="yeti">
        <Router history={history} routes={routes} />
      </ThemeSwitcher>
    </Provider>, document.getElementById('app')
);
```
**NOTE:** You can wrap any top level component with the ```ThemeSwitcher``` *except* for ```Provider``` (Router or any other component is fine). ```Provider``` is special, and you'll get a blank screen if you place it inside the ```ThemeSwitcher```.

To let users swich themes add the ```ThemeChooser``` to one of your pages (e.g. a Settings page). The ```ThemeChooser``` gets passed a reference to the ```ThemeSwitcher``` via the ```React Context``` mechanism, so it can trigger a re-render and not display the children components during theme unloading and reloading.

###ThemeSwitcher props
* ```defaultTheme``` - default theme to use if user has not selected one (default ```'lumen'```)
* ```storeThemeKey``` - name of localStorage key used to save the last theme (default ```theme```)
* ```themes``` - named array of custom themes to choose, formatted as `{THEME: "CSS AS STRING"}`  (default ```null```)
* ```themeOptions``` -  array of themes to display in the ```ThemeChooser``` (default is all Bootswatch themes)

###ThemeChooser props
* ```style``` - custom styles to apply to the ```ThemeChooser``` (default ```{}```)
* ```size``` - size of the ```ThemeChooser```, one of (xs, sm, md, lg) (default ```""```)
* ```change``` - function called when the theme is changed  (default ```null```)


### Theme files (and required Bootstrap and JQuery javascript)

- For convenience the theme switcher comes bundled with the [Bootswatch](https://bootswatch.com/) themes and dependent fontfaces. These files MUST be copied to your distribution folder of your Web server.

- The theme switcher will add and remove themes by changing the style of the document. For example, all theme styles will be places in the documents style tag with the data-type attribute of theme.

```
<style type="text/css" data-type="theme">
	THEME STYLES...
</style>
```

- Here is a sample Webpack config that uses the [CopyWebpackPlugin](https://github.com/kevlened/copy-webpack-plugin) to copy the theme files provided with the distribution into your bundle:

```javascript
import CopyWebpackPlugin from "copy-webpack-plugin";

export default {
  ...
  plugins: [
    new CopyWebpackPlugin([
      { // Theme Assets
        from: "node_modules/react-bootstrap-theme-switcher/themes/",
        to: "assets/",
        toType: "dir"
      }
    ]),
  ...  
```

### Auto reload last theme used

If you want the last theme used to be automatically loaded in the future you can provide the ThemeSwitcher with the name of a localStorage key to use to save the theme name:

```javascript
<ThemeSwitcher defaultTheme="yeti" storeThemeKey="theme" />
```
This way, if no theme is currently loaded 'yeti' will be used, but if the user selects another theme it's name will be saved in localStorage under the ```theme``` key and used in the future until it is changed again.

### Theme selection
By default the ```ThemeChooser``` displays all Bootswatch themes. However, if you only want to use a subset you can specify the theme names via the ```themeOptions``` property of the ```ThemeSwitcher```, for example:

```javascript
let themes = ['default', 'cerulean', 'darkly'];

render(
    <Provider store={store}>
      <ThemeSwitcher defaultTheme='default' storeThemeKey="theme" themeOptions={themes}>
        <Router history={history} routes={routes} />
      </ThemeSwitcher>
    </Provider>, document.getElementById('app')
);
```
Use the lower case theme names, the ```ThemeChooser``` will capitalize them and show them in alphabetical order.
