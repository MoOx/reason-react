---
id: component-as-prop
title: Component as Prop
---

In ReactJS, `<Menu banner=MyBanner />` is easy; in ReasonReact, we can't trivially pass the whole component module ([explanations](https://reasonml.github.io/guide/language/module)). Solution:

```reason
let bannerCallback = (prop1, prop2) => <MyBanner message=prop1 count=prop2 />;

<Menu bannerFunc=bannerCallback />;
```

--- 

In practise, you could imagine accepting children like this:

```reason
<RandomComponent wrapperFunc=((~children) => <div className="something-special"> ...children </div>) />
```

The associated definition of the `RandomComponent` will be:

```reason
let component = ReasonReact.statelessComponent("RandomComponent");

let make =
    (
      ~wrapperFunc=(~children) => <div> ...children </div>,
      _children
    ) => {
  ...component,
  render: (_self) =>
    <div>
      (wrapperFunc(
        ~children=[|
          <span>Something</span>
        |]
      ))
       
    </div>
}
```

