![banner](/Resources/banner.png)

# Buoy

**Buoy** is a simple iBeacon listener/manager that handles all of the hard stuff for you, matey!

## Requirements

* iOS 7+

## Instsallation

Use [Cocoapods](http://www.cocoapods.org) to install Buoy.

`pod 'Buoy'`

## Using

To begin, `#import <BUOYListener.h>` in the classes you want to use the Listener and it's notifications in. Buoy works by listening for iBeacons and broadcasting NSNotifications through the `[NSNotificationCenter defaultCenter]` so any of your classes can key in on these and do something with the data.

#### Set Up

Before you can get a notification from the BUOYListener, you need to set it up. You will need a list of the different `Proximity UUIDs` you want to listen for. These are specific to the iBeacons themselves, and you can configure your different iBeacons to have new UUIDs with most major kinds. Here's how you set up the Listener.

```objc
// AppDelegate.m

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    ...

    NSArray *uuids = @[[[NSUUID alloc] initWithUUIDString:@"00000000-0000-0000-0000-000000000000"],[NSUUID alloc] initWithUUIDString:@"11111111-1111-1111-1111-111111111111"]];
    [[BUOYListener defaultListener] listenForBeaconsWithProximityUUIDs:uuids];

    ...
}
```

#### Handle Finding iBeacons

When the Listener finds an iBeacon it will broadcast a notification with that beacon in the `notification userInfo` dictionary object. Found beacons are of the type `CLBeacon`.

```objc
// Set Up Notifications

[[NSNotificationCenter defaultCenter] addObserverForName:kBUOYDidFindBeaconNotification object:nil queue:nil usingBlock:^(NSNotification *note) {
        if (note.userInfo[kBUOYBeacon]) {
            CLBeacon *beacon = note.userInfo[kBUOYBeacon];
            // Do something here!
        }
}];
```

#### Using the iBeacon

Each `CLBeacon` object has a few properties that you can key off to handle internal logic specific for your app.

* `major` - NSNumber
* `minor` - NSNumber
* `proximityUUID` - NSUUID
* `accuracy` - CLLocationAccuracy (meters)
* `proximity` - CLProximity (enum)
* `rssi` - NSInteger (dB of signal strength)

#### Buoy CLBeacon Methods

Beyond these base properties, the included `CLBeacon+Buoy.{h,m}` category class has some included methods to make your life a little easier as well.

**Beacon Accuracy String**

```objc
CLBeacon *someBeacon;
NSString *accuracy = [someBeacon accuracyStringWithDistanceType:kBuoyDistanceTypeFeet];
```

This uses the following enum for `kBuoyDistanceType`

* `kBuoyDistanceTypeMeters`
* `kBuoyDistanceTypeFeet`
* `kBuoyDistanceTypeYards`

**Major/Minor**

```objc
NSString *M = [someBeacon majorString];
NSString *m = [someBeacon minorString];
```

## License

This project is licensed under the standard MIT License, which can be found [here](/LICENSE.md)