# WordPress Plugin Template Loader
A class that allows loading WordPress template parts in plugins. Based on the [Gamajo Template Loader](https://github.com/GaryJones/Gamajo-Template-Loader), adapted to PSR-2 and PSR-4 coding standards.

## Description

This class builds upon the `get_template_part()` function in WordPress, but allows you to load template parts from your plugin, with built in fallbacks.

This fork uses modern PHP coding standards such as PSR-2 code formatting, PSR-4 autoloading, and namespacing, which eliminates the need to make a copy of the class, or to use Mozart to prefix the class name.

## Installation

This isn't a WordPress plugin on its own, it must be loaded as a Composer dependency.
`composer require gamajo/template-loader`

## Instantiate class
To instantiate the template loader class, first add a `use` directive to set the namespace of this class, and then assign an instance of the class to a variable:

  ~~~php
  <?php
  namespace RexRana\FitnessPro;

  // Template loader instantiated elsewhere, such as the main plugin file.
  $meal_planner_template_loader = new Meal_Planner_Template_Loader;
  ~~~

* Use it to call the `get_template_part()` method. This could be within a shortcode callback, or something you want theme developers to include in their files.

  ~~~php
  $meal_planner_template_loader->get_template_part( 'recipe' );
  ~~~
* If you want to pass data to the template, call the `set_template_data()` method with an array before calling `get_template_part()`. `set_template_data()` returns the loader object to allow for method chaining.

  ~~~php
  $data = array( 'foo' => 'bar', 'baz' => 'boom' );
  $meal_planner_template_loader
      ->set_template_data( $data );
      ->get_template_part( 'recipe' );
  ~~~
  
  The value of `bar` is now available inside the recipe template as `$data->foo`.
  
  If you wish to use a different variable name, add a second parameter to `set_template_data()`:

  ~~~php
  $data = array( 'foo' => 'bar', 'baz' => 'boom' );
  $meal_planner_template_loader
      ->set_template_data( $data, 'context' )
      ->get_template_part( 'recipe', 'ingredients' );
  ~~~
  
  The value of `bar` is now available inside the recipe template as `$context->foo`.

  This will try to load up `wp-content/themes/my-theme/meal-planner/recipe-ingredients.php`, or `wp-content/themes/my-theme/meal-planner/recipe.php`, then fallback to `wp-content/plugins/meal-planner/templates/recipe-ingredients.php` or `wp-content/plugins/meal-planner/templates/recipe.php`.

## Usage

When you call the `GetTemplatePart` method of this class, it looks for the template name in the following order:

- child theme
- parent theme
- the template folder for the plugin. 

## Change Log

See the [change log](CHANGELOG.md).

## License

[GPL-2.0+](LICENSE).

## Contributions

Contributions are welcome - fork, fix and send pull requests against the `develop` branch please.

## Credits

Original class `Gamajo Template Loader` Built by [Gary Jones](https://twitter.com/GaryJ)  
Copyright 2013 [Gamajo Tech](https://gamajo.com)
