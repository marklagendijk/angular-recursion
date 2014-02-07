# angular-recursion-helper

A service which makes it easy possible to have recursive Angular directives.

## Why
When an Angular directive calls itself, Angular gets into an endless loop. This service provides the logic needed to work around this.

## Usage
The service is used by injecting it into your directive and using it in the directives' compile function. The following example shows this. See [this fiddle](http://jsfiddle.net/xUsCc/34/) to see this example running.

``` javascript
module.directive("tree", function(RecursionHelper) {
    return {
        restrict: "E",
        scope: {family: '='},
        template: 
        '<p>{{ family.name }}{{test }}</p>'+
            '<ul>' + 
                '<li ng-repeat="child in family.children">' + 
                    '<tree family="child"></tree>' +
                '</li>' +
            '</ul>',
        compile: function(element) {
            return RecursionHelper.compile(element, function(scope, iElement, iAttrs, controller, transcludeFn){
                // Define your normal link function here.
                // Alternative: instead of passing a function,
                // you can also pass an object with 
                // a 'pre'- and 'post'-link function.
            });
        }
    };
});
```