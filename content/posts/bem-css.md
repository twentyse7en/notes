+++
author = "Abijith b"
title =  "Better css classnames with BEM"
date = 2022-02-04T21:36:59+05:30
tags = [
    "css",
    "web",
]
+++

For me naming css class names took more time than writing actual styles.
usual I get bored and end up review comment to alter names. One of my
seniors suggested to checkout BEM.

## So what is BEM?
BEM is methodology introduced at [yandex]() for building sites fast
and supported for a long time. It helps to create extendable and
reusable interface components.

BEM stands for `block--element__modifier`

## Block Element Modifier

### Block

Indepedent page component that can be reused. Block name describes
what is it in higher level.

Example 1
```html
<button class='button'>

</button>

<div class='menu'>

</div>
```

blocks can be nested.

Example 2

```html
<header class="header">
    <div class="logo">
    </div>
</header>
```

### Element

Part of block, it doesn't have any indepedent existence, always related
to it's block.

It's separated using double underscore `__`

```html
<form class="search-form">
    <input class="search-form__input">
    </input>

    <button class="search-form__button">
    </button>
</form>
```

elements can be nested

```html
<form class="search-form">
    <div class="search-form__container">
	<input class="search-form__input">
	</input>

	<button class="search-form__button">
	</button>
    </div>
</form>
```

It shouldn't be used indepedently without it's block

```html
<form class="search-form">
    ...
</form>

<!-- not recommended -->
<button class="search-form__button">
</button>
```

### Modifier

An entity that defines the appearance, state or behaviour of block or element.

common usecase:
- size : `--large`, `--medium`
- disabled: `--disabled`
- theme: `--tokyo-night`, `--dracula`

## complete example

```html
<div class='chat chat--internal'>
    <input class='chat__input'>
    </input>

    <button class='chat__button chat__submit--disabled'>
    </button>
</div>

```

```css
.chat {}
.chat--internal {}
.chat__input {}
.chat__button {}
.chat__submit--disabled {}
```

```scss
.chat {
    &--internal {}
    &__input {}
    &__button {
	&--disabled {}
    }
}
```
