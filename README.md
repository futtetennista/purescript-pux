# Pux

[![Latest release](https://img.shields.io/bower/v/purescript-pux.svg)](https://github.com/alexmingoia/purescript-pux/releases)
[![Build Status](https://travis-ci.org/alexmingoia/purescript-pux.svg?branch=master)](https://travis-ci.org/alexmingoia/purescript-pux)
[![Gitter Chat](https://img.shields.io/gitter/room/gitterHQ/gitter.svg)](https://gitter.im/alexmingoia/purescript-pux)

Pux is an FRP interface to React, based on the [Elm app
architecture](https://github.com/evancz/elm-architecture-tutorial). It is a
simple pattern for modular, nested components that are easy to test, refactor,
and debug - making it simple and straightfoward to build complex web
applications.

- Build React UIs as a fold of actions to state via
  [`purescript-signal`](https://github.com/bodil/purescript-signal/)
- Type-safe routing
- Server-side rendering (isomorphic applications)
- Hot-reloading of components

---

- [Guide](http://alexmingoia.github.io/purescript-pux)
- [API Reference](http://alexmingoia.github.io/purescript-pux/docs/API/Pux.html)

### Installation

The easiest way to get started is to clone the
[starter app](http://github.com/alexmingoia/pux-starter-app),
which includes a hot-reloading setup using webpack:

```sh
git clone git://github.com/alexmingoia/pux-starter-app.git example
cd example
npm install
npm run watch
```

Pux is also available as the bower package `purescript-pux`.

### Example

The following chunk of code sets up a basic counter that you can increment and
decrement:

```purescript
import Prelude (const, show, bind)
import Pux.Html (Html, (!), (#), div, span, button, text)
import Pux.Html.Events (onClick)

data Action = Increment | Decrement

type State = Int

update :: Action -> State -> State
update Increment count = count + 1
update Decrement count = count - 1

view :: State -> Html Action
view count =
  div # do
    button ! onClick (const Increment) # text "Increment"
    span # text ("Counter: " ++ show count)
    button ! onClick (const Decrement) # text "Decrement"
  where bind = Pux.Html.Elements.bind

main = do
  app <- start
    { initialState: 0
    , update: fromSimple update
    , view: view
    , inputs: []
    }

  renderToDOM "#app" app.html
```
