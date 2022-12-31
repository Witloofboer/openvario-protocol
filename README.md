# OpenVario Protocol

This document describes the OpenVario serial port protocol.

**Version: 1.4-larus**

## Specification

The OpenVario serial port protocol is built around the `$POV` NMEA sentence:

    $POV,<type>,<value>,<type>,<value>,...*<checksum>

The NMEA sentence always starts with `$POV` and ends up with a `*` and a `checksum`. Each `$POV` sentence can contain multiple datapoints defined by `type` and `value`. The `type` part is an uppercase letter describing the type and meaning of the following `value` according to the overview of supported types below. The value is a floating point number. The length of the sentence is not fixed, but terminated with the usual asterisk and a two character checksum.

The list of datapoints should be terminated with the usual asterisk and two character checksum.

## Supported Datapoints

The following section describes all datapoints supported by the current version of the OpenVario protocol.

### Airspeed

* `S`: true airspeed in `km/h`  
  Example: `$POV,S,123.45*05`

### Humidity
* `H`: relative humidity in `%`  
  Example: `$POV,H,58.42*24`

### Pressure

* `P`: static pressure in `hPa`  
  Example: `$POV,P,1018.35*39`

* `Q`: dynamic pressure in `Pa`  
  Example: `$POV,Q,23.3*04`

* `R`: total pressure in `hPa`  
  Example: `$POV,R,1025.17*35`

### Temperature

* `T`: temperature in `°C`  
  Example: `$POV,T,23.52*35`

### Voltage

* `V`: battery voltage in `volt`  
  Example: `$POV,V,12.3*01`

### Vario

* `E`: TE vario in `m/s`  
  Example: `$POV,E,2.15*14`
  
### Wind

* `Wis`: Instantaneous wind speed in `m/s`  
  Example: `$POV,Wis,10.3*18`

* `Wid`: Instantaneous wind direction (true) in `°`  
  Example: `$POV,Wid,243*26`
  
* `Was`: Average wind speed in `m/s`  
  Example: `$POV,Was,10.0*13`

* `Wad`: Average wind direction (true) in `°`  
  Example: `$POV,Wad,241*3E`
  
### Attitude

All attitude references are given with regard to a moving coordinate system that is attached to the glider, parallel to the earth's surface and with the ...-axis pointing in the direction of the horizontal movement.

* `R`: Roll angle referenced to earth's horizontal plane in `degrees`  
  Example: `$POV,R,33.23*34`

* `P`: Pitch angle referenced to earth's horizontal plane in `degrees`  
  Example: `$POV,P,10.12*35`
  
* `Y`: Yaw angle referenced to the direction of motion in `degrees`  
  Example: `$POV,Y,-12.89*11`

## Commands

`C`: Command followed by type of command and parameter if necessary
  Example: `C,<type>,<parameter>`
  
Commands are grouped in "Types":

* Vario:  
    `VU`: Volume of external vario up by 10%   
    `VD`: Volume of external vario down by 10%  
    `VM`: Mute external Vario  
    Example: `$POV,C,VU*09` => Set up volume by 10%  
    
* McCready:  
     `MC,<value>`  
     Example: `$POV,C,MC,+0.5*28` => Set McCready to +0.5  
     
* Wing Load:  
     `WL,<value>`  
     `value` is a factor of the additional weight added by water ballast in relation to the reference weight of the glider  
     Example: `$POV,C,WL,1.0*12` => No water ballast  
              `$POV,C,WL,1.1*13` => 10% more weight than reference weight       
   
* Bugs:
     `BU,<value>`  
     `value` reflects the value of bugs  
     Example: `$POV,C,BU,1.0*1E` => No Bugs  
              `$POV,C,WL,0.5*16` => 50% Bugs      
   
* Real Polar:  
     `RPO,<coff a>, <coff b>. <coff c>`  
     `coff x` are reflecting the coefficients of the polar including bugs and ballast  
     Example: 
     
* Ideal Polar:  
     `IPO,<coff a>, <coff b>. <coff c>`  
     `coff x` are reflecting the coefficients of the ideal polar (just glider, without bugs and ballast)  
     Example:  
     
## Checksum

The NMEA Checksum is calculated over the NMEA string between `$` and `*`. The checksum is a hexadecimal number representing the XOR of all bytes.
