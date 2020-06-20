# Command Line Interface

X Framework comes with a CLI which can help you build your 
app files. To run CLI, use the following command in the
root of your project.

> php x

You will be presented with a menu to handle the most used
tasks. Here are the current commands you can use.

- **Controller**

It creates an empty controller in App/Controllers directory.
They are used to govern a request to its appropriate response.

- **Resource**

It creates a simple CRUD controller in App/Controllers 
directory which is linked with a model. A resource is just a
controller which contains 7 methods used for creating a
CRUD controller.

- **Model**

It creates a model in App/Models directory. A model is an
abstraction for your database table. It allows you to
talk with the database.

- **Event**

It creates an event in App/Events directory. Events can be
emitted in your application when something worthy happens
An event is called when the App mounts and whenever a
Validation Error happens.

- **Listener**

It creates an event listener in App/Listeners directory. An
event can call multiple listeners.

- **Policy**

It creates an model policy in App/Policies directory.
Policies authorize Users on how they can manipulate a
model.

- **Unit Tests**

Finally, PHPUnit is official unit testing library, and
you can finally write unit tests.
