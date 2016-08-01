Templates are the site's markup, where images and js, css files are located as well as the site html structure. The default template is called Default.

A regular site would have multiple css files and a javascript folder containing js files, an image folder. The rest of the sites pages come from the views.

The typical elements of a page include the following. The title comes from an array with a key of the title followed by the constant SITETITLE, this lets the controllers set the page titles in an array that is passed to the template.

The template_url function is being used to get the full path to the CSS file.

## Routing Images / CSS / JS files
When files are above the document root resources must be placed inside the **Templates/Default/Assets** folder otherwise they won't be loaded correctly.

To load images the template_url function can be used, it accepts 2 params
1. The relative path of the asset
2. the theme to be used.

````php
<img src='<?php echo template_url('images/logo.jpg', 'Default');?>' alt=''>
```

Page example:

````php
<!DOCTYPE html>
<html lang="<?php echo LANGUAGE_CODE; ?>">
<head>
    <meta charset="utf-8">
    <title><?php echo $title.' - '.SITETITLE;?></title>
    <?php
    echo $meta;//place to pass data / plugable hook zone
    Assets::css([
        'https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css',
        template_url('css/style.css', 'Default')',
    ]);
    echo $css; //place to pass data / plugable hook zone
    ?>
</head>
<body>
<?php echo $afterBody; //place to pass data / plugable hook zone?>

<div class="container">
```

The default template comes with multiple files to demonstrate different use cases. 

default.php and custom.php are full page layouts whilst header.php and footer.php are partial layouts that can be mixed, the files ending with -rtl.php mean they are right to left formats.

A theme can be as simple as a single layout file and an assets folder and message.php file.