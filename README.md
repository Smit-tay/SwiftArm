# SwiftArm

make test -

Use of the Swift Pro requires communication with a USB device.  This is OS specific.
Under modern linux, when the Swift Pro is plugged in the USB Abstract Control Model (ACM)
will "automagically" assign the device to /dev/ttyACMX, where X is a digit.
You can automatically create a symlink to /dev/swiftpro using the following
udev rule via a file named something like: /etc/udev/rules.d/99-swiftpro.rules

in that file you need only make an entry like this:

    SUBSYSTEM=="tty", ATTRS{idVendor}=="xxxx", ATTRS{idProduct}=="yyyy", SYMLINK+="swiftpro"

where xxxx and yyyy correspond to the Swift Pro -
    possibly: ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0042"

Make sure to reload the udev rules via:

    sudo udevadm control --reload-rules

Then reconnect the SwiftPro

Note, we have a single test target which is supposed to test everything (i.e. 'all')
defined in the CMakeLists.txt file.  This is why the Jenkinsfile only has to run:

    ctest -V
