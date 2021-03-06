# Nova grid system for Laravel Nova

## Install

You can install this package via composer:

```
composer require codenco-dev/nova-grid-system
```

Then, you will need to register the tool within the NovaServiceProvider.php:


```
use CodencoDev\NovaGridSystem\NovaGridSystem;

/**
     * Get the tools that should be listed in the Nova sidebar.
     *
     * @return array
     */
    public function tools()
    {
        return [
        //other tools
            new NovaGridSystem
        ];
    }
```

## How to use?

This package allows you to define field sizes for detail, update or resource creation pages with tailwind CSS classes.
### Like I want
You can use the `size` method on your field to add a meta property for each page. All Tailwind fluid classes for size can be used.
See https://tailwindcss.com/docs/width/#fluid-width

### Where I want
If you don't want to change the size of fields on each page, you can use methods to filter the effect:

- `sizeOnCreating` 
- `sizeOnUpdating` 
- `sizeOnForms` 
- `sizeOnDetail` 

### Stacked or not stacked...
Automatically, with the default configuration, the field labels are stacked.
If you don't want this, you can use the `stacked` method with a parameter value of `false`.
As for previous point, you can do it wherever you want with:

- `stackedOnCreating` 
- `stackedOnUpdating` 
- `stackedOnForms` 
- `stackedOnDetail`

### Damn ! There is a bottom border
On the detail page, Nova automatically remove its lower border for the last field.
In this package, it's not possible to calculate the last field, so you can delete it yourself.
You can use these methods:

- `removeBottomBorderOnCreating` 
- `removeBottomBorderOnUpdating` 
- `removeBottomBorderOnForms` 
- `removeBottomBorderOnDetail`


### Example

This code uses the simplest method of configuration::

```
public function fields(Request $request)
{
    return [
        ID::make()->sortable(),

        Gravatar::make()->maxWidth(50),

        Text::make('Name')
            ->size('w-1/3')
            ->sortable()
            ->rules('required', 'max:255'),

        Text::make('Email')
            ->size('w-1/3')
            ->sortable()
            ->rules('required', 'email', 'max:254')
            ->creationRules('unique:users,email')
            ->updateRules('unique:users,email,{{resourceId}}'),
        Password::make('Password')
            ->size('w-1/3') // Only on forms, because we use onlyOnForms method with
            ->onlyOnForms()
            ->creationRules('required', 'string', 'min:8')
            ->updateRules('nullable', 'string', 'min:8'),
    ];
}
```

In this previous example, sizes are ok in forms pages but not in detail pages.
This code presents several configurations depending on the context and uses different methods

```
/**
     * Get the fields displayed by the resource.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return array
     */
    public function fields(Request $request)
    {
        return [
            ID::make()->sortable(),

            Gravatar::make()->maxWidth(50),

            Text::make('Name')

                ->sizeOnDetail('w-1/2')
                ->stackedOnDetail(false)

                ->sizeOnForms('w-1/3')

                ->sortable()
                ->rules('required', 'max:255'),

            Text::make('Email')

                ->sizeOnDetail('w-1/2')
                ->stackedOnDetail(false)

                ->sizeOnForms('w-1/3')

                ->sortable()
                ->rules('required', 'email', 'max:254')
                ->creationRules('unique:users,email')
                ->updateRules('unique:users,email,{{resourceId}}'),
            Password::make('Password')
                ->size('w-1/3') // Only on forms, because we use onlyOnForms method with
                ->onlyOnForms()
                ->creationRules('required', 'string', 'min:8')
                ->updateRules('nullable', 'string', 'min:8'),

            Date::make('Created At')
                ->hideWhenCreating()
                ->size('w-1/2')
                ->stacked(false)
                ->stackedOnForms(true)
                ->removeBottomBorderOnForms(),
            Date::make('Updated At')
                ->hideWhenCreating()
                ->size('w-1/2')
                ->stacked(false)
                ->stackedOnForms(true)
                ->removeBottomBorderOnForms(),
            Badge::make('Status 1', function () {
                return 'info';
            })->size('w-1/6')->removeBottomBorder(),
            Badge::make('Status 2', function () {
                return 'success';
            })->size('w-1/6')->removeBottomBorder(),
            Badge::make('Status 3', function () {
                return 'warning';
            })->size('w-1/6')->removeBottomBorder(),
            Badge::make('Status 4', function () {
                return 'info';
            })->size('w-1/6')->removeBottomBorder(),
            Badge::make('Status 5', function () {
                return 'success';
            })->size('w-1/6')->removeBottomBorder(),
            Badge::make('Status 6', function () {
                return 'warning';
            })->size('w-1/6')->removeBottomBorder(),


        ];
    }
```


### Configuration

You can disable each "automatic" feature for each type of page with the `nova-grid-system.php` config file.
To publish it, you can use this command:

```
php artisan vendor:publish --tag="nova-grid-system"
```  


## Screenshots

![Creating form](https://user-images.githubusercontent.com/17202290/82239889-ca091580-9939-11ea-9b45-aeff0e26db4a.png)

![Updating form](https://user-images.githubusercontent.com/17202290/82239901-cbd2d900-9939-11ea-8c2e-60d31f1daa33.png)

![Detail page](https://user-images.githubusercontent.com/17202290/82239906-cd9c9c80-9939-11ea-9281-18293f8e7447.png)

## License

The MIT License (MIT). Please see License File for more information.

## About
This package is inspired by a deleted package by Eduardo Geschonke https://github.com/jobcerto