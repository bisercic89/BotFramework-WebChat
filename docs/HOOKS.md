# Web Chat API as React hooks

Web Chat is designed to be highly customizable. In order to build your own UI, you can use React Hooks to hook your UI component into Web Chat API.

To enable Web Chat API, all UI components must be located under the [`<Composer>`](https://github.com/microsoft/BotFramework-WebChat/blob/master/packages/component/src/Composer.js) component. You can refer to our [plain UI customization](https://github.com/microsoft/BotFramework-WebChat/tree/master/samples/06.recomposing-ui/d.plain-ui) sample for details.

## Why React Hooks

React Hooks will make your code cleaner and shorter, and also greatly improve readability.

Web Chat exposes APIs through React Hooks. This API surface enables us to freely move things behind the scene, introduce new APIs, and safely deprecate APIs. It will also make it easier to shuffle work between the internal Redux store and React Context.

## Two types of hooks

We design our hooks largely with two basic shapes:

-  Actions, these are functions that you can call at any time to perform a side-effect
-  Properties, these are getter functions with an optional setter
   -  This is same as [React State Hook pattern](https://reactjs.org/docs/hooks-state.html), but setters are optional
   -  If the value is changed, React will call your render function again

### Actions

All actions will return a function that can be called at a later point. For example, if you want to focus to the send box, you will use the following code:

```js
const focusSendBox = useFocusSendBox();

focusSendBox();
```

### Properties

All properties follow [React State Hook pattern](https://reactjs.org/docs/hooks-state.html). For example, if you want to get and set the value of send box, you will use the following code:

```js
const [sendBoxValue, setSendBoxValue] = useSendBoxValue();

console.log(`The send box value is "${sendBoxValue}".`);

setSendBoxValue('Hello, World!');
```

> Note: some properties may not be settable.

# List of hooks

Following is the list of hooks supported by Web Chat API.

-  [`useActivities`](#useactivities)
-  [`useAdaptiveCardsHostConfig`](#useadaptivecardshostconfig)
-  [`useAdaptiveCardsPackage`](#useadaptivecardspackage)
-  [`useAvatarForBot`](#useavatarforbot)
-  [`useAvatarForUser`](#useavatarforuser)
-  [`useByteFormatter`](#useByteFormatter)
-  [`useConnectivityStatus`](#useconnectivitystatus)
-  [`useDateFormatter`](#useDateFormatter)
-  [`useDebouncedNotification`](#usedebouncednotification)
-  [`useDictateInterims`](#usedictateinterims)
-  [`useDictateState`](#usedictatestate)
-  [`useDirection`](#useDirection)
-  [`useDisabled`](#usedisabled)
-  [`useDismissNotification`](#usedismissnotification)
-  [`useEmitTypingIndicator`](#useemittypingindicator)
-  [`useFocusSendBox`](#usefocussendbox)
-  [`useGrammars`](#usegrammars)
-  [`useGroupTimestamp`](#usegrouptimestamp)
-  [`useLanguage`](#uselanguage)
-  [`useLastTypingAt`](#uselasttypingat)
-  [`useLocalize`](#uselocalize) (Deprecated)
-  [`useLocalizer`](#useLocalizer)
-  [`useMarkActivityAsSpoken`](#usemarkactivityasspoken)
-  [`useNotification`](#usenotification)
-  [`usePerformCardAction`](#useperformcardaction)
-  [`usePostActivity`](#usepostactivity)
-  [`useReferenceGrammarID`](#usereferencegrammarid)
-  [`useRelativeTimeFormatter`](#useRelativeTimeFormatter)
-  [`useRenderActivity`](#userenderactivity)
-  [`useRenderActivityStatus`](#userenderactivitystatus)
-  [`useRenderAttachment`](#userenderattachment)
-  [`useRenderMarkdownAsHTML`](#userendermarkdownashtml)
-  [`useScrollToEnd`](#usescrolltoend)
-  [`useSendBoxValue`](#usesendboxvalue)
-  [`useSendEvent`](#usesendevent)
-  [`useSendFiles`](#usesendfiles)
-  [`useSendMessage`](#usesendmessage)
-  [`useSendMessageBack`](#usesendmessageback)
-  [`useSendPostBack`](#usesendpostback)
-  [`useSendTimeoutForActivity`](#usesendtimeoutforactivity)
-  [`useSendTypingIndicator`](#usesendtypingindicator)
-  [`useSetNotification`](#usesetnotification)
-  [`useShouldSpeakIncomingActivity`](#useshouldspeakincomingactivity)
-  [`useStartDictate`](#usestartdictate)
-  [`useStopDictate`](#usestopdictate)
-  [`useStyleOptions`](#usestyleoptions)
-  [`useStyleSet`](#usestyleset)
-  [`useSubmitSendBox`](#usesubmitsendbox)
-  [`useSuggestedActions`](#usesuggestedactions)
-  [`useTimeoutForSend`](#usetimeoutforsend)
-  [`useUserID`](#useuserid)
-  [`useUsername`](#useusername)
-  [`useVoiceSelector`](#usevoiceselector)
-  [`useWebSpeechPonyfill`](#usewebspeechponyfill)

## `useActivities`

```js
useActivities(): [Activity[]]
```

This function will return a list of activities.

## `useAdaptiveCardsHostConfig`

```js
useAdaptiveCardsHostConfig(): [AdaptiveCards.HostConfig]
```

This function is only available in full bundle. The function will return the Adaptive Cards Host Config used for styling Adaptive Cards.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useAdaptiveCardsPackage`

```js
useAdaptiveCardsPackage(): [AdaptiveCards]
```

This function is only available in full bundle. The function will return the Adaptive Cards package used for building and rendering Adaptive Cards.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useAvatarForBot`

```js
useAvatarForBot(): [{
  image: string,
  initials: string
}]
```

This function will return the image and initials of the bot. Both image and initials are optional and can be falsy.

To set the avatar for the bot, change the props passed to Web Chat via style options.

## `useAvatarForUser`

```js
useAvatarForUser(): [{
  image: string,
  initials: string
}]
```

This function will return the image and initials of the user. Both image and initials are optional and can be falsy.

To set the avatar for the user, change the props passed to Web Chat via style options.

## `useByteFormatter`

> New in 4.9.0.

```js
useByteFormatter() => (bytes: number) => string
```

This function will return a function that, when called with a file size, will return a localized representation of the size in bytes, kilobytes, megabytes, or gigabytes. It honors the language settings from the `useLanguage` hook.

## `useConnectivityStatus`

```js
useConnectivityStatus(): [string]
```

This function will return the Direct Line connectivity status:

-  `connected`: Connected
-  `connectingslow`: Connecting is incomplete and more than 15 seconds have passed
-  `error`: Connection error
-  `notconnected`: Not connected, related to invalid credentials
-  `reconnected`: Reconnected after interruption
-  `reconnecting`: Reconnecting after interruption
-  `sagaerror`: Errors on JavaScript renderer; please see the browser's console
-  `uninitialized`: Initial connectivity state; never connected and not attempting to connect.

## `useDateFormatter`

> New in 4.9.0.

```js
useDateFormatter() => (dateOrString: (Date | number | string)) => string
```

This function will return a function that, when called with a `Date` object, `number`, or `string`, will return a localized representation of the date in absolute time. It honors the language settings from the `useLanguage` hook.

## `useDebouncedNotification`

```js
interface Notification {
  alt?: string;
  id: string;
  level: 'error' | 'info' | 'success' | 'warn';
  message: string;
}

useDebouncedNotifications(): [Notification[]]
```

When called, this hook will return a debounced array of notifications.

Due to debouncing, notifications retrieved using this hook may not be current. At the time of convergence, this hook will trigger another render.

For the debounce behavior, please read our [article regarding notification system](https://github.com/microsoft/BotFramework-WebChat/tree/master/docs/NOTIFICATION.md).

## `useDictateInterims`

```js
useDictateInterims(): [string[][]]
```

This function will return active interims processed from a dictation event.

The first array represents separate sentences while the second array represents potential ambiguities or alternatives for the same sentence.

## `useDictateState`

```js
useDictateState(): [string]
```

This function will return one of the following dictation states:

-  `IDLE`: Recognition engine is idle; not recognizing
-  `WILL_START`: Will start recognition after synthesis completed
-  `STARTING`: Recognition engine is starting; not accepting any inputs
-  `DICTATING`: Recognition engine is accepting input
-  `STOPPING`: Recognition engine is stopping; not accepting any inputs

> Please refer to `Constants.DictateState` in `botframework-webchat-core` for up-to-date details.

To control dictate state, use the [`useStartDictate`](#usestartdictate) and [`useStopDictate`](#usestopdictate) hooks.

## `useDirection`

```js
useDirection(): [string]
```

This function will return one of two language directions:

-  `ltr` or otherwise: Web Chat UI will display as left-to-right
-  `rtl`: Web Chat UI will display as right-to-left

This value will be automatically configured based on the `locale` of Web Chat.

If you would prefer to set this property manually, change the value `dir` prop passed to Web Chat.

## `useDisabled`

```js
useDisabled(): [boolean]
```

This function will return whether the UI should be disabled or not. All interactable UI components should honor this value.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useDismissNotification`

```js
useDismissNotification(): (id: string) => void
```

This hook will return a function which can be called to dismiss a notification given its ID.

## `useEmitTypingIndicator`

```js
useEmitTypingIndicator(): () => void
```

When called, this function will send a typing activity from the user to the bot.

## `useFocusSendBox`

```js
useFocusSendBox(): () => void
```

When called, this function will send focus to the send box.

## `useGrammars`

```js
useGrammars(): [string[]]
```

This function will return grammars for speech-to-text. Grammars is a list of words provided by the implementer for the speech-to-text engine to bias towards. It is commonly used for selecting the correct words with same or similar pronunciations, e.g. Bellevue vs. Bellview vs. Bellvue.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useGroupTimestamp`

```js
useGroupTimestamp(): [number]
```

This function will return the interval for grouping similar activities with a single timestamp. The interval is represented in milliseconds.

For example, if this value is `5000`, successive activities within 5 seconds will share the timestamp of the first message.

To control the `groupTimestamp` state, change the props passed to Web Chat via style options.

## `useLanguage`

```ts
type LanguageOptions = 'speech';

useLanguage(options?: LanguageOptions): [string]
```

This function will return the language of the UI. All UI components should honor this value.

If no options are passed, the return value will be the written language.

If `"speech"` is passed to `options`, the return value will be the oral language instead of written language. For example, the written language for Hong Kong and Taiwan are Traditional Chinese, while the oral language are Cantonese and Taiwanese Mandarin respectively.

To modify this value, change the value in the `locale` prop passed to Web Chat.

## `useLastTypingAt`

```js
useLastTypingAt(): [{
  [id: string]: number
}]
```

This function will return a map of the last typing time of all participants. The time is based on the client clock.

This property is computed on every incoming activity.

## `useLocalize`

```js
useLocalize(identifier: string) => string
```

> This function is deprecated. Developers should migrate to [`useLocalizer`](#useLocalizer).

This function will return a localized string represented by the identifier. It honors the language settings from the `useLanguage` hook.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useLocalizer`

> New in 4.9.0.

```js
interface LocalizerOptions {
  plural: Boolean;
}

useLocalizer(options: LocalizerOptions) => (identifier: string, ...arguments: string[]) => string
```

This function, when called, will return a localized string represented by the identifier and its arguments. It honors the language settings from the `useLanguage` hook.

### Plural form

```js
interface PluralRulesIdentifier {
  zero?: string,
  one?: string,
  two?: string,
  few?: string,
  many?: string,
  other: string
}

useLocalizer({ plural: true }) => (identifiers: PluralRulesIdentifier, firstArgument: number, ...otherArguments: string[]) => string
```

Some localized strings may contains words that have multiple plural forms, for example, "1 notification" and "2 notifications".

Web Chat supports multiple plural forms based on [Unicode Plural Rules](http://cldr.unicode.org/index/cldr-spec/plural-rules). For a single string, you will need to specify up to 6 strings (zero, one, two, few, many, other). You must at least specify string that represent the "other" plural rule.

The first argument must be a number and used to select which plural form to use. Only cardinal is supported at this time.

```js
const localize = useLocalize({ plural: true });

// The following code will print "2 notifications" to console.

console.log(
   localize(
      {
         zero: 'No notifications',
         one: 'A notification',
         two: '$1 notifications',
         few: '$1 notifications',
         many: '$1 notifications',
         other: '$1 notifications'
      },
      2
   )
);
```

## `useMarkActivityAsSpoken`

```js
useMarkActivityAsSpoken(): (activity: Activity) => void
```

When called, this function will mark the activity as spoken and remove it from the text-to-speech queue.

## `useNotifications`

```js
interface Notification {
  alt?: string;
  id: string;
  level: 'error' | 'info' | 'success' | 'warn';
  message: string;
}

useNotifications(): [Notification[]]
```

When called, this hook will return an array of notifications.

## `usePerformCardAction`

```js
usePerformCardAction(): ({
  displayText: string,
  text: string,
  type: string,
  value: string
}) => void
```

When called, this function will perform the card action based on its `type`. The card action will be performed by `cardActionMiddleware`.

List of supported card action types can be found in this [Direct Line Activity card action schema](https://github.com/microsoft/botframework-sdk/blob/master/specs/botframework-activity/botframework-activity.md#type-1).

## `usePostActivity`

```js
usePostActivity(): (activity: Activity) => void
```

When called, this function will post the activity on behalf of the user, to the bot.

You can use this function to send any type of activity to the bot, however we highly recommend limiting the [activity types](https://github.com/microsoft/BotFramework-WebChat/tree/master/docs/ACTIVITYTYPES.md) to one of the following:

-  `event`
-  `message`
-  `typing`

## `useReferenceGrammarID`

```js
useReferenceGrammarId(): [string]
```

When called, this function will return the reference grammar ID used to improve speech-to-text performance when used with Cognitive Services.

This value is not controllable and is passed to Web Chat from the Direct Line channel.

## `useRelativeTimeFormatter`

> New in 4.9.0.

```js
useRelativeTimeFormatter() => (dateOrString: (Date | number | string)) => string
```

This function will return a function that, when called with a `Date` object, `number`, or `string`, will return a localized representation of the date in relative time, e.g. "2 minutes ago". It honors the language settings from the `useLanguage` hook.

## `useRenderActivity`

```js
useRenderActivity(
  renderAttachment: ({
    activity: Activity,
    attachment: Attachment
  }) => React.Element
): ({
  activity: Activity,
  nextVisibleActivity: Activity
}) => React.Element
```

This function is for rendering an activity and its attachments inside a React element. Because of the parent-child relationship, the caller will need to pass a render function in order for the attachment to create a render function for the activity. When rendering the activity, the caller will need to pass `activity` and `nextVisibleActivity`. This function is a composition of `activityRendererMiddleware`, which is passed as a prop.

Note that not all activities are rendered, e.g. the event activity. Because of this, those activities will not be rendered. The `nextVisibleActivity` is the pointer to the next visible activity and is intended for the activity status renderer on grouping timestamps for adjacent activities.

### New in 4.8.0

Previously, we use `timestampClassName` to control if the activity should show timestamp or not. The `timestampClassName` should be add as a `class` attribute the DOM element which contains the timestamp.

Today, we pass `activity` and `nextVisibleActivity` to the middleware, so the `activityRendererMiddleware` make the decision about timestamp visibility. For example, developers can group timestamp based on activity type.

## `useRenderActivityStatus`

```js
useRenderActivityStatus(): ({
  activity: Activity,
  nextVisibleActivity: Activity
}) => React.Element
```

This function is for rendering the status of an activity. The caller will need to pass `activity` and `nextVisibleActivity` as parameters. This function is a composition of `activityStatusRendererMiddleware`, which is passed as a prop.

## `useRenderAttachment`

```js
useRenderAttachment(): ({
  activity: Activity,
  attachment: Attachment
}) => React.Element
```

This function is for rendering an attachments inside a React element. The caller will need to pass `activity` and `attachment` as parameters. This function is a composition of `attachmentRendererMiddleware`, which is passed as a prop.

```js
() => next => { activity, attachment } => next({ activity, attachment })
```

## `useRenderMarkdownAsHTML`

```js
useRenderMarkdownAsHTML(): (markdown: string): string
```

This function will return a function that, when called, will render Markdown into an HTML string. For example,

```js
const renderMarkdown = useRenderMarkdown();

renderMarkdown('Hello, World!') === '<p>Hello, World!</p>\n';
```

To modify this value, change the value in the style options prop passed to Web Chat.

## `useScrollToEnd`

```js
useScrollToEnd(): () => void
```

This function will return a function that, when called, will scroll the transcript view to the end.

## `useSendBoxValue`

```js
useSendBoxValue(): [string, (value: string) => void]
```

This function will return the current value of the send box and the setter function to change the value.

## `useSendEvent`

```js
useSendEvent(): (name: string, value: string) => void
```

When called, this function will send an event activity to the bot.

## `useSendFiles`

```js
useSendFiles(): (files: (Blob | File)[]) => void
```

When called, this function will send a message activity with one or more [File](https://developer.mozilla.org/en-US/docs/Web/API/File) attachments to the bot, including these operations:

-  Convert [File](https://developer.mozilla.org/en-US/docs/Web/API/File) into object URL
-  Generate thumbnail and will use a Web Worker and an offscreen canvas if supported

If you are using an `ArrayBuffer`, you can use `FileReader` to convert it into a blob before calling [`URL.createObjectURL`](https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL).

## `useSendMessage`

```js
useSendMessage(): (text: string, method: string) => void
```

When called, this function will send a text message activity to the bot.

You can optionally include the input method how the text message was collected. Currently, if specified, only `speech` is supported.

## `useSendMessageBack`

```js
useSendMessageBack(): (value: any, text: string, displayText: string) => void
```

When called, this function will send a `messageBack` activity to the bot.

## `useSendPostBack`

```js
useSendPostBack(): (value: any) => void
```

When called, this function will send a `postBack` activity to the bot.

## `useSendTimeoutForActivity`

```js
useSendTimeoutForActivity(): (activity: Activity) => number
```

When called, this function will return a function to evaluate the timeout (in milliseconds) for sending a specific activity.

## `useSendTypingIndicator`

```js
useSendTypingIndicator(): [boolean]
```

This function will return whether the typing indicator will be sent to the bot when the send box value is being modified.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useSetNotification`

```js
interface Notification {
  alt?: string;
  id: string;
  level: 'error' | 'info' | 'success' | 'warn';
  message: string;
}

useSetNotification(): (notification: Notification) => void
```

This hook will return a function which can be called to add or update a notification. If a notification with same ID is already in the system, it will be updated. Otherwise, a new notification will be added.

The `message` field will be processed through an internal Markdown renderer. If Markdown is provided, it is recommended to provide plain text via the `alt` field for assistive technologies.

The toast UI will [debounce notifications](https://github.com/microsoft/BotFramework-WebChat/tree/master/docs/NOTIFICATION.md#postponing-changes-via-debounce) that update too frequently.

## `useShouldSpeakIncomingActivity`

```js
useShouldSpeakIncomingActivity(): [boolean, (value: boolean) => void]
```

This function will return a boolean and a function.

1. boolean: whether the next incoming activity will be queued for text-to-speech
1. function: a setter function to control the behavior

If the last outgoing message is sent via speech, Web Chat will set this state to `true`, so the response from bot will be synthesized as speech.

## `useStartDictate`

```js
useStartDictate(): () => void
```

This function will open the microphone for dictation. You should only call this function via a user-initiated gesture. Otherwise, the browser may block access to the microphone.

## `useStopDictate`

```js
useStopDictate(): () => void
```

This function will close the microphone. It will not send the interims to the bot, but leave the interims in the send box.

## `useStyleOptions`

```js
useStyleOptions(): [StyleOptions]
```

This function will return the style options. UI components should honor the styling preferences.

The value is not the same as the props. Web Chat will merge the style options passed in props with default values specified in [`defaultStyleOptions.js`](https://github.com/microsoft/BotFramework-WebChat/blob/master/packages/component/src/Styles/defaultStyleOptions.js).

To modify the value of `styleOptions` state, change the props you pass to Web Chat.

## `useStyleSet`

```js
useStyleSet(): [StyleSet]
```

This function will return the style set.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useSubmitSendBox`

```js
useSubmitSendBox(): () => void
```

This function will send the text in the send box to the bot and clear the send box.

## `useSuggestedActions`

```js
useSuggestedActions(): [CardAction[], (CardAction[]) => void]
```

This function will return an array and a setter function.

1. array: a list of suggested actions that should be shown to the user
1. function: a setter function to clear suggested actions. The setter function can only be used to clear suggested actions, and it will accept empty array or falsy value only.

The suggested actions are computed from the last message activity sent from the bot. If the user posts an activity, the suggested actions will be cleared.

## `useTimeoutForSend`

```js
useTimeoutForSend(): [number]
```

This function will return the interval of time paused before a sending activity is considered unsuccessful. The interval is represented in milliseconds. Due to network partitioning problems, activities that fail to send may eventually be successfully delivered to the bot.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useUserID`

```js
useUserID(): [string]
```

This function will return the user ID.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useUsername`

```js
useUsername(): [string]
```

This function will return the username.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useVoiceSelector`

```js
useVoiceSelector(activity: Activity): (voices: SpeechSynthesisVoice[]) => SpeechSynthesisVoice
```

This function will return a function that can be called to select the voice for a specific activity.

To modify this value, change the value in the style options prop passed to Web Chat.

## `useWebSpeechPonyfill`

```js
useWebSpeechPonyfill(): [{
  SpeechGrammarList: SpeechGrammarList,
  SpeechRecognition: SpeechRecognition,
  speechSynthesis: SpeechSynthesis,
  SpeechSynthesisUtterance: SpeechSynthesisUtterance
}]
```

This function will return the ponyfill for the Web Speech API.

To modify this value, change the value in the style options prop passed to Web Chat.

# Component-specific hooks

These are hooks specific provide specific user experience.

## `MicrophoneButton`

These are hooks that are specific for the microphone button.

-  [`useMicrophoneButtonClick`](#usemicrophonebuttonclick)
-  [`useMicrophoneButtonDisabled`](#usemicrophonebuttondisabled)

### `useMicrophoneButtonClick`

```js
useMicrophoneButtonClick(): () => void
```

When called, this function will toggle microphone open or close.

### `useMicrophoneButtonDisabled`

```js
useMicrophoneButtonDisabled(): () => void
```

This function will return whether the microphone button is disabled. This is different from `useDisabled()`. The microphone button could be disabled because it is currently starting or stopping.

This value can be partly controllable through Web Chat props.

## `SendBox`

These are hooks that are specific for the send box.

-  [`useSendBoxSpeechInterimsVisible`](#usesendboxspeechinterimsvisible)

### `useSendBoxSpeechInterimsVisible`

```js
useSendBoxSpeechInterimsVisible(): [boolean]
```

This function will return whether the send box should show speech interims.

## `TextBox`

These are hooks that are specific to the text box in the send box.

-  [`useTextBoxSubmit`](#usetextboxsubmit)
-  [`useTextBoxValue`](#usetextboxvalue)

### `useTextBoxSubmit`

```js
useTextBoxSubmit(): (setFocus: boolean) => void
```

This function will send the text box value as a message to the bot. In addition to the original `useSubmitSendBox` hook, this function will also scroll to bottom and, optionally, set focus to the send box.

The focus is useful for a phone scenario where the virtual keyboard will only be shown when a text box is focused.

### `useTextBoxValue`

```js
useTextBoxValue(): [string, (value: string) => void]
```

This function will return a string and a function.

1. string: the text box value
1. function: the setter function to set the text box value.

The setter function will call the setter of [`useSendBoxValue`](#usesendboxvalue) and also stop dictation if started.

## `TypingIndicator`

These are hooks that are specific to the typing indicator.

-  [`useTypingIndicatorVisible`](#usetypingindicatorvisible)

### `useTypingIndicatorVisible`

```js
useTypingIndicatorVisible(): [boolean]
```

This function will return whether the typing indicator should be visible or not. This function is time-sensitive, meaning that the value will change as time passes.

This function derives the visibility of the typing indicator via:

-  `typingAnimationDuration` value specified in style options, in milliseconds
-  Values from the `useLastTypingAt` hook
