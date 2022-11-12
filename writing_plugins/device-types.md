# Adding a new device type

First you need to create a class for the device type. Then, call `registerDeviceType` with the class.

```ts
import { registerDeviceType, DeviceInstance } from '../../../src/plugins.js';

class MiniLightMono extends DeviceInstance {
    ...
}

registerDeviceType(MiniLightMono);
```

> **Warning**  
> Your IDE might add an automatic import of `registerDeviceType` from `devices.ts`. You should use the one from `plugins.ts` instead.

Here is the reference for the contents of the class:

## Static fields

Items marked with an asterisk (*) are required.

### `id`*

```ts
static id: `${string}:${string}` = "minilight:mono";
```

The ID of the device type. This ID is used to internally refer to the device type and is not shown to the user.

It consists of two parts separated by a colon: `supertype:subtype`.

Several related device types can share a supertype.

### `super_name`*

```ts
static super_name = "MiniLight";
```

The name of the device type that is shown to the user. Device types with the same super name in their ID should have the same super_name, though this isn't enforced.

### `sub_name`*

```ts
static sub_name = "Mono";
```

The mode or any other name that is specific to the device type and not shared in the supertype. It should make sense when displayed in a parentheses next to the super name, and when displayed with a lighter font next to the super name:

`MiniLight (Mono)`  
`MiniLight (RGB)` <-- These should make sense.

|           |      |
| --------- | ---- |
| MiniLight | Mono |
| MiniLight | RGB  |

_So should the table above._

### `icon`*

```ts
static icon: HMApi.T.IconName = "Lightbulb";
```

The icon to show for the device type. It is also the fallback icon for the devices of this type shown in the home page.

Should be a Font Awesome Solid v6 free icon name.

### `forRoomController`*

```ts
static forRoomController: `${string}:${string}` = "arduino:serial";
```

```ts
static forRoomController: `${string}:*` = "arduino:*";
```

```ts
static forRoomController: '*' = '*';
```

The ID room controller type the device type is compatible with.  
For example, a value of `arduino:serial` means the device is only compatible with the `arduino:serial` controller.

The subtype (the second part) can be replaced with an asterisk (`*`), which means the device is compatible with any room controller with the same super type.  
For example, a value of `arduino:*` means the device is compatible with `arduino:serial`, `arduino:i2c`, `arduino:wifi`, `arduino:1234`, etc.

The value can also be a single asterisk (`*`) character, which means the device is compatible with all room controllers. This is useful for devices that do not interact with any room controller and directly connect to the hub.

### `settingsFields`*

```ts
static settingsFields: SettingsFieldDef[] = [
  ...
]
```

An array of `HMApi.T.SettingsField` that indicate the options for the device. (e.g. Arduino pin)

### `hasMainToggle`

```ts
static hasMainToggle = false
```

Whether the device has a man toggle. If a device has a main toggle, it can be turned on/off by clicking it in the home page using the apps.

Default: false

### `clickable`

```ts
static clickable = true;
```

Whether the device can be clicked in the apps. Currently there isn't any click event for the device and this field is only useful in conjunction with the `hasMainToggle` static field.

Default: true

### `interactions`

```ts
static interactions: Record<string, HMApi.T.DeviceInteraction.Type> = {
    "id": {
        ...
    },
    "id": {
        ...
    },
    ...
};
```

The interactions for the device type. Interactions are ways to control the device in addition to the main toggle. <!--TODO: Add interactions page -->

### `defaultInteraction`

The default interaction of the device. A default interaction is displayed on the device itself in addition to its menu. If not set, an on/off label will be shown.

#### Double default interactions

A `TwoButtonNumber` interaction can be used in conjunction with one other interaction. To do this, separate the interactions with a plus sign (+).  
In this case, the `TwoButtonNumber` interaction must appear first.

### `defaultInteractionWhenOff`

The value for `defaultInteraction` when the device is off. Replaces `defaultInteraction` when `hasMainToggle` is true and `mainToggleState` is false, even if it isn't defined.

## Built in fields

These fields are built in, They are considered read-only and should not be changed.

### `id`

Type: `string`

Contains the device's ID. (not to be confused with device type ID)

### `name`

Type: `string`

Contains the name of the device. (not to be confused with device type name)

### `type`

Type: `string`

The device's type ID. Equal to the `id` static field.

### `settings`

Type: `Record<string, string|number|boolean>`

Contains the values for the settings fields.

### `roomId`

Type: `string`

Contains the ID of the room the device is in.

### `disabled`

Type: `false | string`

When the device is working normally, the value is `false`.
When the device has been disabled by using `disable(reason)`, it will be a string which contains the error.

### `initialized`

Type: `boolean`

Indicates whether the device has been initialized. Will be set to true in `init()` and set to false in `dispose()`.

### `roomController` (getter)

Type: `RoomControllerInstance`

Returns the room controller of this device.

You **can** override this method to check the room controller type. This changes the return type of the getter which allows you to access custom fields and methods:

```ts
get roomController() {
    const c = super.roomController;
    if (c instanceof MyRoomController) { // Check whether the room controller is the one specified in `forRoomController`
        return c;
    } else {
        throw new Error("Room controller is not a MyRoomController"); // This error will crash hub. The reason for doing this is that things have gone too wrong for other means of error handling to be used.
    }
}
```

## State fields

These fields contain the current state of the device and can be changed at any time.

### `icon`

Type: `HMApi.T.IconName | undefined`

The icon to show for the device in the home page. If set to `undefined`, the static field will be used. Should be a Font Awesome Solid v6 free icon name.

Default: undefined

### `iconText`

Type: `string | undefined`

A big text to show instead of the icon. Should be vert short so it can fit in the icon area. If set, it will override the `icon` field.

Default: undefined

### `iconColor`

Type: `HMApi.T.UIColor | undefined`

The color of the icon or text icon. If not set, the default text color will be used. Will be ignored if `mainToggleState` is true.

Default: undefined

### `mainToggleState`

Type: `boolean`

The state of the main toggle. When true, the device will be shown as active.

Default: false

### `activeColor`

Type: `HMApi.T.UIColor | undefined`

The background color of the device when `mainToggleState` is true. If not set, the accent color will be used.

### `interactionStates`

Type: `Record<string, HMApi.T.DeviceInteraction.State | undefined>`

The state of the interactions of the device. Define an initializer to set default states.

## Methods

### `constructor`

```ts
constructor(properties: HMApi.T.Device, room: RoomControllerInstance) {
    super(properties, room);

    // Custom code
}
```

Here you can initialize any custom fields. All [built-in fields](#built-in-fields) are already initialized by the base class.

Any initialization code such as connection or IO requests should be inside `init`.

### `init`

```ts
async init() {
  // Custom code

  return super.init();
}
```

Here you can open connections and initialize the device. The room controller is already initialized when this method is called.

Called when:

- The device is created
- The device is edited
- The device is restarted
- The room containing the device is initialized
- The hub has started

### `dispose`

```ts
async dispose() {
  await super.dispose();

  // Custom code
}
```

Here you can shut down the device, close connections and dispose any resources. The room controller is not yet shut down when this method is called.

Called when:

- The device is edited
- The device is deleted
- The device is restarted
- The room containing the device is shutting down
- The hub is shutting down

### `disable` (built-in)

```ts
this.disable("Some error occurred");
```

Call this when there is an error to disable the device.

Do not override.

### `getCurrentState`

```ts
async getCurrentState() {
    // Update state fields
    // this.iconText = (await getTemperature())+"°C"

    return super.getCurrentState();
}
```

This method is called when refreshing the device states. You can update the state fields before they are returned.

### `toggleMainToggle`

```ts
async toggleMainToggle() {
    await super.toggleMainToggle();

    // Custom code
}
```

This method is called when the main toggle of the device is toggled. Here you can send messages to the room controller to turn on/off the device.

### `sendInteractionAction`

```ts
async sendInteractionAction(interactionId: string, action: HMApi.T.DeviceInteraction.Action) {
    await super.sendInteractionAction(interactionId, action);

    // Custom code
}
```

This method is called when an action is sent to an interaction in the device. Here you can reflect the changes in your device.

### `validateSettings` (static)

```ts
static validateSettings(settings: Record<string, string|number|boolean>): void | undefined | string | Promise<void | undefined | string> {
  return "This parameter is invalid";
}
```

Here you can run custom checks on the settings fields (e.g. an Arduino pin existing), in addition to the built in ones.  
Return undefined or nothing when all fields are valid. Return an error message when a field is invalid.

Can be async.

The built-in checks are:

- Data type
- Minimum/maximum value/length
- Value being one of the options
- Value not being empty (for required fields)
