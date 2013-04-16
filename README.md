## Backbone.VirtualCollection

<a href="http://teambox.com"><img alt="Built at Teambox" src="http://i.imgur.com/hqNPlHe.png"/></a>

![Build Status](https://api.travis-ci.org/p3drosola/Backbone.VirtualCollection.png)


Backbone.VirtualCollection allows you use Backbone.Marionette CollectionViews and CompositeViews on a subset of
a backbone collection.

### Usage

For example, let's say you have a task collection, and want to show a list of tasks that belong to a specific user.

We can instanciate a virtual collection, that only contains tasks that belong to Rupert (who has user_id 13).
The contructor takes two parameters, the first is the parent collection, the second is a filter function that takes the model as argument. You can also just specify a hash of attributes to match.

```js
var virtual_collection = new Backbone.VirtualCollection(tasks_collection, function (task) {
  return task.get('user_id') == 13;
});
// or using a hash of attributes to match
virtual_collection = new Backbone.VirtualCollection(tasks_collection, {user_id: 13});

var view = new TaskListView({
  collection: virtual_collection
});

// note: it's important to call
// this.collection.unbindIndexListeners();
// in the onClose of the view. It clears up the listeners used to update the virtual collection
```

The marionette collection view will only display the tasks that belong to Rupert, and it will update automatically. In other words, when a task is created that belongs to Rupert it will appear, but not if it belongs to Bob.

### How does it work?

#### It's really light
VirtualCollection **only** implements the methods used by a Marionette CollectionView to render a collection. It does not attempt to mimic all the behaviours of an actual collection. This keeps the overhead down.

#### It's DRY
VirtualCollection does not store, or duplicate any data. We've used other solutions in the past, and duplicating data is just plain bad news.

#### It's Fast
VirtualCollection builds an internal index of model ids that pass the filter. That way interating with the `each` interator is fast.  It doesn't have to go through the whole parent collection and re-evaluate the all the filters.


> By the way, `VirtualCollection.buildFilterFromHash` is the function that turns a object into a filter function.You might find it usefull.

Happy hacking!





### License
The MIT License (MIT)

Copyright (c) 2013 Pedro  p3dro.sola@gmail.com

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
