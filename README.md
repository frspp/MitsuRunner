# MitsuRunner introduction
This software for ESP8266 or ESP32 prevents Mitsubishi Heat Pumps unnecessarily de-frosting outdoor unit 

This .yaml file is designed for https://ESPhome.io, so Esphome is needed to generate, compile and update sources. Please follow information from their site how to set-up environments.

## Background
**Mitsubishi Heat Pump's built-in software does not suit -5 c degrees or colder winter climate where pump is used for heating.** The minus degrees pump heats mainly with very short cycles (30...60 minutes) and in between tries to de-frost outdoor unit's heat exhanger even there is no ice. Constant de-frost leads to poor heating and most likely very poor effiency ("COP") compared to unit where defrost happens in very same conditions only once every 2-3 hours. Also indoor unit blows cold inside when defrost is happening. All this is waste of energy.

The design of Mitsubishi MZN-LN25/35 etc. heatpumps is poor both in software and in mechanics. 
- It uses poor "fuzzy logic" algorithm: Instead of following current state with sensors it tries to predict from previous cycles when outdoor unit needs de-frost. 
- The mechanical side probems are related to defrost temperature sensor which is used to detect when the ice has melt from outdoor heat exhanger. The defrost temperature sensor is installed poorly so it touches non-waterproof insulation material, which gets wet from melting water/condensation and then freezes. Frozen defrost sensor does not measure correct value used needed for "fuzzy logic". Thus "fuzzy logic" starts to think outdoor heat exhanger is too icy and starts to decrease duration of heating cycles to prevent icing. This leads to situation where pump does not heat much but tries to melt outdoor unit's heat exhanger often. Basically this means wasting energy as it takes warm air from inside to defrost the outdoor unit.

This software started from topic in Finnish lampopumput.info forum: https://lampopumput.info/foorumi/threads/msz-ln-sulatushuijaus.31223/ 

## Harwdare shopping list
- 1x ESP32 or ESP8266 based controller, for exampple WeMos D1 Mini
- 1x USB power source for controller
- 1x relay compatible with controller, for example Wemos D1 mini relay shield
- 2x waterproof DS18B20 temperature sensors
- (2x non-waterproof DS18B20 for backup in case waterproof sensors reacts too slow)
- 1x 33k (33000 ohm) resistor

And some basic hardware store items like 
- wire (multicore wire like CAT-cable or phone cable)
- enclosure for contoller
- etc.
