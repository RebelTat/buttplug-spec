# Generic Device Messages

Generic device messages pertain to classes of devices, versus specific
devices. For instance, the generic VibrateCmd should be supported by
all vibrating devices, and StopDeviceCmd should be supported by all
devices in order to stop them from whatever their current action may
be.

---
## StopDeviceCmd

**Description:** Client request to have the server stop a device from
whatever actions it may be taking. This message should be supported by
all devices, and the server should know how to stop any device it
supports.

**Introduced In Spec Version:** 0

**Last Updated In Spec Version:** 0

**Fields:**

* _Id_ (unsigned int): Message Id
* _DeviceIndex_ (unsigned int): Index of device to stop.

**Expected Response:**

* Ok message with matching Id on successful request.
* Error message on value or message error.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>+Server: StopDeviceCmd Id=1
    Server->>-Client: Ok Id=1
</mermaid>

**Serialization Example:**

```json
[
  {
    "StopDeviceCmd": {
      "Id": 1,
      "DeviceIndex": 0
    }
  }
]
```
---
## StopAllDevices

**Description:** Sent by the client to tell the server to stop all
devices. Can be used for emergency situations, on client shutdown for
cleanup, etc… While this is considered a Device Message, since it
pertains to all currently connected devices, it does not specify a
device index (and does not end with 'Cmd').

**Introduced In Spec Version:** 0

**Last Updated In Spec Version:** 0

**Fields:**

* _Id_ (unsigned int): Message Id

**Expected Response:**

* Ok message with matching Id on successful request.
* Error message on value or message error.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>+Server: StopAllDevices Id=1
    Server->>-Client: Ok Id=1
</mermaid>

**Serialization Example:**

```json
[
  {
    "StopAllDevices": {
      "Id": 1
    }
  }
]
```
---
## VibrateCmd

**Description:** Causes a device that supports vibration to run
specific vibration motors at a certain speeds. Devices with multiple
vibrator features may take multiple values. The
[FeatureCount](enumeration.md#messageattributes) attribute for the
message in the
[DeviceList](enumeration.md#devicelist)/[DeviceAdded](enumeration.md#deviceadded)
message will contain that information.

**Introduced In Spec Version:** 1

**Last Updated In Spec Version:** 1

**Fields:**

* _Id_ (unsigned int): Message Id
* _DeviceIndex_ (unsigned int): Index of device
* _Speeds_ (array): Vibration speeds
  * _Index_ (unsigned int): Index of vibration motor
  * _Speed_ (double): Vibration speed with a range of [0.0-1.0]

**Expected Response:**

* Ok message with matching Id on successful request.
* Error message on value or message error.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>+Server: VibrateCmd Id=1
    Server->>-Client: Ok Id=1
</mermaid>

**Serialization Example:**

```json
[
  {
    "VibrateCmd": {
      "Id": 1,
      "DeviceIndex": 0,
      "Speeds": [
        {
          "Index": 0,
          "Speed": 0.5
        },
        {
          "Index": 1,
          "Speed": 1.0
        }
      ]
    }
  }
]
```
---
## LinearCmd

**Description:** Causes a device that supports linear movement to move
to a position over a certain amount of time. Devices with multiple
linear actuator features may take multiple values. The
[FeatureCount](enumeration.md#messageattributes) attribute for the
message in the
[DeviceList](enumeration.md#devicelist)/[DeviceAdded](enumeration.md#deviceadded)
message will contain that information.

**Introduced In Spec Version:** 1

**Last Updated In Spec Version:** 1

**Fields:**

* _Id_ (unsigned int): Message Id
* _DeviceIndex_ (unsigned int): Index of device
* _Vectors_ (array): Linear actuator speeds and positions
  * _Index_ (unsigned int): Index of linear actuator
  * _Duration_ (unsigned int): Movement time in milliseconds
  * _Position_ (double): Target position with a range of [0.0-1.0]

**Expected Response:**

* Ok message with matching Id on successful request.
* Error message on value or message error.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>+Server: LinearCmd Id=1
    Server->>-Client: Ok Id=1
</mermaid>

**Serialization Example:**

```json
[
  {
    "LinearCmd": {
      "Id": 1,
      "DeviceIndex": 0,
      "Vectors": [
        {
          "Index": 0,
          "Duration": 500,
          "Position": 0.3
        },
        {
          "Index": 1,
          "Duration": 1000,
          "Position": 0.8
        }
      ]
    }
  }
]
```
---
## RotateCmd

**Description:** Causes a device that supports rotation to rotate at a
certain speeds in specified directions. Devices with multiple rotating
features may have multiple values. The
[FeatureCount](enumeration.md#messageattributes) attribute for the
message in the
[DeviceList](enumeration.md#devicelist)/[DeviceAdded](enumeration.md#deviceadded)
message will have this information.

**Introduced In Spec Version:** 1

**Last Updated In Spec Version:** 1

**Fields:**

* _Id_ (unsigned int): Message Id
* _DeviceIndex_ (unsigned int): Index of device
* _Rotations_ (array): Rotation speeds
  * _Index_ (unsigned int): Index of rotation motor
  * _Speed_ (double): Rotation speed with a range of [0.0-1.0]
  * _Clockwise_ (boolean): Direction of rotation (clockwise may be subjective)

**Expected Response:**

* Ok message with matching Id on successful request.
* Error message on value or message error.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>+Server: RotateCmd Id=1
    Server->>-Client: Ok Id=1
</mermaid>

**Serialization Example:**

```json
[
  {
    "RotateCmd": {
      "Id": 1,
      "DeviceIndex": 0,
      "Rotations": [
        {
          "Index": 0,
          "Speed": 0.5,
          "Clockwise": true
        },
        {
          "Index": 1,
          "Speed": 1.0,
          "Clockwise": false
        }
      ]
    }
  }
]
```
---
## ShockCmd

**Description:** Causes a device that supports shocks/estim to deliver
a static shock at a certain strength for a certain period of time. The
attributes for this device in
[DeviceList](enumeration.md#devicelist)/[DeviceAdded](enumeration.md#deviceadded)
message will have the following information:

- FeatureCount: Number of shock channels available on device (i.e.
  usually one on a collar, 2 on an ET-312, etc...)
- StepCount: Number of shock strength levels per channel

**Introduced In Spec Version:** 2

**Last Updated In Spec Version:** 2

**Fields:**

* _Id_ (unsigned int): Message Id
* _DeviceIndex_ (unsigned int): Index of device
* _Shocks_ (array): Shock strength/duration
  * _Index_ (unsigned int): Index of rotation motor
  * _Strength_ (double): Shock strength with a range of [0.0-1.0]
  * _Duration_ (unsigned int): Duration of shock, in milliseconds.

**Expected Response:**

* Ok message with matching Id on successful request.
* Error message on value or message error.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>+Server: ShockCmd Id=1
    Server->>-Client: Ok Id=1
</mermaid>

**Serialization Example:**

```json
[
  {
    "ShockCmd": {
      "Id": 1,
      "DeviceIndex": 0,
      "Shocks": [
        {
          "Index": 0,
          "Strength": 0.5,
          "Duration": 500
        }
      ]
    }
  }
]
```
---
## PatternCmd

**Description:** Causes a device that supports pattern playback to
start playing a pattern. The attributes for this device in
[DeviceList](enumeration.md#devicelist)/[DeviceAdded](enumeration.md#deviceadded)
message will have the following information:

- _Patterns_ (list of strings): Names of patterns that can be played on the device

**Introduced In Spec Version:** 2

**Last Updated In Spec Version:** 2

**Fields:**

* _Id_ (unsigned int): Message Id
* _DeviceIndex_ (unsigned int): Index of device
* _Pattern_ (string): Pattern to start playing

**Expected Response:**

* Ok message with matching Id on successful request.
* Error message on value or message error.

**Flow Diagram:**

<mermaid>
sequenceDiagram
    Client->>+Server: PatternCmd Id=1
    Server->>-Client: Ok Id=1
</mermaid>

**Serialization Example:**

```json
[
  {
    "PatternCmd": {
      "Id": 1,
      "DeviceIndex": 0,
      "Pattern": "wave"
    }
  }
]
```
