# Adding a new room controller type

First you need to create a class for the room controller type. Then, call `registerRoomController` with the class.

```ts
import { registerRoomController, RoomControllerInstance } from "../../../src/plugins.js";

class MyRoomController extends RoomControllerInstance {
  ...
}

registerRoomController(MyRoomController);
```

To allow the devices to communicate using the room controller, add methods to send messages and make sure they are consistent across room controllers of the same supertype.

Here is the reference for the contents of the class:

## Static fields

Items marked with an asterisk (*) are required.

### `id`*

```ts
static id: `${string}:${string}` = "myroomcontroller:mini";
```

The ID of the room controller type. This ID is used to internally refer to the room controller type and is not shown to the user.

It consists of two parts separated by a colon: `supertype:subtype`.

Several related room controller types can share a supertype.

### `super_name`*

```ts
static super_name = "My Room Controller"
```

The name of the room controller type that is shown to the user. Room controllers with the same super name in their ID should have the same super_name, though this isn't enforced.

### `sub_name`*

```ts
static sub_name = "Mini"
```

The mode or any other name that is specific to the room controller and not shared in the supertype. It should make sense when displayed in a parentheses next to the super name, and when displayed with a lighter font next to the super name:

`SuperRoom (USB)`  
`SuperRoom (Bluetooth)` <-- These should make sense.

|           |           |
| --------- | --------- |
| SuperRoom | USB       |
| SuperRoom | Bluetooth |

_So should the table above._

### `settingsFields`*

```ts
static settingsFields: SettingsFieldDef[] = [
  ...
]
```

An array of `HMApi.T.SettingsField` that indicate the options for the room controller. (e.g. port)

## Built in fields

These fields are built in, They are considered read-only and should not be changed.

### `id`

Type: `string`

Contains the room's ID. (not to be confused with controller type ID)

### `name`

Type: `string`

Contains the name of the room. (not to be confused with controller type name)

### `icon`

Type: `"living-room" | "kitchen" | "bedroom" | "bathroom" | "other"`

Contains the icon chosen for the room.

### `type`

Type: `string`

The room's controller type ID. Equal to the `id` static field.

### `settings`

Type: `Record<string, string|number|boolean>`

Contains the values for the settings fields.

### `disabled`

Type: `false | string`

When the room controller is working normally, the value is `false`.  
When the room controller has been disabled by using `disable(reason)`, it will be a string which contains the error.

### `initialized`

Type: `boolean`

Indicates whether the room controller has been initialized. Will be set to true in `init()` and set to false in `dispose()`.

### `devices`

Type: `Record<string, DeviceInstance>`

Contains the instances of the devices inside the room controller.

## Methods

### `constructor`

```ts
constructor(properties: HMApi.T.Room) {
  super(properties);

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

Here you can open connections and initialize the room controller. The devices will be initialized at the end by the base class.

Called when:

- The room is created
- The room is edited
- The room is restarted
- The hub has started

### `dispose`

```ts
async dispose() {
  await super.dispose();

  // Custom code
}
```

Here you can shut down the room controller, close connections and dispose any resources. The devices are already shut down at this point.

Called when:

- The room is edited
- The room is deleted
- The room is restarted
- The hub is shutting down

### `disable` (built-in)

```ts
this.disable("Some error occurred");
```

Call this when there is an error to disable the room.

Do not override.

### `validateSettings` (static)

```ts
static validateSettings(settings: Record<string, string|number|boolean>): void | undefined | string | Promise<void | undefined | string> {
  return "This parameter is invalid";
}
```

Here you can run custom checks on the settings fields (e.g. a port existing), in addition to the built in ones.  
Return undefined or nothing when all fields are valid. Return an error message when a field is invalid.

Can be async.

The built-in checks are:

- Data type
- Minimum/maximum value/length
- Value being one of the options
- Value not being empty (for required fields)
