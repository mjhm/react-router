# Index Routes and Index Links

## Index Routes

To illustrate the use case for `IndexRoute`, imagine the following route
config without it:

```js
<Router>
  <Route path="/" component={App}>
    <Route path="accounts" component={Accounts}/>
    <Route path="statements" component={Statements}/>
  </Route>
</Router>
```

When the user visits `/`, the App component is rendered, but the route
doesn't match `/accounts` or `/statements`, so `this.props.children` is
`undefined`. To render a default component, say `<Home/>`, you could
easily include `{this.props.children || <Home/>}` in the `App` component.

However the bare `<Home/>` component doesn't participate in routing,
and therefore can't take advantage of features such as the `onEnter` hooks,
etc. By specifying it as an `IndexRoute`, `<Home/>` will be `App`'s
`this.props.children`. So it will render as a first
class route component peer of `Accounts` and `Statements`.

```js
<Router>
  <Route path="/" component={App}>
    <IndexRoute component={Home}/>
    <Route path="accounts" component={Accounts}/>
    <Route path="statements" component={Statements}/>
  </Route>
</Router>
```

Now `App` can render `{this.props.children}` and we have a first-class
route for `Home` that can participate in routing.

## Index Links

If you were to include `<Link to="/">Home</Link>` in this app, the
resulting link would always be rendered with the `activeClassName` and
`activeStyle` since every URL starts with `/`. This is a problem because
we'd like to link to `Home` but only show it as active when the `<Home/>`
component is rendered.

Use `<IndexLink to="/">Home</IndexLink>` to link to the `<Home/>` index
route so that it is only active for the specific `/` route.
