A few small programs to set the LCD 16x2 and LEDs using wiringPi for Odroid. Use arguments.

Requires user-access to gpio. Make udev rules if you still have to use sudo:
```
groupadd -f -r gpio
usermod -G gpio alarm
cd /etc/udev/rules.d/
touch 99-gpio.rules
```

Then add following content to 99-gpio.rules:
```
SUBSYSTEM=="meson-gpiomem", KERNEL=="gpiomem", GROUP="gpio", MODE="0660"
SUBSYSTEM=="gpio", KERNEL=="gpiochip*", ACTION=="add", PROGRAM="/bin/sh -c 'chown root:gpio /sys/class/gpio/export /sys/class/gpio/unexport ; chmod 220 /sys/class/gpio/export /sys/class/gpio/unexport'"
SUBSYSTEM=="gpio", KERNEL=="gpio*", ACTION=="add", PROGRAM="/bin/sh -c 'chown root:gpio /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value ; chmod 660 /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value'"
```

Then reboot. You should now be able to access gpio without sudo.
