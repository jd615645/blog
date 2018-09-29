---
title: 在Laravel 5.6 Auth 使用 reCAPTCHA
date: 2018-09-29 20:30:00
tags: 
- Laravel
categories:
- BackEnd
---
使用 [anhskohbo/no-captcha](https://github.com/anhskohbo/no-captcha)
## Installation
```
composer require anhskohbo/no-captcha
```

## Setup
在 `app/config/app.php` 新增

- 在 `providers` 新增
```
Anhskohbo\NoCaptcha\NoCaptchaServiceProvider::class,
```
- 在 `aliases` 新增
```
'NoCaptcha' => Anhskohbo\NoCaptcha\Facades\NoCaptcha::class,
```
- Publish the config file
```
php artisan vendor:publish --provider="Anhskohbo\NoCaptcha\NoCaptchaServiceProvider"
```

## Configuration
新增 `NOCAPTCHA_SECRET` and `NOCAPTCHA_SITEKEY` 在 `.env`
```
NOCAPTCHA_SECRET=secret-key
NOCAPTCHA_SITEKEY=site-key
```
記得在 [Google recaptcha](https://www.google.com/recaptcha/admin) 註冊

## 使用
- 在 `login.blade.php` 添加
```html
<div class="g-recaptcha" data-sitekey="{{ config('captcha.sitekey') }}"></div>
```

- 在 `Http/Controllers/Auth/LoginController.php` 新增
```php
use Illuminate\Http\Request;
```
以及
```php
/**
* Validate the user login request.
*
* @param  \Illuminate\Http\Request  $request
* @return void
*/
protected function validateLogin(Request $request)
{
  $this->validate($request, [
      $this->username() => 'required',
      'password' => 'required',
      'g-recaptcha-response' => 'required|captcha',
  ]);
}
```

- 若要自訂 `Validation` 訊息可在 `resources/lang/en/validation.php` 新增
```php
'custom' => [
    'g-recaptcha-response' => [
        'required' => 'Please verify that you are not a robot.',
        'captcha' => 'Captcha error! try again later or contact site admin.',
    ],
],
```

- captcha error 訊息可用以下方式來顯示
```
@if ($errors->has('g-recaptcha-response'))
    <span class="help-block">
        <strong>{{ $errors->first('g-recaptcha-response') }}</strong>
    </span>
@endif
```