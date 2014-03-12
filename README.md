# User Registration Tutorial

For this tutorial, we'll create a registration workflow. It's a two-step process:

* ask the user to log in with his/her Facebook account
* ask him/her to fill in a form to complete the profile

## Prerequisites

- A hull.io application. [Create one](https://accounts.hullapp.io/) if you haven’t already.
- An authentication provider linked to your app, so users can log in. For this tutorial we will use Facebook but you can use any other supported by hull. [See our documentation](http://hull.io/docs/references/services) for more details.
- An HTTP server, to serve the files included in this repository. If you're not sure how to do it, check out [our guide](https://github.com/hull/minimhull/wiki/Setup-an-HTTP-server).

## Step 1 - Bootstraping your app

First, create an `index.html`. Add jQuery, and `hull.js` to your page. For the sake of this tutorial,
we will also use Twitter Bootstrap, though it is not mandatory.

```html
<link href="//netdna.bootstrapcdn.com/twitter-bootstrap/2.3.1/css/bootstrap-combined.min.css">
<script src="//ajax.googleapis.com/ajax/libs/jquery/2.0.0/jquery.min.js"></script>
<script src="//d3f5pyioow99x0.cloudfront.net/0.8/hull.js"></script>
```

Now initialize hull.

```html
<script>
Hull.init({
  appId: 'APPLICATION_ID',
  orgUrl: 'ORGANIZATION_URL'
});
</script>
```

`APPLICATION_ID` and `ORGANIZATION_URL` must be replaced with the correct values which you can find in your [admin](https://accounts.hullapp.io).

## Step 2 - Creating wrapper component

Depending on the user being logged in or not, we want to display a form (if logged in) or a login button (resp. if not logged in).

To do that we need to create a `wrapper` component. Insert the following code in your HTML document:

```html
  <!-- Javascript -->
  <script>
  Hull.component('wrapper', {
    templates: ['intro']
  });
  </script>
  
  <!-- Template -->
  <script type="text/x-template" data-hull-template="wrapper/intro">
    {{#if loggedIn}}
      <p>Hello {{me.name}} – <a href="#" data-hull-action="logout">Logout</a></p>
      <div data-hull-component="registration/form@hull"></div>
    {{else}}
      <p>Hello visitor</p>
      <div data-hull-component="login/button@hull"></div>
    {{/if}}
  </script>
```

Congratualations! You've just created your first component! Let's add it to our HTML document.

```html
<div data-hull-component="wrapper"></div>
```

Refresh your browser, you should see the sign in button.

## Step 3 - Updating the template when user logs in.

As you can see, clicking on the sign in button doesn't show the form. To fix this, we need to set a `refreshEvents` property
to refresh (re-render) the component when the user is updated (logged in/out, changed properties).

```html
<head>
  <!-- Code omitted on purpose -->

  <script>
  Hull.component('wrapper', {
    templates: ['intro'],
    refreshEvents: ['model.hull.me.change']
  });
  </script>

  <!-- Code omitted on purpose -->
</head>
```

Here we set `refreshEvents` property to `['model.hull.me.change']`. This is for the component to refresh itself every time the current user changes.

Now, clicking on the sign-in button should show a form that contains two fields (name and email). You probably want to know more about your user.

## Step 4 - Customizing the form

What about asking the user his/her website ?

To do that, we need the to instanciate the `admin/registration@hull` component that will allow us to define our form's fields.
It is a component dedicated to customizing the registration form in your hull.io app.
We will put this component in a new HTML document.

```html
<html>
  <head>
    <title>Hull Registration Admin</title>

    <link rel="stylesheet" href="//netdna.bootstrapcdn.com/twitter-bootstrap/2.3.1/css/bootstrap-combined.min.css">
    <script src="//ajax.googleapis.com/ajax/libs/jquery/2.0.0/jquery.min.js"></script>
    <script src="//hull-js.s3.amazonaws.com/0.8/hull.js"></script>

    <script>
      Hull.init({
        appId: 'APPLICATION_ID',
        orgUrl: 'ORGANIZATION_URL'
      });
    </script>
  </head>
  <body>
    <div data-hull-component="admin/registration@hull"></div>
  </body>
</html>
```

Replace `APPLICATION_ID`, and `ORGANIZATION_URL` with the correct values which you can find in your [admin](http://hullapp.io).

Open this new file in your browser and fill the textare with:

```json
[
  {
    "name": "name",
    "type": "text",
    "label": "Name",
    "placeholder": "Your name",
    "error": "Please enter your name",
    "required": true
  },
  {
    "name": "email",
    "type": "email",
    "label": "Email",
    "placeholder": "you@provider.com",
    "error": "Please enter a valid email adress",
    "required": true
  },
  {
    "name": "website",
    "type": "url",
    "label": "Website",
    "placeholder": "http://website.com",
    "error": "Please enter a valid URL"
  }
]
```

- `"name"`: the name of the field. this will be the key in the user profile.
- `"type"`: the type of the `<input>`. You can use HTML5 input type.
- `"label"`: the label of the field. It will be visible by the user.
- `"placeholder"`: the value of the input `placeholder` attribute.
- `"error"`: the error message that will be displayed if the field validation fails.
- `"required"`: boolean value that indicates whether the field is required or not.

Go back to your `index.html` you should see the website field.

Now that you know how to save information in the user profile, you probably want to list your users and their profile informations.

## Step 5 - Listing users

In your `admin.html` add the `users_admin` component.

```html
<body>
  <div data-hull-component="admin/registration@hull"></div>
  <div data-hull-component="admin/users_list@hull"></div>
</body>
```

Refresh your browser and you're done.

## Conclusion

What did we actually learn?

- Creating a component.
- Customizing a registration form.
- Saving informations in the user profile.
- Listing users.


For any question send us an email to contact@hull.io.
