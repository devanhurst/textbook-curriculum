
# Intro to Backbone Models

##Learning Goals

By the end of this lesson you should be able to:

- Explain what a Backbone Model is
- Create your own Backbone Model
- Instantiate your Backbone Model and assign attributes
- Use some basic Backbone Model methods

## Creating a Backbone Model

You create your own Model classes by extending the `Backbone.Model` class:

```javascript
var Person = Backbone.Model.extend( {

});
```

Then you can create your own instances of this `Person` model:

```javascript
var ada = new Person();
```

Note that we use a capital letter for our Backbone model to indicate that this is a Model object.

## Constructors & Initialize

Just like with Ruby, you can create constructors to initialize attributes and set up your Model.  

The `initialize` function runs, when a new instance is created. We specify the `initialize` as an object that we pass into the `extend` function.

```javascript
var Person = Backbone.Model.extend( {
  initialize: function() {
    console.log("A new person has been instantiated.");
	}
});

var ada = new Person();
```

So if the script above is run the console will result in:

```console
A new person has been instantiated.
```

The defaults property lets you set default values to **attributes** for your model.  You can then retrieve those attribute values with the `get` function. We can also provide other properties to the model by including them in the object we pass to `extend`.

```javascript
var Person = Backbone.Model.extend( {
  defaults: {
      name: "Ada",
      age: 21
  },
  initialize: function() {
    console.log("Person has been Created");
  }
});

var ada = new Person();
console.log("The person is named " + ada.get("name"));
```

Results in:
```console
Person has been Created
The person is named Ada
```

You can also pass in **attributes** to a model at instantiation via a JSON object in the parameters to new.

```javascript
var babbage = new Person({name: "Charles Babbage", age: 65, skills: "Hardware Design, Mathematics, Flower Arrangement."});
```

These attributes do not necessarily need to have default values specified, or really be specified in any way. In the example above, we've provided two of the fields that we decided to set defaults for, and added another, `skills`.

## Get, Set & Unset

Once you have an instance of your Model you can use `get` & `set` functions to set its attributes. For the `set` function you may pass two arguments, an attribute name and the attribute's value. Or you may pass an object containing multiple attributes to be set on the model.

Setting **each key/value**:
```javascript
ada.set("skills", "Programming, Mathematics, Mountain Climbing.");
ada.set("phone", "(444) 465-9122");
```

Setting using an **object**:
```javascript
ada.set({
  skills: "Programming, Mathematics, Mountain Climbing.",
  phone: "(444) 465-9122"
});
```

```javascript
console.log(ada.get("phone")); // '(444) 465 9122'
console.log(ada.get("skills")); // 'Programming, Mathematics, Mountain Climbing.'
```

You can use `unset` to remove an attribute.
```javascript
ada.unset("skills");

console.log(ada.get("skills")); // undefined
```

## Adding Additional Functions to a Model

You can add additional functions to a Model like other properties:

```javascript
var Person = Backbone.Model.extend( {
  defaults: {
      name: "Ada",
      age: 21
  },
  initialize: function() {
    console.log("Person has been Created");
  },
  sayHi: function() {
    console.log(this.get("name") + " says HI!");
  }
});

var myPerson = new Person();

myPerson.sayHi(); // 'Ada says HI!'
```

## Rendering a Model

So we've demonstrated how to create a Model, but we've not bound it to a view to be rendered in the browser.  We can look at how to do this now.

We can set up a new Model for a Todo List, to go with the View we created at the end of lesson 2.  

```javascript
TodoManager.Models.Todo = Backbone.Model.extend( {
  defaults: {
    title: "Something to do",
    description: "",
    completed: false
  }
});

```

Now in our view we want it to actually render the Backbone Model instead of a generic JavaScript Object.  So we will use the model and call it's `.toJSON()` function.  

```javascript
// app/views/todo.js
TodoManager.Views.Todo = Backbone.View.extend( {
  tagName: 'section',
  className: 'media no-bullet column',
  template: _.template($('#tpl-todo').html()),
  initialize:  function() {
    // put event listeners here!
  },
  render: function() {
  						// convert the model to JSON for the template
    this.$el.html(this.template(this.model.toJSON()));
    return this;
  }
});

```

And lastly to put them together in our `app/js/app.js` file.
```javascript
window.TodoManager = {
  Models: {},
  Collections: {},
  Views: {}

};
$(function() {
    // create a new Model object
  var myTodo = new TodoManager.Models.Todo({
      title: "Learn Backbone!",
      description: "Structure my code!",
      completed: false
  });
    // create a new View with the Model attribute set.
  var todoView = new TodoManager.Views.Todo({
    model: myTodo
    });
    $('#todocontainer').append(todoView.render().$el);
});
```

## Resources
- [Backbone Model & View Documentation](http://backbonejs.org/#Model-View-separation)
-  [An Intro to Backbone Models & Collections](http://liquidmedia.org/blog/2011/01/backbone-js-part-1/)
-  [Backbone Fundamentals, Models Chapter](https://addyosmani.com/backbone-fundamentals/#models-1)
