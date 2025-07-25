# rtl_433

rtl_433 (despite the name) is a generic data receiver, mainly for the 433.92 MHz, 868 MHz (SRD), 315 MHz, 345 MHz, and 915 MHz ISM bands.

The official source code is in the https://github.com/merbanan/rtl_433/ repository.
For more documentation and related projects see the https://triq.org/ site.

It works with [RTL-SDR](https://github.com/osmocom/rtl-sdr/) and/or [SoapySDR](https://github.com/pothosware/SoapySDR/).
Actively tested and supported are Realtek RTL2832 based DVB dongles (using RTL-SDR) and LimeSDR ([LimeSDR USB](https://www.crowdsupply.com/lime-micro/limesdr) and [LimeSDR mini](https://www.crowdsupply.com/lime-micro/limesdr-mini) engineering samples kindly provided by [MyriadRf](https://myriadrf.org/)), PlutoSDR, HackRF One (using SoapySDR drivers), as well as SoapyRemote.

![rtl_433 screenshot](./docs/screenshot.png)

## Building / Installation

rtl_433 is written in portable C (C99 standard) and known to compile on Linux (also embedded), MacOS, and Windows systems.
Older compilers and toolchains are supported as a key-goal.
Low resource consumption and very few dependencies allow rtl_433 to run on embedded hardware like (repurposed) routers.
Systems with 32-bit i686 and 64-bit x86-64 as well as (embedded) ARM, like the Raspberry Pi and PlutoSDR are well supported.

See [BUILDING.md](docs/BUILDING.md)

On Debian (sid) or Ubuntu (19.10+), `apt-get install rtl-433` for other distros check https://repology.org/project/rtl-433/versions

On FreeBSD, `pkg install rtl-433`.

On MacOS, `brew install rtl_433`.

Docker images with rtl_433 are available [on the github page of hertzg](https://github.com/hertzg/rtl_433_docker).

## How to add support for unsupported sensors

See [CONTRIBUTING.md](./docs/CONTRIBUTING.md).

## Running

    rtl_433 -h

```

  A "rtl_433.conf" file is searched in "./", XDG_CONFIG_HOME e.g. "$HOME/.config/rtl_433/",
  SYSCONFDIR e.g. "/usr/local/etc/rtl_433/", then command line args will be parsed in order.
		= General options =
  [-V] Output the version string and exit
  [-v] Increase verbosity (can be used multiple times).
       -v : verbose notice, -vv : verbose info, -vvv : debug, -vvvv : trace.
  [-c <path>] Read config options from a file
		= Tuner options =
  [-d <RTL-SDR USB device index> | :<RTL-SDR USB device serial> | <SoapySDR device query> | rtl_tcp | help]
  [-g <gain> | help] (default: auto)
  [-t <settings>] apply a list of keyword=value settings to the SDR device
       e.g. for SoapySDR -t "antenna=A,bandwidth=4.5M,rfnotch_ctrl=false"
       for RTL-SDR use "direct_samp[=1]", "offset_tune[=1]", "digital_agc[=1]", "biastee[=1]"
  [-f <frequency>] Receive frequency(s) (default: 433920000 Hz)
  [-H <seconds>] Hop interval for polling of multiple frequencies (default: 600 seconds)
  [-p <ppm_error>] Correct rtl-sdr tuner frequency offset error (default: 0)
  [-s <sample rate>] Set sample rate (default: 250000 Hz)
  [-D quit | restart | pause | manual] Input device run mode options (default: quit).
		= Demodulator options =
  [-R <device> | help] Enable only the specified device decoding protocol (can be used multiple times)
       Specify a negative number to disable a device decoding protocol (can be used multiple times)
  [-X <spec> | help] Add a general purpose decoder (prepend -R 0 to disable all decoders)
  [-Y auto | classic | minmax] FSK pulse detector mode.
  [-Y level=<dB level>] Manual detection level used to determine pulses (-1.0 to -30.0) (0=auto).
  [-Y minlevel=<dB level>] Manual minimum detection level used to determine pulses (-1.0 to -99.0).
  [-Y minsnr=<dB level>] Minimum SNR to determine pulses (1.0 to 99.0).
  [-Y autolevel] Set minlevel automatically based on average estimated noise.
  [-Y squelch] Skip frames below estimated noise level to reduce cpu load.
  [-Y ampest | magest] Choose amplitude or magnitude level estimator.
		= Analyze/Debug options =
  [-A] Pulse Analyzer. Enable pulse analysis and decode attempt.
       Disable all decoders with -R 0 if you want analyzer output only.
  [-y <code>] Verify decoding of demodulated test data (e.g. "{25}fb2dd58") with enabled devices
		= File I/O options =
  [-S none | all | unknown | known] Signal auto save. Creates one file per signal.
       Note: Saves raw I/Q samples (uint8 pcm, 2 channel). Preferred mode for generating test files.
  [-r <filename> | help] Read data from input file instead of a receiver
  [-w <filename> | help] Save data stream to output file (a '-' dumps samples to stdout)
  [-W <filename> | help] Save data stream to output file, overwrite existing file
		= Data output options =
  [-F log | kv | json | csv | mqtt | influx | syslog | trigger | rtl_tcp | http | null | help] Produce decoded output in given format.
       Append output to file with :<filename> (e.g. -F csv:log.csv), defaults to stdout.
       Specify host/port for syslog with e.g. -F syslog:127.0.0.1:1514
  [-M time[:<options>] | protocol | level | noise[:<secs>] | stats | bits | help] Add various meta data to each output.
  [-K FILE | PATH | <tag> | <key>=<tag>] Add an expanded token or fixed tag to every output line.
  [-C native | si | customary] Convert units in decoded output.
  [-n <value>] Specify number of samples to take (each sample is an I/Q pair)
  [-T <seconds>] Specify number of seconds to run, also 12:34 or 1h23m45s
  [-E hop | quit] Hop/Quit after outputting successful event(s)
  [-h] Output this usage help and exit
       Use -d, -g, -R, -X, -F, -M, -r, -w, or -W without argument for more help



		= Supported device protocols =
    [01]  Silvercrest Remote Control
    [02]  Rubicson, TFA 30.3197 or InFactory PT-310 Temperature Sensor
    [03]  Prologue, FreeTec NC-7104, NC-7159-675 temperature sensor
    [04]  Waveman Switch Transmitter
    [06]* ELV EM 1000
    [07]* ELV WS 2000
    [08]  LaCrosse TX Temperature / Humidity Sensor
    [10]  Acurite 896 Rain Gauge
    [11]  Acurite 609TXC Temperature and Humidity Sensor
    [12]  Oregon Scientific Weather Sensor
    [13]* Mebus 433
    [14]* Intertechno 433
    [15]  KlikAanKlikUit Wireless Switch
    [16]  AlectoV1 Weather Sensor (Alecto WS3500 WS4500 Ventus W155/W044 Oregon)
    [17]  Cardin S466-TX2
    [18]  Fine Offset Electronics, WH2, WH5, Telldus Temperature/Humidity/Rain Sensor
    [19]  Nexus, FreeTec NC-7345, NX-3980, Solight TE82S, TFA 30.3209 temperature/humidity sensor
    [20]  Ambient Weather F007TH, TFA 30.3208.02, SwitchDocLabs F016TH temperature sensor
    [21]  Calibeur RF-104 Sensor
    [22]  X10 RF
    [23]  DSC Security Contact
    [24]* Brennenstuhl RCS 2044
    [25]  Globaltronics GT-WT-02 Sensor
    [26]  Danfoss CFR Thermostat
    [29]  Chuango Security Technology
    [30]  Generic Remote SC226x EV1527
    [31]  TFA-Twin-Plus-30.3049, Conrad KW9010, Ea2 BL999
    [32]  Fine Offset Electronics WH1080/WH3080 Weather Station
    [33]  WT450, WT260H, WT405H
    [34]  LaCrosse WS-2310 / WS-3600 Weather Station
    [35]  Esperanza EWS
    [36]  Efergy e2 classic
    [37]* Inovalley kw9015b, TFA Dostmann 30.3161 (Rain and temperature sensor)
    [38]  Generic temperature sensor 1
    [39]  WG-PB12V1 Temperature Sensor
    [40]  Acurite 592TXR temp/humidity, 592TX temp, 5n1, 3n1, Atlas weather station, 515 fridge/freezer, 6045 lightning, 899 rain, 1190/1192 leak
    [41]  Acurite 986 Refrigerator / Freezer Thermometer
    [42]  HIDEKI TS04 Temperature, Humidity, Wind and Rain Sensor
    [43]  Watchman Sonic / Apollo Ultrasonic / Beckett Rocket oil tank monitor
    [44]  CurrentCost Current Sensor
    [45]  emonTx OpenEnergyMonitor
    [46]  HT680 Remote control
    [47]  Conrad S3318P, FreeTec NC-5849-913 temperature humidity sensor, ORIA WA50 ST389 temperature sensor
    [48]* Akhan 100F14 remote keyless entry
    [49]  Quhwa
    [50]  OSv1 Temperature Sensor
    [51]  Proove / Nexa / KlikAanKlikUit Wireless Switch
    [52]  Bresser Thermo-/Hygro-Sensor 3CH
    [53]  Springfield Temperature and Soil Moisture
    [54]  Oregon Scientific SL109H Remote Thermal Hygro Sensor
    [55]  Acurite 606TX / Technoline TX960 Temperature Sensor
    [56]  TFA pool temperature sensor
    [57]  Kedsum Temperature & Humidity Sensor, Pearl NC-7415
    [58]  Blyss DC5-UK-WH
    [59]  Steelmate TPMS
    [60]  Schrader TPMS
    [61]* LightwaveRF
    [62]* Elro DB286A Doorbell
    [63]  Efergy Optical
    [64]* Honda Car Key
    [67]  Radiohead ASK
    [68]  Kerui PIR / Contact Sensor
    [69]  Fine Offset WH1050 Weather Station
    [70]  Honeywell Door/Window Sensor, 2Gig DW10/DW11, RE208 repeater
    [71]  Maverick ET-732/733 BBQ Sensor
    [72]* RF-tech
    [73]  LaCrosse TX141-Bv2, TX141TH-Bv2, TX141-Bv3, TX141W, TX145wsdth, (TFA, ORIA) sensor
    [74]  Acurite 00275rm,00276rm Temp/Humidity with optional probe
    [75]  LaCrosse TX35DTH-IT, TFA Dostmann 30.3155 Temperature/Humidity sensor
    [76]  LaCrosse TX29IT, TFA Dostmann 30.3159.IT Temperature sensor
    [77]  Vaillant calorMatic VRT340f Central Heating Control
    [78]  Fine Offset Electronics, WH25, WH32, WH32B, WN32B, WH24, WH65B, HP1000, Misol WS2320 Temperature/Humidity/Pressure Sensor
    [79]  Fine Offset Electronics, WH0530 Temperature/Rain Sensor
    [80]  IBIS beacon
    [81]  Oil Ultrasonic STANDARD FSK
    [82]  Citroen TPMS
    [83]  Oil Ultrasonic STANDARD ASK
    [84]  Thermopro TP11 Thermometer
    [85]  Solight TE44/TE66, EMOS E0107T, NX-6876-917
    [86]* Wireless Smoke and Heat Detector GS 558
    [87]  Generic wireless motion sensor
    [88]  Toyota TPMS
    [89]  Ford TPMS
    [90]  Renault TPMS
    [91]  inFactory, nor-tec, FreeTec NC-3982-913 temperature humidity sensor
    [92]  FT-004-B Temperature Sensor
    [93]  Ford Car Key
    [94]  Philips outdoor temperature sensor (type AJ3650)
    [95]  Schrader TPMS EG53MA4, PA66GF35
    [96]  Nexa
    [97]  ThermoPro TP08/TP12/TP20 thermometer
    [98]  GE Color Effects
    [99]  X10 Security
    [100]  Interlogix GE UTC Security Devices
    [101]* Dish remote 6.3
    [102]  SimpliSafe Home Security System (May require disabling automatic gain for KeyPad decodes)
    [103]  Sensible Living Mini-Plant Moisture Sensor
    [104]  Wireless M-Bus, Mode C&T, 100kbps (-f 868.95M -s 1200k)
    [105]  Wireless M-Bus, Mode S, 32.768kbps (-f 868.3M -s 1000k)
    [106]* Wireless M-Bus, Mode R, 4.8kbps (-f 868.33M)
    [107]* Wireless M-Bus, Mode F, 2.4kbps
    [108]  Hyundai WS SENZOR Remote Temperature Sensor
    [109]  WT0124 Pool Thermometer
    [110]  PMV-107J (Toyota) TPMS
    [111]  Emos TTX201 Temperature Sensor
    [112]  Ambient Weather TX-8300 Temperature/Humidity Sensor
    [113]  Ambient Weather WH31E Thermo-Hygrometer Sensor, EcoWitt WH40 rain gauge, WS68 weather station
    [114]  Maverick ET73
    [115]  Honeywell ActivLink, Wireless Doorbell
    [116]  Honeywell ActivLink, Wireless Doorbell (FSK)
    [117]* ESA1000 / ESA2000 Energy Monitor
    [118]* Biltema rain gauge
    [119]  Bresser Weather Center 5-in-1
    [120]  Digitech XC-0324 / AmbientWeather FT005TH temp/hum sensor
    [121]  Opus/Imagintronix XT300 Soil Moisture
    [122]  FS20 / FHT
    [123]* Jansite TPMS Model TY02S
    [124]  LaCrosse/ELV/Conrad WS7000/WS2500 weather sensors
    [125]  TS-FT002 Wireless Ultrasonic Tank Liquid Level Meter With Temperature Sensor
    [126]  Companion WTR001 Temperature Sensor
    [127]  Ecowitt Wireless Outdoor Thermometer WH53/WH0280/WH0281A
    [128]  DirecTV RC66RX Remote Control
    [129]* Eurochron temperature and humidity sensor
    [130]  IKEA Sparsnas Energy Meter Monitor
    [131]  Microchip HCS200/HCS300 KeeLoq Hopping Encoder based remotes
    [132]  TFA Dostmann 30.3196 T/H outdoor sensor
    [133]  Rubicson 48659 Thermometer
    [134]  AOK Weather Station rebrand Holman Industries iWeather WS5029, Conrad AOK-5056, Optex 990018
    [135]  Philips outdoor temperature sensor (type AJ7010)
    [136]  ESIC EMT7110 power meter
    [137]  Globaltronics QUIGG GT-TMBBQ-05
    [138]  Globaltronics GT-WT-03 Sensor
    [139]  Norgo NGE101
    [140]  Elantra2012 TPMS
    [141]  Auriol HG02832, HG05124A-DCF, Rubicson 48957 temperature/humidity sensor
    [142]  Fine Offset Electronics/ECOWITT WH51, SwitchDoc Labs SM23 Soil Moisture Sensor
    [143]  Holman Industries iWeather WS5029 weather station (older PWM)
    [144]  TBH weather sensor
    [145]  WS2032 weather station
    [146]  Auriol AFW2A1 temperature/humidity sensor
    [147]  TFA Drop Rain Gauge 30.3233.01
    [148]  DSC Security Contact (WS4945)
    [149]  ERT Standard Consumption Message (SCM)
    [150]* Klimalogg
    [151]  Visonic powercode
    [152]  Eurochron EFTH-800 temperature and humidity sensor
    [153]  Cotech 36-7959, SwitchDocLabs FT020T wireless weather station with USB
    [154]  Standard Consumption Message Plus (SCMplus)
    [155]  Fine Offset Electronics WH1080/WH3080 Weather Station (FSK)
    [156]  Abarth 124 Spider TPMS
    [157]  Missil ML0757 weather station
    [158]  Sharp SPC775 weather station
    [159]  Insteon
    [160]  ERT Interval Data Message (IDM)
    [161]  ERT Interval Data Message (IDM) for Net Meters
    [162]* ThermoPro-TX2 temperature sensor
    [163]  Acurite 590TX Temperature with optional Humidity
    [164]  Security+ 2.0 (Keyfob)
    [165]  TFA Dostmann 30.3221.02 T/H Outdoor Sensor (also 30.3249.02)
    [166]  LaCrosse Technology View LTV-WSDTH01 Breeze Pro Wind Sensor
    [167]  Somfy RTS
    [168]  Schrader TPMS SMD3MA4 (Subaru) 3039 (Infiniti, Nissan, Renault)
    [169]* Nice Flor-s remote control for gates
    [170]  LaCrosse Technology View LTV-WR1 Multi Sensor
    [171]  LaCrosse Technology View LTV-TH Thermo/Hygro Sensor
    [172]  Bresser Weather Center 6-in-1, 7-in-1 indoor, soil, new 5-in-1, 3-in-1 wind gauge, Froggit WH6000, Ventus C8488A
    [173]  Bresser Weather Center 7-in-1, Air Quality PM2.5/PM10 7009970, CO2 7009977, HCHO/VOC 7009978 sensors
    [174]  EcoDHOME Smart Socket and MCEE Solar monitor
    [175]  LaCrosse Technology View LTV-R1, LTV-R3 Rainfall Gauge, LTV-W1/W2 Wind Sensor
    [176]  BlueLine Innovations Power Cost Monitor
    [177]  Burnhard BBQ thermometer
    [178]  Security+ (Keyfob)
    [179]  Cavius smoke, heat and water detector
    [180]  Jansite TPMS Model Solar
    [181]  Amazon Basics Meat Thermometer
    [182]  TFA Marbella Pool Thermometer
    [183]  Auriol AHFL temperature/humidity sensor
    [184]  Auriol AFT 77 B2 temperature sensor
    [185]  Honeywell CM921 Wireless Programmable Room Thermostat
    [186]  Hyundai TPMS (VDO)
    [187]  RojaFlex shutter and remote devices
    [188]  Marlec Solar iBoost+ sensors
    [189]  Somfy io-homecontrol
    [190]  Ambient Weather WH31L (FineOffset WH57) Lightning-Strike sensor
    [191]  Markisol, E-Motion, BOFU, Rollerhouse, BF-30x, BF-415 curtain remote
    [192]  Govee Water Leak Detector H5054, Door Contact Sensor B5023
    [193]  Clipsal CMR113 Cent-a-meter power meter
    [194]  Inkbird ITH-20R temperature humidity sensor
    [195]  RainPoint soil temperature and moisture sensor
    [196]  Atech-WS308 temperature sensor
    [197]  Acurite Grill/Meat Thermometer 01185M
    [198]* EnOcean ERP1
    [199]  Linear Megacode Garage/Gate Remotes
    [200]* Auriol 4-LD5661/4-LD5972/4-LD6313 temperature/rain sensors
    [201]  Unbranded SolarTPMS for trucks
    [202]  Funkbus / Instafunk (Berker, Gira, Jung)
    [203]  Porsche Boxster/Cayman TPMS
    [204]  Jasco/GE Choice Alert Security Devices
    [205]  Telldus weather station FT0385R sensors
    [206]  LaCrosse TX34-IT rain gauge
    [207]  SmartFire Proflame 2 remote control
    [208]  AVE TPMS
    [209]  SimpliSafe Gen 3 Home Security System
    [210]  Yale HSA (Home Security Alarm), YES-Alarmkit
    [211]  Regency Ceiling Fan Remote (-f 303.75M to 303.96M)
    [212]  Renault 0435R TPMS
    [213]  Fine Offset Electronics WS80 weather station
    [214]  EMOS E6016 weatherstation with DCF77
    [215]  Emax W6, rebrand Altronics x7063/4, Optex 990040/50/51, Orium 13093/13123, Infactory FWS-1200, Newentor Q9, Otio 810025, Protmex PT3390A, Jula Marquant 014331/32, TechniSat IMETEO X6 76-4924-00, Weather Station or temperature/humidity sensor
    [216]* ANT and ANT+ devices
    [217]  EMOS E6016 rain gauge
    [218]  Microchip HCS200/HCS300 KeeLoq Hopping Encoder based remotes (FSK)
    [219]  Fine Offset Electronics WH45 air quality sensor
    [220]  Maverick XR-30 BBQ Sensor
    [221]  Fine Offset Electronics WN34S/L/D and Froggit DP150/D35 temperature sensor
    [222]  Rubicson Pool Thermometer 48942
    [223]  Badger ORION water meter, 100kbps (-f 916.45M -s 1200k)
    [224]  GEO minim+ energy monitor
    [225]  TyreGuard 400 TPMS
    [226]  Kia TPMS (-s 1000k)
    [227]  SRSmith Pool Light Remote Control SRS-2C-TX (-f 915M)
    [228]  Neptune R900 flow meters
    [229]  WEC-2103 temperature/humidity sensor
    [230]  Vauno EN8822C
    [231]  Govee Water Leak Detector H5054
    [232]  TFA Dostmann 14.1504.V2 Radio-controlled grill and meat thermometer
    [233]* CED7000 Shot Timer
    [234]  Watchman Sonic Advanced / Plus, Tekelek
    [235]  Oil Ultrasonic SMART FSK
    [236]  Gasmate BA1008 meat thermometer
    [237]  Flowis flow meters
    [238]  Wireless M-Bus, Mode T, 32.768kbps (-f 868.3M -s 1000k)
    [239]  Revolt NC-5642 Energy Meter
    [240]  LaCrosse TX31U-IT, The Weather Channel WS-1910TWC-IT
    [241]  EezTire E618, Carchet TPMS, TST-507 TPMS
    [242]* Baldr / RainPoint rain gauge.
    [243]  Celsia CZC1 Thermostat
    [244]  Fine Offset Electronics WS90 weather station
    [245]* ThermoPro TX-2C Thermometer and Humidity sensor
    [246]  TFA 30.3151 Weather Station
    [247]  Bresser water leakage
    [248]* Nissan TPMS
    [249]  Bresser lightning
    [250]  Schou 72543 Day Rain Gauge, Motonet MTX Rain, MarQuant Rain Gauge, TFA Dostmann 30.3252.01/47.3006.01 Rain Gauge and Thermometer, ADE WS1907
    [251]  Fine Offset / Ecowitt WH55 water leak sensor
    [252]  BMW Gen4-Gen5 TPMS and Audi TPMS Pressure Alert, multi-brand HUF/Beru, Continental, Schrader/Sensata, Audi
    [253]  Watts WFHT-RF Thermostat
    [254]  Thermor DG950 weather station
    [255]  Mueller Hot Rod water meter
    [256]  ThermoPro TP28b Super Long Range Wireless Meat Thermometer for Smoker BBQ Grill
    [257]  BMW Gen2 and Gen3 TPMS
    [258]  Chamberlain CWPIRC PIR Sensor
    [259]  ThermoPro Meat Thermometers, TP829B 4 probes with temp only
    [260]* Arad/Master Meter Dialog3G water utility meter
    [261]  Geevon TX16-3 outdoor sensor
    [262]  Fine Offset Electronics WH46 air quality sensor
    [263]  Vevor Wireless Weather Station 7-in-1
    [264]  Arexx Multilogger IP-HA90, IP-TH78EXT, TSN-70E
    [265]  Rosstech Digital Control Unit DCU-706/Sundance/Jacuzzi
    [266]  Risco 2 Way Agility protocol, Risco PIR/PET Sensor RWX95P
    [267]  ThermoPro Meat Thermometers, TP828B 2 probes with Temp, BBQ Target LO and HI
    [268]  Bresser Thermo-/Hygro-Sensor Explore Scientific ST1005H
    [269]  DeltaDore X3D devices
    [270]* Quinetic
    [271]  Landis & Gyr Gridstream Power Meters 9.6k
    [272]  Landis & Gyr Gridstream Power Meters 19.2k
    [273]  Landis & Gyr Gridstream Power Meters 38.4k
    [274]  Revolt ZX-7717 power meter
    [275]  GM-Aftermarket TPMS
    [276]  RainPoint HCS012ARF Rain Gauge sensor
    [277]  Apator Metra E-RM 30
    [278]  ThermoPro TX-7B Outdoor Thermometer Hygrometer
    [279]  Nexus, CRX, Prego sauna temperature sensor
    [280]  Homelead HG9901 (Geevon, Dr.Meter, Royal Gardineer) soil moisture/temp/light level sensor
    [281]  Maverick XR-50 BBQ Sensor

* Disabled by default, use -R n or a conf file to enable


		= Input device selection =
	RTL-SDR device driver is available.
  [-d <RTL-SDR USB device index>] (default: 0)
  [-d :<RTL-SDR USB device serial (can be set with rtl_eeprom -s)>]
	To set gain for RTL-SDR use -g <gain> to set an overall gain in dB.
	SoapySDR device driver is available.
  [-d ""] Open default SoapySDR device
  [-d driver=rtlsdr] Open e.g. specific SoapySDR device
	To set gain for SoapySDR use -g ELEM=val,ELEM=val,... e.g. -g LNA=20,TIA=8,PGA=2 (for LimeSDR).
  [-d rtl_tcp[:[//]host[:port]] (default: localhost:1234)
	Specify host/port to connect to with e.g. -d rtl_tcp:127.0.0.1:1234


		= Gain option =
  [-g <gain>] (default: auto)
	For RTL-SDR: gain in dB ("0" is auto).
	For SoapySDR: gain in dB for automatic distribution ("" is auto), or string of gain elements.
	E.g. "LNA=20,TIA=8,PGA=2" for LimeSDR.


		= Flex decoder spec =
Use -X <spec> to add a flexible general purpose decoder.

<spec> is "key=value[,key=value...]"
Common keys are:
	name=<name> (or: n=<name>)
	modulation=<modulation> (or: m=<modulation>)
	short=<short> (or: s=<short>)
	long=<long> (or: l=<long>)
	sync=<sync> (or: y=<sync>)
	reset=<reset> (or: r=<reset>)
	gap=<gap> (or: g=<gap>)
	tolerance=<tolerance> (or: t=<tolerance>)
	priority=<n> : run decoder only as fallback
where:
<name> can be any descriptive name tag you need in the output
<modulation> is one of:
	OOK_MC_ZEROBIT :  Manchester Code with fixed leading zero bit
	OOK_PCM :         Non Return to Zero coding (Pulse Code)
	OOK_RZ :          Return to Zero coding (Pulse Code)
	OOK_PPM :         Pulse Position Modulation
	OOK_PWM :         Pulse Width Modulation
	OOK_DMC :         Differential Manchester Code
	OOK_PIWM_RAW :    Raw Pulse Interval and Width Modulation
	OOK_PIWM_DC :     Differential Pulse Interval and Width Modulation
	OOK_MC_OSV1 :     Manchester Code for OSv1 devices
	FSK_PCM :         FSK Pulse Code Modulation
	FSK_PWM :         FSK Pulse Width Modulation
	FSK_MC_ZEROBIT :  Manchester Code with fixed leading zero bit
<short>, <long>, <sync> are nominal modulation timings in us,
<reset>, <gap>, <tolerance> are maximum modulation timings in us:
PCM/RZ  short: Nominal width of pulse [us]
         long: Nominal width of bit period [us]
PPM     short: Nominal width of '0' gap [us]
         long: Nominal width of '1' gap [us]
PWM     short: Nominal width of '1' pulse [us]
         long: Nominal width of '0' pulse [us]
         sync: Nominal width of sync pulse [us] (optional)
common    gap: Maximum gap size before new row of bits [us]
        reset: Maximum gap size before End Of Message [us]
    tolerance: Maximum pulse deviation [us] (optional).
Available options are:
	bits=<n> : only match if at least one row has <n> bits
	rows=<n> : only match if there are <n> rows
	repeats=<n> : only match if some row is repeated <n> times
		use opt>=n to match at least <n> and opt<=n to match at most <n>
	invert : invert all bits
	reflect : reflect each byte (MSB first to MSB last)
	decode_uart : UART 8n1 (10-to-8) decode
	decode_dm : Differential Manchester decode
	match=<bits> : only match if the <bits> are found
	preamble=<bits> : match and align at the <bits> preamble
		<bits> is a row spec of {<bit count>}<bits as hex number>
	unique : suppress duplicate row output

	countonly : suppress detailed row output

E.g. -X "n=doorbell,m=OOK_PWM,s=400,l=800,r=7000,g=1000,match={24}0xa9878c,repeats>=3"



		= Output format option =
  [-F log|kv|json|csv|mqtt|influx|syslog|trigger|rtl_tcp|http|null] Produce decoded output in given format.
	Without this option the default is LOG and KV output. Use "-F null" to remove the default.
	Append output to file with :<filename> (e.g. -F csv:log.csv), defaults to stdout.
  [-F mqtt[s][:[//]host[:port][,<options>]] (default: localhost:1883)
	Specify MQTT server with e.g. -F mqtt://localhost:1883
	Default user and password are read from MQTT_USERNAME and MQTT_PASSWORD env vars.
	Add MQTT options with e.g. -F "mqtt://host:1883,opt=arg"
	MQTT options are: user=foo, pass=bar, retain[=0|1], <format>[=topic]
	Supported MQTT formats: (default is all)
	  availability: posts availability (online/offline)
	  events: posts JSON event data, default "<base>/events"
	  states: posts JSON state data, default "<base>/states"
	  devices: posts device and sensor info in nested topics,
	           default "<base>/devices[/type][/model][/subtype][/channel][/id]"
	A base topic can be set with base=<topic>, default is "rtl_433/HOSTNAME".
	Any topic string overrides the base topic and will expand keys like [/model]
	E.g. -F "mqtt://localhost:1883,user=USERNAME,pass=PASSWORD,retain=0,devices=rtl_433[/id]"
	For TLS use e.g. -F "mqtts://host,tls_cert=<path>,tls_key=<path>,tls_ca_cert=<path>"
	With MQTT each rtl_433 instance needs a distinct driver selection. The MQTT Client-ID is computed from the driver string.
	If you use multiple RTL-SDR, perhaps set a serial and select by that (helps not to get the wrong antenna).
  [-F influx[:[//]host[:port][/<path and options>]]
	Specify InfluxDB 2.0 server with e.g. -F "influx://localhost:9999/api/v2/write?org=<org>&bucket=<bucket>,token=<authtoken>"
	Specify InfluxDB 1.x server with e.g. -F "influx://localhost:8086/write?db=<db>&p=<password>&u=<user>"
	  Additional parameter -M time:unix:usec:utc for correct timestamps in InfluxDB recommended
  [-F syslog[:[//]host[:port] (default: localhost:514)
	Specify host/port for syslog with e.g. -F syslog:127.0.0.1:1514
  [-F trigger:/path/to/file]
	Add an output that writes a "1" to the path for each event, use with a e.g. a GPIO
  [-F rtl_tcp[:[//]bind[:port]] (default: localhost:1234)
	Add a rtl_tcp pass-through server
  [-F http[:[//]bind[:port]] (default: 0.0.0.0:8433)
	Add a HTTP API server, a UI is at e.g. http://localhost:8433/


		= Meta information option =
  [-M time[:<options>]|protocol|level|noise[:<secs>]|stats|bits] Add various metadata to every output line.
	Use "time" to add current date and time meta data (preset for live inputs).
	Use "time:rel" to add sample position meta data (preset for read-file and stdin).
	Use "time:unix" to show the seconds since unix epoch as time meta data. This is always UTC.
	Use "time:iso" to show the time with ISO-8601 format (YYYY-MM-DD"T"hh:mm:ss).
	Use "time:off" to remove time meta data.
	Use "time:usec" to add microseconds to date time meta data.
	Use "time:tz" to output time with timezone offset.
	Use "time:utc" to output time in UTC.
		(this may also be accomplished by invocation with TZ environment variable set).
		"usec" and "utc" can be combined with other options, eg. "time:iso:utc" or "time:unix:usec".
	Use "replay[:N]" to replay file inputs at (N-times) realtime.
	Use "protocol" / "noprotocol" to output the decoder protocol number meta data.
	Use "level" to add Modulation, Frequency, RSSI, SNR, and Noise meta data.
	Use "noise[:<secs>]" to report estimated noise level at intervals (default: 10 seconds).
	Use "stats[:[<level>][:<interval>]]" to report statistics (default: 600 seconds).
	  level 0: no report, 1: report successful devices, 2: report active devices, 3: report all
	Use "bits" to add bit representation to code outputs (for debug).


		= Read file option =
  [-r <filename>] Read data from input file instead of a receiver
	Parameters are detected from the full path, file name, and extension.

	A center frequency is detected as (fractional) number suffixed with 'M',
	'Hz', 'kHz', 'MHz', or 'GHz'.

	A sample rate is detected as (fractional) number suffixed with 'k',
	'sps', 'ksps', 'Msps', or 'Gsps'.

	File content and format are detected as parameters, possible options are:
	'cu8', 'cs16', 'cf32' ('IQ' implied), and 'am.s16'.

	Parameters must be separated by non-alphanumeric chars and are case-insensitive.
	Overrides can be prefixed, separated by colon (':')

	E.g. default detection by extension: path/filename.am.s16
	forced overrides: am:s16:path/filename.ext

	Reading from pipes also support format options.
	E.g reading complex 32-bit float: CU32:-


		= Write file option =
  [-w <filename>] Save data stream to output file (a '-' dumps samples to stdout)
  [-W <filename>] Save data stream to output file, overwrite existing file
	Parameters are detected from the full path, file name, and extension.

	File content and format are detected as parameters, possible options are:
	'cu8', 'cs8', 'cs16', 'cf32' ('IQ' implied),
	'am.s16', 'am.f32', 'fm.s16', 'fm.f32',
	'i.f32', 'q.f32', 'logic.u8', 'ook', and 'vcd'.

	Parameters must be separated by non-alphanumeric chars and are case-insensitive.
	Overrides can be prefixed, separated by colon (':')

	E.g. default detection by extension: path/filename.am.s16
	forced overrides: am:s16:path/filename.ext

```


Some examples:

| Command | Description
|---------|------------
| `rtl_433` | Default receive mode, use the first device found, listen at 433.92 MHz at 250k sample rate.
| `rtl_433 -C si` | Default receive mode, also convert units to metric system.
| `rtl_433 -f 868M -s 1024k` | Listen at 868 MHz and 1024k sample rate.
| `rtl_433 -M hires -M level` | Report microsecond accurate timestamps and add reception levels (depending on gain).
| `rtl_433 -R 1 -R 8 -R 43` | Enable only specific decoders for desired devices.
| `rtl_433 -A` | Enable pulse analyzer. Summarizes the timings of pulses, gaps, and periods. Can be used with `-R 0` to disable decoders.
| `rtl_433 -S all -T 120` | Save all detected signals (`g###_###M_###k.cu8`). Run for 2 minutes.
| `rtl_433 -K FILE -r file_name` | Read a saved data file instead of receiving live data. Tag output with filenames.
| `rtl_433 -F json -M utc \| mosquitto_pub -t home/rtl_433 -l` | Will pipe the output to network as JSON formatted MQTT messages. A test MQTT client can be found in `examples/mqtt_rtl_433_test_client.py`.
| `rtl_433 -f 433.53M -f 434.02M -H 15` | Will poll two frequencies with 15 seconds hop interval.

## Security

Please note: We aim to make `rtl_433` safe to use, but it should not be assumed secure.
There is no reason to e.g. run with `sudo`, we do read and write files without any checks.

The output is literally pulled from thin air, it's not to be trusted.
If you feed downstream systems with data make sure edge cases are checked and handled.
Network inputs and outputs are for use in a trusted local network, will contain unfiltered data, and might overload the recipient
(know that e.g. the MQTT output can be controlled by anyone with a radio sender).

## Google Group

Join the Google group, rtl_433, for more information about rtl_433:
https://groups.google.com/forum/#!forum/rtl_433


## Troubleshooting

If you see this error:

    Kernel driver is active, or device is claimed by second instance of librtlsdr.
    In the first case, please either detach or blacklist the kernel module
    (dvb_usb_rtl28xxu), or enable automatic detaching at compile time.

then

    sudo rmmod rtl2832_sdr dvb_usb_rtl28xxu rtl2832

or add

    blacklist dvb_usb_rtl28xxu

to /etc/modprobe.d/blacklist.conf

## Releases

Version numbering scheme used is year.month. We try to keep the API compatible between releases but focus is on maintainablity.
