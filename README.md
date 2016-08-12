# ESPEasy
### FHEM Module To Control ESPEasy

To bind this module into FHEM update service use the FHEM following commands:
* `update add https://raw.githubusercontent.com/ddtlabs/ESPEasy/develop/controls_ESPEasy.txt`
* `update` 

To remove this module from FHEM update service use the FHEM following command:
* `update delete https://raw.githubusercontent.com/ddtlabs/ESPEasy/develop/controls_ESPEasy.txt`

To install only once (no automatic updates via FHEM update command):
* `update all https://raw.githubusercontent.com/ddtlabs/ESPEasy/develop/controls_ESPEasy.txt`

Or just download the module and copy it to your FHEM-Modul folder.

More information about FHEM update can be found here:
[FHEMWIKI](http://www.fhemwiki.de/wiki/Update) [FHEM command reference](http://fhem.de/commandref.html#update)


### Release Notes:
```
0.1   - public release
0.1.1 - added internal timer to poll GPIO status
      - added attribut interval
      - added attribut pollGPIOs
      - improved logging
      - added esp command "status"
      - added statusRequest
      - commands are case insensitive, now
      - updated command reference
      - delete unknown readings
0.1.2 - renamed attribut interval to Interval
      - presence check
      - added statusRequest cmd
      - added forgotten longpulse command
0.1.3 - added internal VERSION
      - moved internal URLCMD to $hash->{helper}
      - added pin mapping for Wemos D1 mini, NodeMCU, ... 
      - within set commands
      - added state mapping (on->1 off->0) within all set commands
      - added set command "clearReadings" (GPIO readings will be wiped out)
      - added get command "pinMap" (displays pin mapping)
      - show usage if there are too few arguments
      - command reference adopted

0.2.0 - chanched module design to bridge/device version
0.2.1 - own tcp port (default 8383) for communication from esp to fhem
      - added basic authentication for incoming requests
      - added attribut readingPrefixGPIO
      - added attribut readingSuffixGPIOState
0.2.2 - fixed statusRequest/presentCheck
      - minor fixes: copy/paste errors...
      - approved logging to better fit dev guide lines
      - handle renaming of devices
      - commands are case sensitive again, sorry :(
0.2.3 - released
      
```

#Command Reference#

<a name="ESPEasy"></a>
<h3>ESPEasy</h3>
<ul>
  <p>
    Provides control to ESP8266/ESPEasy
  </p>
  <b>Notes</b>
  <ul>
    <li>You have to define a bridge device before any logical device can be
      defined.
      </li>
    <li>You have to configure your ESP to use "FHEM HTTP" controller protocol
      (available in ESPEasy R109+). Furthermore the ESP controller port and the
      FHEM ESPEasy bridge port must be the same, of cause.
      </li><br>
    <li>Requirements: perl module <b>JSON</b> lately.<br>
      Use "cpan install JSON" or operating system's package manager to install
      Perl JSON Modul. Depending on your os the required package is named: 
      libjson-perl or perl-JSON.
      </li>
  </ul>
<br>
  <a name="ESPEasydefine"></a>
  <b>Define </b>(bridge device)<br>
<br>
  <ul>

  <code>define &lt;name&gt; ESPEasy bridge &lt;port&gt;
  </code>
  <br><br>

  <ul>
  <code>&lt;name&gt;</code>
  <ul>Specifies a device name of your choise.<br>
  eg. <code>ESPBridge</code>
  </ul><br>
  </ul>

  <ul>
  <code>&lt;port&gt;</code>
  <ul>Specifies tcp port for incoming http requests. This port must <u>not</u> be used
  by any other application or daemon on your system and must be in the range
  1025..65535<br>
  eg. <code>8383</code><br>
  </ul>

  </ul>

    <p><u>Define Examples:</u></p>
    <ul>
      <li><code>define ESPBridge ESPEasy bridge 8383</code></li>
    </ul>
  </ul>
<br>

  <a name="ESPEasyget"></a>
  <b>Get </b>(bridge device)<br>
<br>
  <ul>
    <li>&lt;reading&gt;<br>
      returns the value of the specified reading<br>
      </li><br>
  </ul>




  <a name="ESPEasyset"></a>
  <b>Set </b>(bridge device)<br>
<br>
  <ul>
<li>user<br>
Specifies username used by basic authentication for incoming requests.
<br>
required value: <code>&lt;username&gt;</code><br>
eg. : <code>set ESPBridge user itsme</code><br>
</li>
<br>

<li>pass<br>
Specifies password used by basic authentication for incoming requests.
<br>
required value: <code>&lt;password&gt;</code><br>
eg. : <code>set ESPBridge pass secretpass</code><br>
</li>
<br>


  </ul>

 <a name="ESPEasyattr"></a>
  <b>Attributes </b>(bridge device)<br><br>
  <ul>
    <li>authentication<br>
      Used to enable basic authentication for incoming requests<br>
      Note that user, pass and attribut authentication must be set to activate
      basic authentication<br>
      Possible values: 0,1<br>
      </li><br>
    <li>autocreate<br>
      Used to overwrite global autocreate setting<br>
      Possible values: 0|1<br>
      Default: 1
      </li><br>
    <li>disable<br>
      Used to disable device<br>
      Possible values: 0,1<br>
      </li><br>
  </ul>

<br>
<hr>
<br>

  <a name="ESPEasydefine"></a>
  <b>Define </b>(logical device)<br><br>
  <ul>
  Note: logical devices will be created automatically if any values are received
  by the bridge device and autocreate is not disabled. If you configured your
  ESP in a way that no data is send independently then you have to define
  logical devices. At least wifi rssi value could be defined to use autocreate.
  
  <br><br>
  
  <code>define &lt;name&gt; ESPEasy &lt;ip|fqdn&gt; &lt;port&gt;
    &lt;IODev&gt; &lt;identifier&gt; 
  </code>
  <br><br>

  <ul>
  <code>&lt;name&gt;</code>
  <ul>Specifies a device name of your choise.<br>
  eg. <code>ESPxx</code>
  </ul><br>
  <code>&lt;ip|fqdn&gt;</code>
  <ul>Specifies ESP IP address or hostname.<br>
    eg. <code>172.16.4.100</code><br>
    eg. <code>espxx.your.domain.net</code>
  </ul>
  </ul>
<br>
  <ul>
  <code>&lt;port&gt;</code>
  <ul>Specifies http port to be used for outgoing request to your ESP. Should
    be: 80<br>
    eg. <code>80</code><br>
  </ul>
  </ul>
<br>
  <ul>
  <code>&lt;IODev&gt;</code>
  <ul>Specifies your ESP bridge device. See above.
    <br>
    eg. <code>ESPBridge</code><br>
  </ul>
  </ul>
<br>
  <ul>
  <code>&lt;identifier&gt;</code>
  <ul>Specifies an identifier that will bind your ESP to this device. Must be
    identical with ESP name (ESP GUI -&gt; Config -&gt; Main Settings -&gt; Name)
    <br>
    eg. <code>ESPxx</code><br>
  </ul>
  </ul>

    <p><u>Define Examples:</u></p>
    <ul>
      <li><code>define ESPxx ESPEasy 172.16.4.100 80 ESPBridge ESPxx</code></li>
    </ul>
  </ul>
<br>

  <a name="ESPEasyget"></a>
  <b>Get </b>
  <ul>
    <li>&lt;reading&gt;<br>
      returns the value of the specified reading<br>
      </li><br>

    <li>pinMap<br>
      returns possible alternative pin names that can be used in commands<br>
      </li><br>
  </ul>

<br>

  <a name="ESPEasyset"></a>
  <b>Set </b>
<br>
  <ul>
Notes:<br>
- Commands are case insensitive.<br>
- Users of Wemos D1 mini or NodeMCU can use Arduino pin names instead of GPIO 
no: D1 =&gt; GPIO5, D2 =&gt; GPIO4, ...,TX =&gt; GPIO1 (see: get pinMap)<br>
- low/high state can be written as 0/1 or on/off
<br><br>

<li>Event<br>
Create an event 
<br>
required value: <code>&lt;string&gt;</code><br>
</li>
<br>

<li>GPIO<br>
Direct control of output pins (on/off)
<br>
required arguments: <code>&lt;pin&gt; &lt;0|1&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/GPIO">ESPEasy:GPIO</a> for details
<br>
</li>
<br>

<li>PWM<br>
Direct PWM control of output pins 
<br>
required arguments: <code>&lt;pin&gt; &lt;level&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/GPIO">ESPEasy:GPIO</a> for details
<br>
</li>
<br>

<li>PWMFADE<br>
PWMFADE control of output pins 
<br>
required arguments: <code>&lt;pin&gt; &lt;target&gt; &lt;duration&gt;</code><br>
pin: 0-3 (0=r,1=g,2=b,3=w), target: 0-1023, duration: 1-30 seconds.
<br>
</li>
<br>

<li>Pulse<br>
Direct pulse control of output pins
<br>
required arguments: <code>&lt;pin&gt; &lt;0|1&gt; &lt;duration&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/GPIO">ESPEasy:GPIO</a> for details
<br>
</li>
<br>

<li>LongPulse<br>
Direct pulse control of output pins
<br>
required arguments: <code>&lt;pin&gt; &lt;0|1&gt; &lt;duration&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/GPIO">ESPEasy:GPIO</a> for details
<br>
</li>
<br>

<li>Servo<br>
Direct control of servo motors
<br>
required arguments: <code>&lt;servoNo&gt; &lt;pin&gt; &lt;position&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/GPIO">ESPEasy:GPIO</a> for details
<br>
</li>
<br>

<li>lcd<br>
Write text messages to LCD screen
<br>
required arguments: <code>&lt;row&gt; &lt;col&gt; &lt;text&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/LCDDisplay">ESPEasy:LCDDisplay</a> 
for details
<br>
</li>
<br>

<li>lcdcmd<br>
Control LCD screen
<br>
required arguments: <code>&lt;on|off|clear&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/LCDDisplay">ESPEasy:LCDDisplay</a> 
for details
<br>
</li>
<br>

<li>mcpgpio<br>
Control MCP23017 output pins
<br>
required arguments: <code>&lt;pin&gt; &lt;0|1&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/MCP23017">ESPEasy:MCP23017</a> 
for details
<br>
</li>
<br>

<li>oled<br>
Write text messages to OLED screen
<br>
required arguments: <code>&lt;row&gt; &lt;col&gt; &lt;text&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/OLEDDisplay">ESPEasy:OLEDDisplay</a>
<br>
</li>
<br>

<li>oledcmd<br>
Control OLED screen
<br>
required arguments: <code>&lt;on|off|clear&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/OLEDDisplay">ESPEasy:OLEDDisplay</a>
<br>
</li>
<br>

<li>pcapwm<br>
Control PCA9685 pwm pins 
<br>
required arguments: <code>&lt;pin&gt; &lt;level&gt;</code><br>
see <a href="http://www.esp8266.nu/index.php/PCA9685">ESPEasy:PCA9685</a>
<br>
</li>
<br>

<li>PCFLongPulse<br>
Long pulse control on PCF8574 output pins
<br>
see <a href="http://www.esp8266.nu/index.php/PCF8574">ESPEasy:PCF8574</a> for 
details
<br>
</li>
<br>

<li>PCFPulse<br>
Pulse control on PCF8574 output pins 
<br>
see <a href="http://www.esp8266.nu/index.php/PCF8574">ESPEasy:PCF8574</a> for 
details
<br>
</li>
<br>

<li>pcfgpio<br>
Control PCF8574 output pins
<br>
see <a href="http://www.esp8266.nu/index.php/PCF8574">ESPEasy:PCF8574</a>
<br>
</li>
<br>

<li>raw<br>
Can be used for own ESP plugins that are not considered at the moment.
<br>
Usage: raw &lt;cmd&gt; &lt;param1&gt; &lt;param2&gt; &lt;...&gt;<br>
eg: raw myCommand 3 1 2
</li>
<br>

<li>status<br>
Request esp device status (eg. gpio)
<br>
required values: <code>&lt;device&gt; &lt;pin&gt;</code><br>
eg: <code>&lt;gpio&gt; &lt;13&gt;</code><br>
</li>
<br>

<li>help<br>
Shows set command usage
<br>
required values: <code>
Event|GPIO|PCFLongPulse|PCFPulse|PWM|Publish|Pulse|Servo|Status|lcd|lcdcmd|
mcpgpio|oled|oledcmd|pcapwm|pcfgpio|status|statusRequest|clearReadings|help
</code><br>
</li>
<br>

<li>statusRequest<br>
Trigger a statusRequest for configured GPIOs (see attribut pollGPIOs) and a 
presenceCheck
<br>
required values: <code>&lt;none&gt;</code><br>
</li>
<br>

<li>clearReadings<br>
Delete all GPIO.* readings
<br>
required values: <code>&lt;none&gt;</code><br>
</li>
<br>

  </ul>

<br>

 <a name="ESPEasyattr"></a>
  <b>Attributes</b>
  <ul>
    <li>disable<br>
      Used to disable device<br>
      Possible values: 0,1<br>
      </li><br>
    <li>pollGPIOs<br>
      Used to enable polling of GPIOs status<br>
      Possible values: comma separated GPIO number list<br>
      Eg. <code>13,15</code>
      </li><br>
    <li>Interval<br>
      Used to set polling interval of GPIOs in seconds<br>
      Possible values: secs &gt; 10<br>
      Default: 300
      Eg. <code>300</code>
      </li><br>
    <li>autosave<br>
      Used to overwrite global autosave setting<br>
      Possible values: 0|1<br>
      Default: 1
      </li><br>
    <li>readingPrefixGPIO<br>
      Specifies a prefix for readings based on GPIO numbers. For example:
      "set ESPxx gpio 15 on" will switch GPIO15 on. Additionally, there is an
      ESP devices (type switch input) that reports the same GPIO but with a
      name instead of GPIO number. To get the reading names synchron you can
      use this attribute. Helpful if you use pulse or longpuls command.
      to be continued...
      <br>
      Default: GPIO
      </li><br>
    <li>readingSuffixGPIOState<br>
      to be done...
      <br>
      Default: no suffix
      </li><br>

  </ul>

</ul>
