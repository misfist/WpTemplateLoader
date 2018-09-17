# WordPress Plugin Template Loader
A class that allows loading WordPress template parts in plugins. Based on the [Gamajo Template Loader](https://github.com/GaryJones/Gamajo-Template-Loader), adapted to PSR-2 and PSR-4 coding standards.

## Description

This class builds upon the `getTemplatePart()` function in WordPress, but allows you to load template parts from your plugin, with built in fallbacks.

This fork uses modern PHP coding standards such as PSR-2 code formatting, PSR-4 autoloading, and namespacing, which eliminates the need to make a copy of the class, or to use Mozart to prefix the class name.

## Installation

This isn't a WordPress plugin on its own, it must be loaded as a Composer dependency.

`composer require rexrana/wp-plugin-template-loader`

## Usage

### Instantiate class
To instantiate the template loader class, first add a `use` directive to set the namespace of this class, and then assign an instance of the class to a variable. You can optionally pass an array when loading to customize the locations of the theme and plugin template directories, as well as the prefix for action filters. 

  ~~~php
  <?php
  use RexRana\WordPressUtils\PluginTemplateLoader;

  // set arguments for the template loader class (optional).
  $loader_params = array(
    'filter_prefix'             => 'my_plugin_template_loader',
    'plugin_directory'          => plugin_dir_path( dirname( __FILE__ ) ),
    'plugin_template_directory' => 'templates',
    'theme_template_directory'  => 'partials',
  );

  // Instantiate the template loader.
  $my_plugin_template_loader = new PluginTemplateLoader($loader_params);
  ~~~

### Load templates

When you call the `GetTemplatePart` method, it looks for the provided template in the following order of directories:

- the value of `theme_template_directory` property in theme directories
  - child theme
  - parent theme
- the plugin template folder - from `plugin_template_directory` property. 


  ~~~php
  $my_plugin_template_loader->getTemplatePart( 'recipe' );
  ~~~

  Using the parameters used in the instantiation example, it will try to load the template in this order: 
  1. `wp-content/themes/child-theme/partials/recipe.php` 
  2. `wp-content/themes/parent-theme/partials/recipe.php`
  3. `wp-content/plugins/meal-planner/templates/recipe.php`.

### Pass data to templates

If you want to pass data to the template, call the `setTemplateData()` method with an array before calling `getTemplatePart()`. `setTemplateData()` returns the loader object to allow for method chaining.

  ~~~php
  $data = array( 'foo' => 'bar', 'baz' => 'boom' );
  $my_plugin_template_loader
      ->setTemplateData( $data );
      ->getTemplatePart( 'recipe' );
  ~~~
  
  The value of `bar` is now available inside the recipe template as `$data->foo`.
  
  ### changing the variable name
  If you wish to use a different variable name, add a second parameter to `setTemplateData()`:

  ~~~php
  $data = array( 'foo' => 'bar', 'baz' => 'boom' );
  $my_plugin_template_loader
      ->setTemplateData( $data, 'context' )
      ->getTemplatePart( 'recipe', 'ingredients' );
  ~~~
  
  The value of `bar` is now available inside the recipe template as `$context->foo`.

## Change Log

See the [change log](CHANGELOG.md).

## License

[GPL-2.0+](LICENSE).

## Contributions

Contributions are welcome - fork, fix and send pull requests against the `develop` branch please.

## Credits

Original class `Gamajo Template Loader` Built by [Gary Jones](https://twitter.com/GaryJ)  
Copyright 2013 [Gamajo Tech](https://gamajo.com)
