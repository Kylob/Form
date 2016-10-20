# use BootPress\Form\Component as Form;

[![Packagist][badge-version]][link-packagist]
[![License MIT][badge-license]](LICENSE.md)
[![HHVM Tested][badge-hhvm]][link-travis]
[![PHP 7 Supported][badge-php]][link-travis]
[![Build Status][badge-travis]][link-travis]
[![Code Climate][badge-code-climate]][link-code-climate]
[![Test Coverage][badge-coverage]][link-coverage]

Coordinates form validation, errors, messages, values, and inputs in a [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) [KISS](http://en.wikipedia.org/wiki/Keep_it_simple_stupid) way.

## Installation

Add the following to your ``composer.json`` file.

``` bash
{
    "require": {
        "bootpress/form": "^1.0"
    }
}
```

## Example Usage

```php
use BootPress\Form\Component as Form;

$form = new Form('form', 'post');

// Create some menus
$form->menu('gender', array(
    'M' => 'Male',
    'F' => 'Female',
));

$form->menu('remember', array('Y' => 'Remember Me'));

// Set the default values
$form->set('values', array(
    'name' => 'Daniel',
    'email' => 'me@example.com',
    'gender' => 'M',
));
```

Now the form's menus and default values have been set up, and you have a ``$form->validator`` object filled with ``$_POST`` vars, ready to go.  You don't have to use the [BootPress Validator Component](https://packagist.org/packages/bootpress/validator), but it sure makes things easier for you.

```php
$form->validator->set(array(
    'name' => 'required',
    'email' => 'required|email',
    'gender' => 'required|inList',
    'password' => 'required|minLength[5]|noWhiteSpace',
    'confirm' => 'required|matches[password]',
    'feedback' => 'maxWords[2]',
    'remember' => 'yesNo',
));

if ($vars = $form->validator->certified()) {
    echo '<pre>'.print_r($vars, true).'</pre>';
    // $form->eject();
}
```

When you create a ``$form->menu()``, we automatically pass it's values to the validator so that you can ``$form->validator->set('field', 'inList')`` with no params, and still be covered.  That's why we didn't put '**inList[M,F]**' for your gender above.  To create the form:

```php
echo $form->header();
echo $form->fieldset('Form', array(
    $form->text('name', array('class' => 'form-control')),
    $form->text('email', array('placeholder' => 'Email Address')),
    $form->radio('gender'),
    $form->password('password'),
    $form->password('confirm'),
    $form->textarea('feedback'),
    $form->checkbox('remember'),
    $form->input('submit', array('name' => 'Submit')),
));
echo $form->close();
```

That would give you the following HTML:

```html
<form name="form" method="post" action="http://example.com?submitted=form" accept-charset="utf-8" autocomplete="off">

    <fieldset><legend>Form</legend>
    
        <input type="text" class="form-control" name="name" id="nameI" value="Daniel" data-rule-required="true">
        
        <input type="text" placeholder="Email Address" name="email" id="emailII" value="me@example.com" data-rule-required="true" data-rule-email="true">
        
        <div class="radio"><label><input type="radio" name="gender" value="M" checked="checked" data-rule-required="true" data-rule-inList="M,F"> Male</label></div>
        <div class="radio"><label><input type="radio" name="gender" value="F"> Female</label></div>
        
        <input type="password" name="password" id="passwordIV" data-rule-required="true" data-rule-minlength="5" data-rule-nowhitespace="true">
        
        <input type="password" name="confirm" id="confirmV" data-rule-required="true">
        
        <textarea name="feedback" id="feedbackVI" cols="40" rows="10" data-rule-maxWords="2"></textarea>
        
        <div class="checkbox"><label><input type="checkbox" name="remember" value="Y"> Remember Me</label></div>
        
        <input type="submit" name="Submit">
        
    </fieldset>
    
</form>
```

You may want to put some labels and error messages in there, but this Form component is meant to be a bare-bones, just-get-the-hard-stuff-done first, so that you can style it anyway you like.

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

[badge-version]: https://img.shields.io/packagist/v/bootpress/form.svg?style=flat-square&label=Packagist
[badge-license]: https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square
[badge-hhvm]: https://img.shields.io/badge/HHVM-Tested-8892bf.svg?style=flat-square
[badge-php]: https://img.shields.io/badge/PHP%207-Supported-8892bf.svg?style=flat-square
[badge-travis]: https://img.shields.io/travis/Kylob/Form/master.svg?style=flat-square
[badge-code-climate]: https://img.shields.io/codeclimate/github/Kylob/Form.svg?style=flat-square
[badge-coverage]: https://img.shields.io/codeclimate/coverage/github/Kylob/Form.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/bootpress/form
[link-travis]: https://travis-ci.org/Kylob/Form
[link-code-climate]: https://codeclimate.com/github/Kylob/Form
[link-coverage]: https://codeclimate.com/github/Kylob/Form/coverage
