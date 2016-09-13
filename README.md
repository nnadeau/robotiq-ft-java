[![Build Status](https://travis-ci.org/nnadeau/robotiq-ft-java.svg?branch=master)](https://travis-ci.org/nnadeau/robotiq-ft-java)
[![Coverage Status](https://coveralls.io/repos/github/nnadeau/robotiq-ft-java/badge.svg?branch=master)](https://coveralls.io/github/nnadeau/robotiq-ft-java?branch=master)

# robotiq-ft-java
A Java package for cross-platform serial communication with a Robotiq FT Sensor

## Communication Protocol
The communication protocol (and basis for `config.properties`) for using a FT Sensor can be found [here](http://support.robotiq.com/pages/viewpage.action?pageId=9601256).

## Usage
### Tested Platforms
- Windows 10 x64
- OSX 10.11.5 x64

### Quick Tests
- Run `SerialUtilities.java` to list available serial ports (sensor should be connected and seen here)
- Update `comm_port` in `config.properties` to the proper serial port (e.g., COM3 for Windows, cu.usbserial-FTXU0M1B for OSX)
- Run `RobotiqFt.java` for a 20 second burst of data printed to the console 

### Example
```java
public static void main(String[] args) {
    Logger.info("Running simple RobotiqFT connection test");
    RobotiqFt robotiqFt = new RobotiqFt();
    try {
        robotiqFt.connect();

        int maxTime = 20;
        Logger.info(String.format("Reading from sensor for %d seconds", maxTime));
        long startTime = System.currentTimeMillis();
        while ((System.currentTimeMillis() - startTime) < (maxTime * 1000)) {
            try {
                System.out.println(Arrays.toString(robotiqFt.getCompleteMeasure()));
            } catch (ModbusException e) {
                Logger.error(e);
            }
        }
    } catch (Exception e) {
        Logger.error(e);
    }
}
```
