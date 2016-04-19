---
layout: post
title: "Meteor-React Tutorial Overview"
comments: True
---

# 1. Project Creation

Creating the project is pretty much the same. However, you do have to
add the react package:

```
meteor add react
```

And you'll probably be using a JSX file, so create an app-specific JSX
file to work with:

```
touch app.jsx
```

# 2. Place starter code in HTML

You'll need something like this HTML file so React has something to render to:

```html
<head>
  <title>Todo List</title>
</head>

<body>
  <div id="render-target"></div>
</body>
```
# 3. Define React Components in the JSX file

Using `React.createClass`, make your components. Make sure that each of
them has a `render()` function inside.

[Maybe put an example here: talk about having an App component that
represents the whole app, and then also making the sub components as
well]

And finally, don't forget to wrap it all up by having something like the
following:

```
if (Meteor.isClient) {
  // This code is executed on the client only

  Meteor.startup(function () {
    // Use Meteor.startup to render the component after the page is ready
    React.render(<App />, document.getElementById("render-target"));
  });
}
```

# 4. Replace Static Data w/ Collection Data

So first we create our collection, and maybe fill it with some dummy
data. And then to get at that data, we need to do three things:

1. Use this mixin inside our app component: `mixins: [ReactMeteorData]`.
   Make sure to put it near the top of your app component.

2. Use a `getMeteorData()` function (that only works if you did step 1
   above) to essentially fetch the data from the collection itself:

   ```js
   getMeteorData() {
     return {
       tasks: Tasks.find({}).fetch()
     }
   },
   ```

   So this method will return an array of the data we want.

3. Now update your `renderTasks()` method so that it pulls the data from
   `this.data.tasks` instead of `this.getTasks()`.

# 5. Letting the User Input Data: Adding Tasks w/ a Form

Well first, you have to add the form there. And when you do this, you
have to keep two things in mind:

1. We're going to have to reference this input field, so let's add an
   attribute `ref="textInput"`.

2. When someone hits enter on the form and submits this text, we have to handle that
   event. So let's add another attribute `onSubmit="this.handleSubmit"`.

Great, now let's finish up by writing that event handler write above
this `render()` block of code:

```js
  handleSubmit(event) {
    event.preventDefault();

    // Find the text field via the React ref
    var text = React.findDOMNode(this.refs.textInput).value.trim();

    Tasks.insert({
      text: text,
      createdAt: new Date()
    });

    // Clear form
    React.findDOMNode(this.refs.textInput).value = "";
  },
```

Note: You can also sort the tasks by changing the line inside
`getMeterData()` to:

```js
tasks: Tasks.find({}, {sort: {createdAt: -1}}).fetch()
```

# 6. Updating and Deleting The Data: Checking Off and Deleting Tasks

First let's add a checkbox and delete button in the "html" which is actually just
JSX.

```html
<button className="delete" onClick={this.deleteThisTask}>
  &times;
</button>

<input
  type="checkbox"
  readOnly={true}
  checked={this.props.task.checked}
  onClick={this.toggleChecked} />
```

Since the whole thing is wrapped in an `<li>` tag anyway, and we might
want to give it a special style when checked, we're going to give it a class:

```html
<li className={taskClassName}>

...

</li>
```

You'll also have to add this in the render() method, but above the
return statement:

```js
const taskClassName = this.props.task.checked ? "checked" : "";
```

At this point, we can write the event handlers for the checkbox and
delete buttons:

```js
toggleChecked() {
  // Set the checked property to the opposite of its current value
  Tasks.update(this.props.task._id, {
    $set: {checked: ! this.props.task.checked}
  });
},
deleteThisTask() {
  Tasks.remove(this.props.task._id);
},
```

# 7. Storing Temp. UI Data in Component State: Filtering Checked Tasks

First add the UI switch that allows the user to control the UI state:

```html
<label className="hide-completed">
  <input
    type="checkbox"
    readOnly={true}
    checked={this.state.hideCompleted}
    onClick={this.toggleHideCompleted} />
  Hide Completed Tasks
</label>
```

Then add an initial state at the top of your component (but under the
mixin declaration):

```js
getInitialState() {
  return {
    hideCompleted: false
  }
},
```

Finally, alter how you read or retrieve the data from your collection:

```js
getMeteorData() {
  let query = {};

  if (this.state.hideCompleted) {
    // If hide completed is checked, filter tasks
    query = {checked: {$ne: true}};
  }

  return {
    tasks: Tasks.find(query, {sort: {createdAt: -1}}).fetch()
  };
},
```

## Extra Feature: Displaying Number of Incomplete Tasks

Let's also display the number of incomplete tasks. First let's add
another line to the `getMeteorData()` method:

```js
return {
  tasks: Tasks.find(query, {sort: {createdAt: -1}}).fetch(),
  incompleteCount: Tasks.find({checked: {$ne: true}}).count()
};
```

Now you can just refer to the variable like this:

```html
<h1>Todo List ({this.data.incompleteCount})</h1>
```

# 8. Adding User Accounts and Wrapping a Blaze Component

This is what needed to be added in our JSX file:

```js
AccountsUIWrapper = React.createClass({
  componentDidMount(){
    Blaze.render(Template.loginButtons, React.findDOMNode(this.refs.container));
  },
  render(){
    return <span ref="container" />
  }
});
```

Two things to note here:

1. We are using Blaze to render our `loginButtons` template. So if
   you're doing this for some other app, just make sure to refer to the
template name there.

2. We are rendering a placeholder element called container. This is
   simply something that React needs to have in order to render the
template into a wrapping component.

Also, don't forget to include this component back into your main App.

# 9. Adding User-Related Functionality

We want to only show the input form when there is a user logged in. To
do that, we first need to make this component aware of the current user.
So let's add the following to our `getMeteorData()` method:

```js
currentUser: Meteor.user()
```

Then we add this conditional statement to our render method:

```js
{ this.data.currentUser ?
  <form className="new-task" onSubmit={this.handleSubmit} >
    <input
      type="text"
      ref="textInput"
      placeholder="Type to add new tasks" />
  </form> : ''
}
```

