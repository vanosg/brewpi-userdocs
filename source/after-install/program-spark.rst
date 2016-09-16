Programming your Spark with BrewPi
====================================
The serial port used by BrewPi is read from the config file (``/home/brewpi/settings/config.cfg``). In most cases, the default serial port is ``/dev/ttyACM0``. If typing ``ls /dev/ttyACM*`` shows ttyACM0 or ttyACM1, then you do not need to change anything. 

If BrewPi fails to start because it was unable to detect your serial port, you'll want to update the config file with the appropriate path. If this occurs, running ``ls /dev/ttyA*`` will reveal potential candidates for of these situations.

Note: It is definitely not ``ttyAMA0``, that is the internal serial port.
Arduino clones sometimes appear as ttyUSB0 or similar. If that is the case, add these lines to  ``/home/brewpi/settings/config.cfg``:

.. code-block:: bash

    port = /dev/ttyUSB0
    altport = /dev/ttyUSB1


Updating your Spark firmware
----------------------------------------

To program your Spark from the web interface, take the following steps:

#.  Run the update script from the brewpi-tools repo (usually installed to /home/brewpi/brewpi-tools):

    .. code-block:: bash

        python updater.py

    If the script asks you which branch you would like to check out, select the 'develop' branch.
    If the script asks you to switch to the master branch, select 'no'.

    When the script asks you if you wish to check your controller version firmware and update it with the most recent version from GitHub, select 'yes'. 

    <<Insert what to do if not up to date>>

#.  When the updater script asks you update the device, select 'no'. 

#.  If you haven't already, plug in your temperature sensors and connect the Spark to your Pi via a USB cable. Once the BrewPi Hardware Test screen loads, take note of the alphanumeric string that is listed above each temperature probe for later use- these are the OneWire addresses you will use the web UI to assign a role to each probe.

#.  Log on to your BrewPi web interface by entering the Raspberry Pi's IP address into a web browser.
    The IP is displayed when the installer finishes, or by typing ``ifconfig`` at the shell (look for inet addr).
    The LCD display in the upper left hand corner should display "Testing" when first loaded. If the page says 'script not running' after waiting up to 60 seconds, you should start the BrewPi script via the button on the right.
    This is automatically done by the CRON job within one minute if you click the 'start script' button.
    If you have not set up CRON yet, you can manually start the script with:

    .. code-block:: bash

        sudo -u brewpi python /home/brewpi/brewpi.py

#.  Open the maintenance panel by clicking the "Maintenance panel" button and go to the `Reprogram controller` tab.

#.  Download the HEX file appropriate for your setup from http://dl.brewpi.com/brewpi-avr/stable.
    You can also compile your own hex file with Atmel Studio.
    Make sure you have the right file for your Arduino type (UNO or Leonardo) and the right shield (Rev A or Rev C).
    Only the first 100 BrewPi shields were rev A. If you bought one recently, you have Rev C.

#.  The BrewPi script will automatically restart after programming and report which version of BrewPi it has found on the Arduino.


Troubleshooting
---------------
* If you're prompted with an error: "cannot move uploaded file" rerun the fix permissions script in ``sudo sh /home/brewpi/fixPermissions.sh``.
* If saving devices in the device manager does not work, your EEPROM was probably not reset properly. Try reprogramming without settings and devices restore.
