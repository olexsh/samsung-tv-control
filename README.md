# Samsung TV control

This is a simple Java library/API for controlling a Samsung Smart TV that listens on port 55000.
Samsung TV's built in 2014 and later use encryption and can therefore **not** be controlled by this library!

The protocol information has been gathered from here: http://sc0ty.pl/2012/02/samsung-tv-network-remote-control-protocol/

## Javadoc

Javadoc location: https://mhvis.github.io/samsung-tv-control/javadoc/

## Builds

The latest build can be found here: https://github.com/mhvis/samsung-tv-control/releases/latest

## Usage

See [javadoc](https://mhvis.github.io/samsung-tv-control/javadoc/), short example:

```java
try {
  InetAddress address = InetAddress.getByName("192.168.123.456");
  SamsungRemote remote = new SamsungRemote(address);
  TVReply reply = remote.authenticate("Toaster"); // Argument is the device name (displayed on television).
  if (reply == TVReply.ALLOWED) {
    remote.keycode("KEY_INFO");
  }
  remote.close();
} catch (IOException e) {
  System.err.println(e.getMessage());
}
```

You should already know the TV IP address. Then the following steps are needed and can be seen above:

1. `SamsungRemote remote = new SamsungRemote(InetAddress);` this opens a socket connection to the TV.
2. `TVReply reply = remote.authenticate("Friendly name");` this sends an authentication message. This will make the TV show a message asking the TV user to allow or deny the connection. The method blocks waiting for the reply. The reply is returned as TVReply and can be one of `ALLOWED`, `DENIED` or `TIMEOUT`.
3. When the reply is `ALLOWED`, you can send keycodes: `remote.keycode("KEY_INFO");` this method blocks and waits for a TV confirmation to check if it arrived. A list of key codes that can be send can be found [here](http://www.openremote.org/display/docs/OpenRemote+2.0+How+To+-+Samsung+TV+Remote) and is also listed in the Keycode enum in this package.
4. When finished, close the socket connection using `remote.close();`
