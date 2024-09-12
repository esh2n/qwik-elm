# 🌳 @esh2n/qwik-elm

🚀 Seamlessly integrate Elm components into your Qwik applications!

Inspired by @mizchi/qwik-svelte and @mizchi/qwik-vue, but tailored for the Elm ecosystem.

## 🌟 Features

- 🔗 Easy integration of Elm components in Qwik
- 🧠 Leverage Elm's robust type system and predictable state management
- ⚡ Benefit from Qwik's partial hydration and lazy-loading
- 🛠 Simple API for bridging Qwik and Elm

## 📦 Installation

```bash
npm install @esh2n/qwik-elm elm vite-plugin-elm -D
```

⚠️ Note: This is currently in the Proof of Concept (PoC) phase. Please thoroughly test in your application before using in production.

## 🚀 Quick Start

### 1. Configure Vite

Update your `vite.config.ts`:

```typescript
import { defineConfig } from "vite";
import { qwikVite } from "@builder.io/qwik/optimizer";
import elmPlugin from "vite-plugin-elm";

export default defineConfig(() => {
  return {
    plugins: [
      elmPlugin({
        debug: process.env.NODE_ENV !== "production",
        optimize: process.env.NODE_ENV === "production",
      }),
      qwikVite()
    ],
  };
});
```

### 2. Create an Elm Component

```plaintext
-- src/components/Counter.elm
module Counter exposing (main)

import Browser
import Html exposing (Html, button, div, text)
import Html.Events exposing (onClick)

type alias Model = Int

type Msg = Increment | Decrement

update : Msg -> Model -> Model
update msg model =
    case msg of
        Increment -> model + 1
        Decrement -> model - 1

view : Model -> Html Msg
view model =
    div []
        [ button [ onClick Decrement ] [ text "-" ]
        , div [] [ text (String.fromInt model) ]
        , button [ onClick Increment ] [ text "+" ]
        ]

main : Program () Model Msg
main =
    Browser.sandbox
        { init = 0
        , update = update
        , view = view
        }
```

### 3. Use in Qwik

```typescript
import { component$ } from "@builder.io/qwik";
import { qwikifyElm$ } from "qwik-elm";
import Counter from "./components/Counter.elm";

const QCounter = qwikifyElm$<{}>({
  init: (node) => {
    const app = Counter.Elm.Counter.init({ node });
    return app;
  }
});

export default component$(() => {
  return <QCounter />;
});
```

## 🔄 Communication between Qwik and Elm

### 🏗 Work in Progress

We're currently developing robust patterns for bi-directional communication between Qwik and Elm. Stay tuned for updates!

## 🛠 API Reference

### qwikifyElm$

```typescript
qwikifyElm$<Props>(options: QwikifyElmOptions): Component<Props>
```

#### Options

- `init`: (node: HTMLElement) => ElmApp
Initialize your Elm application and return the Elm app instance.


## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. 🍴 Fork the repository
2. 🌿 Create your feature branch: `git checkout -b my-new-feature`
3. 💾 Commit your changes: `git commit -am 'Add some feature'`
4. 🚀 Push to the branch: `git push origin my-new-feature`
5. 🎉 Submit a pull request


## 📝 TODO

- 🧪 Comprehensive unit testing
- 🚩 Support for Elm flags
- 🔒 Improve type safety for Elm-Qwik communication
- 🔄 Optimize re-rendering on props change
- 📚 Expand documentation and examples

## 📜 License

MIT