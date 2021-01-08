# @flayyer/flayyer-types

![npm-version](https://badgen.net/npm/v/@flayyer/flayyer-types)
![downloads](https://badgen.net/npm/dt/@flayyer/flayyer-types)
![size](https://badgen.net/bundlephobia/minzip/@flayyer/flayyer-types)
![tree-shake](https://badgen.net/bundlephobia/tree-shaking/@flayyer/flayyer-types)

Flayyer type definition for Typescript templates created with [create-flayyer-app](https://github.com/flayyer/create-flayyer-app/).

**👉 Want to learn more about smart image previews? Visit as at [flayyer.com](https://flayyer.com?ref=flayyer-types)**

**📖 Checkout our official documentation: [docs.flayyer.com](https://docs.flayyer.com/docs/advanced/typescript)**

## Install

```sh
npm install --save-dev @flayyer/flayyer-types

yarn add --dev @flayyer/flayyer-types
```

## Usage

The provided `TemplateProps<T>` accepts a generic object type for the possible variables passed to the template. Keep in mind not every variable is guaranteed to be present, this is the reason why every variable will be marked as _possibly undefined_.

```tsx
import React from "react";
import { TemplateProps } from "@flayyer/flayyer-types";

export default function SimpleTemplate({ variables }: TemplateProps) {
  const title = variables.title; // type is `string | undefined`;
  return (
    <div>
      {title && <h1>{title}</h1>}
    </div>
  );
}
```

Since URL serialization converts `Date` and `number` to strings, every field type will be typed as `string | undefined`. Objects and arrays will be also typed using a recursive strategy.

```tsx
import React from "react";
import { TemplateProps } from "@flayyer/flayyer-types";

// Example:
type Variables = {
  title: string;
  count: number;
  price: number;
  createdAt: Date;
  object: {
    name: string;
    age: number;
  };
  array: [
    {
      id: number;
    },
  ];
};

export default function MainTemplate({ variables }: TemplateProps<Variables>) {
  const {
    title, // type is `string | undefined`
    count, // type is `string | undefined`
    price = 10, // type is `string | 10` because incoming values will be string!
    createdAt, // type is `string | undefined`
    object, // type is a recursive object with `string | undefined` values
    array, // type is a recursive array with `string | undefined` values
  } = variables;

  return (
    <div>
      {title && <h1>{title}</h1>}
    </div>
  );
}
```

### Platform information

Sometimes we can identify which platform your link are being shared on. You can get this information via the `agent` prop.

```tsx
import React from "react";
import { TemplateProps, FlayyerAgentName } from "@flayyer/flayyer-types";

export default function MainTemplate({ agent }: TemplateProps) {
  if (agent.name === FlayyerAgentName.WHATSAPP) {
    // Custom rules for squared template
    return ...
  } else {
    // Default 1200x630 banner.
    return ...
  }
}
```

## Import assets

Remove Typescript warning when importing files such as images, fonts, style files, etc.
Use the following code in a `types.d.ts` file in the root fo your Flayyer project.

```ts
// types.d.ts

/// <reference types="@flayyer/flayyer-types/global" />
```

## Experimental Javascript support

You can help your IDE with typing information. Here is an working but experimental example in Visual Studio Code:

```js
import { TemplateProps } from "@flayyer/flayyer-types"; // eslint-disable-line no-unused-vars

/**
 * Make sure to default export a React component
 * @param {TemplateProps} [props] - Flayyer props.
 */
export default function JavascriptTemplate({ variables }) {
  const title = variables.title; // IDE will suggest `title` has type `string | undefined`
  // ...
}
```
