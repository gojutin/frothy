# ![Logo](frothy_icon.png) Frothy

## A customizable, drop-in login form for Firebase and React.

[![npm version](https://badge.fury.io/js/frothy.svg)](https://badge.fury.io/js/frothy)
[![Open Source Love](https://badges.frapsoft.com/os/mit/mit.svg?v=102)](https://github.com/ellerbrock/open-source-badge/)

### [Interactive Demo](https://frothy-123.firebaseapp.com/)

#### Frothy supports the following Firebase authentication features:

* Login with Email/Password
* Sign Up with Email/Password (with Recaptcha)
* Google Login
* Facebook Login
* Twitter Login
* GitHub Login
* Login with Phone Number (with Recaptcha)
* Anonymous Login
* Send Password Reset Link

Frothy is built with [React](https://reactjs.org/), the [Firebase SDK](https://firebase.google.com/docs/database/web/start) and [Styled-Components](https://www.styled-components.com/).

## Getting Started

Disclaimer: This is a new project and has not been thoroughly tested. At the moment, it is not recommended to use this library in production. If you'd like to help get it there, your contributions would be greatly appreciated!

In order to use this component, you will need to have the `firebase` and `styled-components` npm packages installed.

`npm i firebase styled-components` or `yarn add firebase styled-components`

You will also need to have Firebase configured for your React project. This process is simple (and free). Here is a rough overview of the process:

In Firebase:

* Create a (free) Firebase project at [https://firebase.google.com/](https://firebase.google.com/).

- In the Firebase console, go to `Authentication > SIGN-IN METHOD` and enable all of the sign-in providers that you are interested in supporting. Firebase will walk you through the steps for each one.

In your React project:

Add your Firebase config to your main `index.js` file. You can find your config in the Firebase console at `Authentication > WEB SETUP`

```
import firebase from 'firebase';

const prodConfig = {
  // apiKey: YOUR_API_KEY,
  // authDomain: YOUR_AUTH_DOMAIN,
  // databaseURL: YOUR_DATABASE_URL,
  // projectId: YOUR_PROJECT_ID,
  // storageBucket: '',
  // messagingSenderId: YOUR_MESSAGING_SENDER_ID,
};

firebase.initializeApp(config);
```

Once Firebase is wired up, just import Frothy and drop it anywhere you'd like.

`npm i frothy` or `yarn add frothy`

```js
// import it
import Frothy from 'frothy';

// use it
<Frothy />;
```

Prefer a modal?

```js
<Frothy modal />
```

## Accessing User and Authentication State

Firebase offers a couple convenient methods to access the user and authentication state:

Authentication State Observer (recommended)

```js
import firebase from 'firebase';

firebase.auth().onAuthStateChanged(function(user) {
  if (user) {
    // User is signed in.
  } else {
    // No user is signed in.
  }
});
```

`currentUser` property

```js
import firebase from 'firebase';

const user = firebase.auth().currentUser;

if (user) {
  // User is signed in.
} else {
  // No user is signed in.
}
```

The Frothy [demo](https://frothy-123.firebaseapp.com/) utilizes the Authentication State Observer to control access to routes based on authentication status. Feel free to dig into the demo's source code to see how this works.

## Props

Out of the box, a Frothy instance includes all of the supported Firebase features outlined above. However, a Frothy instance can be customized to include only the features that you want to support via props. The component will automatically reformat it's style based on the features that you have chosen to implement. A few additional styling props are also exposed for further customization.

Here is a Frothy instance with all of the available props set to their default values. In other words, the examples below is the same as just using `<Frothy />`.

```js
import { Button } from './common';

<Frothy
  // Modal
  modal={false} // If true, the login form becomes a modal and provides a button to toggle
  modalButton={<Button>Sign In</Button>} // Accepts a component instance
  // General Styling
  themeColor="#2196F3" // Sets the color of the tabs and buttons.
  showBorder={true} // Shows or hide the box border
  rounded={1} // 0 to 5 (not rounded to most rounded)
  shadow={2} // 0 to 5 (no shadow to thickest shadow)
  showLabels={true} // Displays the provider name under the social icon buttons
  // Header Styles
  title={'Welcome to Frothy!'} // Accepts a component instance or a string
  titleColor="#424242"
  titleBackgroundColor="#ededed"
  // Auth Props
  emailLogin={true} // Enable email login
  emailRemember={true} // Enable "Remember me" checkbox (localStorage)
  emailSignup={true} // Enable email signup
  passwordReset={true} // Enable password reset
  phone={true} // Enable phone login
  recaptcha={'invisible'} // invisible or normal (phone login only)
  anonymous={true} // Enable anonymous login
  google={true} // Enable Google login
  facebook={true} // Enable Facebook login
  twitter={true} // Enable Twitter login
  github={true} // Enable GitHub login
/>;
```

Here is the full list of the `propTypes` and `defaultProps` that are available:

```js
Frothy.propTypes = {
  // Modal
  modal: PropTypes.bool,
  modalButton: PropTypes.element,

  // General Styling
  themeColor: PropTypes.string,
  showBorder: PropTypes.bool,
  rounded: PropTypes.oneOf([0, 1, 2, 3, 4, 5]),
  shadow: PropTypes.oneOf([0, 1, 2, 3, 4, 5]),
  showLabels: PropTypes.bool,

  // Header Styling
  title: PropTypes.oneOfType([PropTypes.string, PropTypes.element]),
  titleColor: PropTypes.string,
  titleBackgroundColor: PropTypes.string,

  // Auth
  emailLogin: PropTypes.bool,
  emailRemember: PropTypes.bool,
  emailSignup: PropTypes.bool,
  passwordReset: PropTypes.bool,
  anonymous: PropTypes.bool,
  phone: PropTypes.bool,
  recaptcha: PropTypes.oneOf(['normal', 'invisible']),
  google: PropTypes.bool,
  facebook: PropTypes.bool,
  twitter: PropTypes.bool,
  github: PropTypes.bool,

  // Pass-throughs
  style: PropTypes.object,
  className: PropTypes.string,
};

Frothy.defaultProps = {
  // Modal
  modal: false,
  modalButton: <ModalButton />,

  // General Styling
  themeColor: '#2196F3',
  showBorder: true,
  rounded: 1,
  shadow: 3,
  showLabels: false,

  // Header Styles
  title: 'Welcome to Frothy!',
  titleColor: '#424242',
  titleBackgroundColor: '#ededed',

  // Auth
  emailLogin: true,
  emailRemember: true,
  emailSignup: true,
  passwordReset: true,
  phone: true,
  recaptcha: 'invisible',
  anonymous: true,
  google: true,
  facebook: true,
  twitter: true,
  github: true,
};
```

## Design Decisions

This library aims to strike a nice balance of configuration options and simplicity.

After initially writing all of the styles with inline JavaScript, it was decided to refactor the project to use Styled-Components. The Styled-Components package is not included as a dependency due to the conflicts associated with having two installations in the same project. Instead, it is a peer dependency that will need to be imported. Styled-Components includes vendor prefixing, which allows this package to be supported by more browsers. It also injects the styles directly into the document, which means that you do not need to requre any additional CSS files.

## What's Next?

* Tests
* Token management?
* Recaptcha support for signup form
* Custom error config

If you would like to contribute, that would be awesome!

Here is how to get started on your own development server.

* `git clone git@github.com:gojutin/frothy.git`
* `cd frothy`
* `npm i`
* `npm run dev`
* visit `localhost:8000` in your browser
* `npm run docs:prod` runs a production build

![Screenshot](screenshot.png)

Made with :green_heart: by a vegan