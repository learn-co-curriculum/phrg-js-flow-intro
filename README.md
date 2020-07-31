# Flow

Flow is a static type checker for your JavaScript code. It checks your code for errors through static type annotations. These types allow you to tell Flow how you want your code to work, and Flow will make sure it does work that way.

[Read about Type Systems and why Flow was designed before continuing this README](https://flow.org/en/docs/lang/).

Next, [read about Flow's Primitve Types](https://flow.org/en/docs/types/primitives/) and then explore other `Type` types using the navigation on the right side of those docs.

Below are some example `types` that rely on one another, directly taken from Nitro. Take a look over each type definition.

```js
// @flow

type Agreement = {
  id: number,
  acknowledgementText: string,
  acknowledgementType: 'signature' | 'checkbox',
  departments: ?Array<Object>,
  type: string,
  active: boolean,
  code: string,
  html: string,
  name: string,
  text: string,
  witness: boolean,
  initials: boolean,
}

type AgreementSignature = {
  id: number,
  image: string,
  signatureType: string,
}

type AgreementAcceptance = {
  id: number,
  accepted: boolean,
  agreement?: Agreement,
  signatures?: Array<AgreementSignature>,
  updatedAt: String,
}

type FormProps = {
  acceptances: Array<AgreementAcceptance>,
  handleSubmit: () => void,
  invalid: boolean,
  submitting: boolean,
  pristine: boolean,
  firstName: string,
  hasDriverLicense: boolean,
  lastName: string,
  middleName: string,
  nickName: string,
  preferredName: string,
  phone: string,
  altPhoneNumber: string,
  stateIssued: string,
  referral: boolean,
  firefoxSelectFieldFix: () => void,
  dispatchZipcodeValidation: () => void,
}
```

Instead of defining types within a function declaration, the majority of Nitro uses [type aliases](https://flow.org/en/docs/types/aliases/). This just means we store a complex type by name, in PascalCase, so it can be reused. It also means we can use an alias as a `Type` itself, as we see with `Agreement` in `AgreementAcceptance`.

The majority of the types above are primitive types, such as boolean, string and number. `AgreementAcceptance`'s `updatedAt` property actually specifies `String` with a capital `S`. There is no difference between `string` and `String`. Flow will accept lowercase or uppercase primitives.

In `Agreement`, we see a [union type](https://flow.org/en/docs/types/unions/) for its `acknowledgementType` property. And instead of accepting different primitive types, it accepts [literal types](https://flow.org/en/docs/types/literals/) of `'signature'` or `'checkbox'`.

In `FormProps`, the `acceptances` property is an [array type](https://flow.org/en/docs/types/arrays/). It specifies that the `AgreementAcceptance` type is what the array must contain. `Array` types could also contain primitive types, such as `Array<number>` or `Array<string>`. It could even specify an [`Object` type](https://flow.org/en/docs/types/objects/#toc-object-type), as can be seen with `Agreement`'s `departments` property.

What is going on with the `?` in front of the `?Array<Object>` type though? This is called a [`Maybe` type](https://flow.org/en/docs/types/maybe/). `Maybe` types accept the provided type as well as `null` or `undefined`. So `?Array<Object>` would mean `Array<Object>`, `null`, or `undefined`.

In `AgreementAcceptance` we see the `?` again, but this time it is right after the property. This is NOT a `Maybe` type. This just means that this property is optional. Which means when the `?` is not there that the property is required.

There is one type left from the example above we have not covered. That is the [function type](https://flow.org/en/docs/types/functions/#toc-syntax-of-functions), which is found 3 times in `FormProps`. This declaration, `() => void`, specifies that the value of the property is a function that takes zero arguments and returns [`void`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/void).
