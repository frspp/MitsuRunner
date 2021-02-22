# Mitsurunner
Software for ESP8266 or ESP32 based device designed to prevent Mitsubishi heatpumps to do too short heating runs and melting ice too often from outdoor unit's heat exhanger.

This software started form topic in lampopumput.info site: https://lampopumput.info/foorumi/threads/msz-ln-sulatushuijaus.31223/ 

There are a lot of Finnish people suffering poor design of Mitsubishi heat pumps, especially MSZ-LN series pumps, whose goes to "grazy mode"where they melt ice from outdoor unit in 30 to 40 minutes cycles, even there is no any ice in outdoor unit's heat exhanger.

The design of these heatpumps are poor from their software side and from mechanical side. The software in the units does not suit for the Finnish climate and most probably not to any climate, where pump needs to be used for heating and temperature goes below -5 degrees. The pump heats mainly with short cycles and tries to melt ice from outdoor unit's heat exhanger even there is no ice. This wastes just energy and makes heating infficient. This is due to poor fuzzy logic algorith which: instead of following current state with sensors, tries to forecast from previous experience, when to melt ice from outdoor heat exhanger. The mechanical side probems are related to defrost temperature sensor, which is used to detect when the ice has melt from outdoor heat exhanger. The defrost temperature sensor is installed touching felt insulation, which get's wet and icy from melting water/condension. After using pump to heat for a while, the felt becomes so wet and thus icy that the defrost sensor does not anymore get correct value when outdoor heat exhanger has melted as it measures fel's ice's temperature now not heat exhangers temperature. Thus it starts to affect for device's fuzzy logic, which starts to think that heating has been too long as outdoor heat exhanger is too icy and thus starts to decrease durating of heating... leading to situation where pump does not heat much, but tries to melt outdoor unit's heat exhanger much. Basically wasting energy by melting really often, meaning : heating outside air by taking heat from inside.  

This .yaml file is designed for ESPhome.io, which is needed to generate, compile and update sources. Please follow information from their site how to set-up environments.

