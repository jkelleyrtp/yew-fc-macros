# QoL macros for functional components in Yew

Yew is getting functional components. Yay! But they're ugly to write. Boo!

This crate exposes the `functional_componenet` macro to skip the boiler plate and get right into developing your newest Yew app.

This is the current way for writing FCs in Yew:

```rust
use yew::{html, Html, Properties};
use yew_functional::{use_state, FunctionComponent, FunctionProvider};

#[derive(Default, Clone, Debug, PartialEq, Properties)]
pub struct CounterProps {
    initial: i32,
}

struct CounterProvider;
impl FunctionProvider for CounterProvider {
    type TProps = CounterProps;
    fn run(props: &Self::TProps) -> Html {
        let (counter, set_counter) = use_state(|| 0);

        return html! {
            <div>
            { format!("Counter total: {}", counter) }
            <button onclick=CB(move |evt| set_counter(*counter + 1))>{"Click me"} </button>
            </div>
        };
    }
}

pub type Counter = FunctionComponent<CounterProvider>;
```

This macro turns the above code into:

```rust
use fpmacros::functional_component;
use yew::{html, Html, Properties};
use yew_functional::{use_state, FunctionComponent, FunctionProvider};

#[derive(Default, Clone, Debug, PartialEq, Properties)]
pub struct CounterProps {
    initial: i32,
}

#[functional_component]
pub fn counter(props: &CounterProps) -> Html {
    let (counter, set_counter) = use_state(|| 0);

    return html! {
        <div>
        { format!("Counter total: {}", counter) }
        <button onclick=CB(move |evt| set_counter(*counter + 1))>{"Click me"} </button>
        </div>
    };
}
```

Note, that this macro follows CamelCase and snake_case conventions, converting your snake_case function identifier to its CamelCase equivalent. Be careful not to rely on the CamelCase version of your functional component to be consistent within the file's scope.
