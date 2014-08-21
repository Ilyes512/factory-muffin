---
layout: layout
title: Usage - Factory Definitions
permalink: usage/definitions/
---

# Usage - Factory Definitions

You can define model factories using the `define` function. You may call it like this: `League\FactoryMuffin\Facade::define('Fully\Qualifed\ModelName', array('foo' => 'bar'))`, where `foo` is the name of the attribute you want set on your model, and `bar` describes how you wish to generate the attribute. Please see the generators section for more information on how this works.

You may optionally specify a callback to be executed on model creation/instantiation as a third parameter. We will pass your model instance as the first parameter to the closure if you specify one. We additionally pass a boolean as the second parameter that will be `true` if the model is being persisted to the database (the create function has used), and `false` if it's not being persisted (the instance function was used). We're using the `isPendingOrSaved` function under the hood here. Note that if you specify a callback and use the create function, we will try to save your model to the database both before and after we execute the callback.

You can also define multiple different factory definitions for your models. You can do this by prefixing the model class name with your "group" followed by a colon. This results in you defining your model like this: like this: `League\FactoryMuffin\Facade::define('myGroup:Fully\Qualifed\ModelName', array('foo' => 'bar'))`. You don't have to entirely define your model here because we will first look for a definition without the group prefix, then apply your group definition on top of that definition, overriding attribute definitions where required.

We have provided a nifty way for you to do this in your tests. PHPUnit provides a `setupBeforeClass` function. Within that function you can call `League\FactoryMuffin\Facade::loadFactories(__DIR__ . '/factories');`, and it will include all files in the factories folder. Within those php files, you can put your definitions (all your code that calls the define function). The `loadFactories` function will throw a `League\FactoryMuffin\Exceptions\DirectoryNotFoundException` exception if the directory you're loading is not found.