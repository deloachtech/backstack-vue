# Backstack Demo (ALPHA)

> IMPORTANT NOTE! This project is not yet ready for production use.

This project contains comprehensive examples demonstrating how to effectively implement the [Backstack API](https://backstack.com) into your codebase. It's available under the MIT license for your use. Use this codebase to kickstart your app development.

Built with:

- Vite 5
- Vue 3
- Vue Router 4
- Pinia 2
- Axios 2
- Bootstrap 5

## Live version

A live version of the repository is available at [https://demo.backstack.com/](https://demo.backstack.com). Log in using `demo` as the username and password.

## Docs

Project documentation can be found at [backstack.com/demo](https://backstack.com/docs/demo).

## API Access

The project accesses the `api.backstack.com` endpoints using `demo` as the application key. The demo is interactive, allowing you to submit changes and observe the effects on the associated pages. It resets to the default state with each new login. 

Log in with `demo` as the username and password. 

When you're ready to go live. All you have to do is update your app key and you're good to go.


## Features

Includes common functionality.

- Log-in
- Reset password
- Sign-up
- Account invoicing with Stripe integration
- Feature-based access control (RBAC)
- User settings
- Account settings
- App versioning (e.g., Basic, Advanced, Premium)
- In-app module purchases (e.g., additional users, more widgets)
- Account networking with fee sharing
- App UI tips with hide/un-hide functionality
- App system alerts
- User configurable notification preferences (e.g. email, text, app)

 Add your app-specific views using the project components (e.g., tables, settings, file uploads) or your own.

## Installation

Download and unpack the contents of the zip file into your project directory. From there, install the `package.json` assets and run the development server.

```sh
npm install
npm run dev
```

Modify the project files as needed for your application.

## Updates

This project does not offer typical updating schemas as it's intended to be a starter project that's fully managed by the user.

# Customization

[Vite](https://vitejs.dev/) is used to compile the `src/template/styles.css` and the `src/template/theme.css`. Modify either of these files to meet your application theming requirements.

Everything controlling the structure (e.g. layout, navigation) is located in `src/template` for your convenience.

You can directly edit the components included in the `src/components` directory.


# Usage

We'll be creating a documentation site in the near future. Until then, here are some key usage topics.

## Sessions

The `session` is provided upon request by the Backstack API. This project requests an updated session before each route change. The logic is implemented in the `src/router.js` file.

See the [Backstack session docs](https://backstack.com/guide/sessions.html) for more information on the session architecture.

## Access Control


Unfortunately, feature access control is hard-coded throughout the project. 

```html
<button v-if="session.hasAccess('account-users:c')" ... >Add User</button>
```

We should create constants for these (e.g. "ACCESS_ACCOUNT_USERS" : "your-label"), but the added layer complicates an otherwise simplified implementation. Features are defined using the Backstack dashboard in whatever format desired. You could create a mapping function if you decide on a different labeling schema. (Or search and replace the hard-coded values.)

By default, the project uses the `hasAccess()` function provided in `src/utils/hasAccess.js` so you can easily change the entire logic if desired. This function is for the `session.hasAccess()` method used to enforce of every aspect of access control.

```js
// Anywhere in your codebase:
const session = useSession();

if (session.hasAccess('some-feature:*,another-feature:r')) {
  // ...
}else{
  // ...
}
```


See the [Backstack access control docs](https://backstack.com/guide/access-control.html) for 
more information on the entire architecture.

> Demo mode uses an independent schema for access control while developing. See `.env.local` for the assignment and `src/session.js` for the implementation.

## Axios

The `axios` installation can be used for any endpoint without additional configuration. A few`axios.config` settings have been added for using the Backstack features related to this project.

| Config key | Description                                                                                                             |
| ---------- | ----------------------------------------------------------------------------------------------------------------------- |
| `api`      | An optional api-specific setting for implementing your logic.                                                           |
| `alert`    | When using the Backstack api, you can bypass the alert logic when needed by setting this value to false in the request. |

```js
await axios.post('https://api.backstack.com/v1/app/reset-password', data, { api: 'backstack' }) ...
```

The [src/components/AxiosError.vue](https://github.com/deloachtech/backstack-demo/blob/main/src/components/AxiosError.vue) component handles Backstack errors using Axios `interceptors`. It eliminates a significant amount of error handling logic elsewhere. You can modify this component to incorporate other services if desired.

