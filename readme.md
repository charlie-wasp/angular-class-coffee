Angular Class
=============

Coffeescript classes support for angular. Makes possible to use coffescript
classes instead of callbacks in angular.


Features
--------

  * Classes are simpler than large callbacks
  * Controllers/resources can be inherited; it is usefull for deduplication
  * It is easier to isolate classes and to do unit-testing


Installation
------------

`npm install angular-class-coffee --save`


Usage
-----

Basic usage:

```coffeescript
# create class
class MyApp.MyCtrl extends IdFly.AngularClass

  # add dependecies to @_import
  @_import = [
    '$scope',
  ]

  # create factory callback and inject it to angular
  app.controller('MyCtrl', @factory())

  # if you want plain class instead of instance in angular it also possible by
  # calling `classFactory`:
  #
  # app.factory('MyResource', @classFactory())

  # add `initialize` method that will be called right after dependecies will
  # be injected to object:
  initialize: () ->
    # all dependecies are available as instance variables that starts with
    # underscore
    @_scope.hello = 'HELLO WORLD'
```

Inheritance:

```coffeescript
# inherit class from your own class
class MyApp.MyChildCtrl extends MyApp.MyCtrl

  # note that you should append dependencies
  @_import = _.extend({}, MyApp.MyCtrl._import, [
    '$http',
  ])

  app.controller('MyChildCtrl', @factory())

  # add `initialize` method that will be called right after dependecies will
  # be injected to object:
  initialize: () ->
    super()
    $http.get('/status', @_setStatus)

  _setStatus: (response) =>
    @_scope.status = response.result
```

Directive:

```coffeescript
# create normal class
class MyApp.MyDirective extends IdFly.AngularClass

  # import dependecies as normal ones
  @_import: [
    '$scope',
    '$http',
  ]

  # set template url and scope to class
  @templateUrl: '/link/to/template.html'
  @scope: {
    data: '=data',
  }

  # use @directive to specify the directive
  app.directive('myDirective', @directive())

  # use link to setup stuff on element
  link: (element) ->
    # link element here
```


Example
-------

```coffeescript
app.controller(($scope, $http) ->
  $http
    .get('/api/status')
    .then((response) ->
      $scope.status = response.result
    )
)
```

Becomes:

```coffeescript
class MyApp.StatusCtrl extends IdFly.AngularClass

  @_import: [
    '$scope',
    '$http',
  ]

  myApp.controller('StatusCtrl', @factory())

  initialize: () ->
    @_http.get('/api/status').then(@_setStatus)

  _setStatus: (response) =>
    @_scope.status = response.result
```


License
-------

The MIT License (MIT)


Authors
-------

[Leonid Shagabutdinov](http://github.com/shagabutdinov)
