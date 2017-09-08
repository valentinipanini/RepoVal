De nombreux acteurs jouent un rôle dans la plateforme **FMUv4-PRO**; utilisateurs finaux, développeurs, chercheurs et fabricants. Les objectifs de cette itération de la plateforme sont:

* Un contrôleur de vol sur une carte unique et ergonomique pour les applications  en espace réduit.
* Un contrôleur de vol modulable en plusieurs cartes pour les applications professionnelles.
* Entrées et sorties suffisantes pour la majorité des applications, sans extension.
* Une utilisation plus intuitive.
* Des capteurs plus performants.
* Des capacités et ressources plus importantes \(384 Ko RAM, 2 Mo Flash\).
* Une fiabilité supérieure et une difficulté d'intégration réduite.
* Un BoM et des coûts de fabrication réduits.

**Points clé du design**

* Design tout-en-un avec un FMU intégré, un IO optionnel et de nombreuses entrées et sorties.
* Une fabrication améliorée, élaborée pour une intégration et une création de boîtier simplifiée.
* Des alimentations séparées pour le FMU et l'IO \(voir Architecture Alimentation\)
* Système de remplacement ponctuel de la batterie pour le FMU et la SRAM / RTC de l'IO implémenté sur la carte.
* Possibilité d'utilisation avec deux modules de puissance.

**Améliorations technologiques**

* Upgrade du microcontrôleur vers un STM32F469; augmentation mémoire flash de 1Mio à 2Mio, de la RAM de 256Kio à 384Kio, plus d'entrées sorties périphériques.
* Gyroscopes / Accéléromètres / Magnétomètres **ICM-20602**, **MPU9K **intégrés.
* Magnétomètre boussole **LIS3MDL **\(le HMC5983 est devenu obsolète\).
* Capteurs connectés sur deux bus SPI \(un bus high-rate et un bus low-noise\).
* Deux bus I2C.
* Deux bus CAN.
* Lecture de tension/ampérage depuis deux modules de puissance.
* Inverseur FrSky.
* Connecteurs JST-GH user-friendly.

**Entées / Sorties:**

* 6-14 sorties servo PWM \(8 de l'IO, 6 du FMU\).
* Entrées R/C pour le CPPM, Spektrum / DSM et S.Bus.
* Entrée analogique / PWM RSSI.
* Sortie servo S.Bus.
* 6 ports série, deux avec le Full flow Control, un limité à 1A, et un avec inverseur de protocole FrSky.
* 2 ports I2C.
* 2 ports SPI externes \(non bufferisés, avec des câbles courts\).
* 2 interfaces bus CAN.
* Entrées analogiques pour la tension/ampérage de deux batteries.
* Connectique pour buzzer piezopour utilisation au sol.
* Connectique favorisant l'upgrade des capteurs.
* LED RGB haute visibilité.
* Safety Switch à LED.

**Caractéristiques physiques et mécaniques:**

* 71 \* 49 \* 23 mm \(en boîtier\).
* 45g \(en boîtier\).
* Emplacement standardisé du connecteur MicroUSB.
* Emplacement standardisé de la LED RGB.
* Emplacement standardisé des connecteurs.

**Architecture du système:**

L'iteration FMUv4-PRO conserve l'architecture PX4FMU+PX4IO de la génération précédente, en intégrant en un seul module physique les deux parties fonctionnelles.

**Sorties PWM:**

Huit sorties PWM sont connectées à l'IO qui peut les contrôler directement via les entrées R/C et un moduleur, même lorsque le FMU est inactif \(failsafe / mode manuel\). Plusieurs vitesses de rafraîchissement sont possibles sur ces sorties divisées en trois groupes : 1 groupe de quatre sorties et 2 groupes de deux sorties. Des signaux PWM jusqu'à 400Hz sont supportés.

Six sorties PWM sont connectées au FMU et intègrent une fonctionnalité de réduction du temps de rafraîchissement. Ces sorties ne peuvent pas être contrôlées par l'IO en mode failsafe. Plusieurs vitesses de rafraîchissement sont possibles sur ces sorties divisées en deux groupes : 1 groupe de quatre et 1 groupe de deux. Des signaux PWM jusqu'à 400Hz sont supportés.

Toutes les sorties PWM sont anti-ESD et sont conçues pour résister à des mauvaises connections accidentelles des servos sans être endommagées. ~~Les contrôleurs des servo sont faits pour contrôler la charge d'entrée d'un servo 50pF sur deux mètres de câble pour servo 26AWG.~~ Les sorties PWM peuvent également être utilisées comme GPIOs. Il est à noter que ces sorties ne sont pas des sorties haute puissance - les contrôleurs PWM sont faits pour contrôler des servos et des signaux analogiques du même type, mais pas de relais ou de LEDs.



**Ports périphériques:**

FMUv4-PRO recommande d'utiliser des connecteurs différents pour chacun des ports périphériques \(hormis quelques exceptions\). Cela permet d'éviter des soucis rapportés par de nombreux utilisateurs lorsqu'ils se connectaient au port multi-IO 15-pin sur le PX4FMU-PRO ~~original et offre la possibilité de fabriquer des câbles spécifiques pour chaque utilisation.~~ 

Cinq ports série sont à disposition. TELEM 1, 2 et 3 intègrent le Full flow Control. TELEM 4 peut être passé en mode inversé par le biais du hardware et n'intègre pas de flow control. Les ports série sont en signaux logiques CMOS 3.3V, tolèrent le 5V, sont bufferisés et anti-ESD.

**Peripheral Ports:**

FMUv4-PRO recommends separate connectors for each of the peripheral ports \(with a few exceptions\). This avoids the issues many users reported connecting to the 15-pin multi-IO port on the original PX4FMU-PRO and allows single-purpose peripheral cables to be manufactured.+

Five serial ports are provided. TELEM 1, 2 and 3 feature full flow control. TELEM4 can be switched into inverted mode by hardware and has no flow control. Serial ports are 3.3V CMOS logic level, 5V tolerant, buffered and ESDprotected.

The SPI ports are not buffered; they should only be used with short cable runs. Signals are 3.3V CMOS logic level, but 5V tolerant.

Two power modules \(voltage and current for each module\) can be sampled by the main processor.

The RSSI input supports either PWM or analog RSSI. CPPM, S.Bus and DSM/ Spektrum share now a single port and are auto-detected in software.

The CAN ports are standard CAN Bus; termination for one end of the bus is fixed onboard.

**Sensors:**

The new**ICM-20602**has been positioned by Invensense as higher-end successor of the MPU-6000 series. The software also supports the MPU-9250, which allows a very cost-effective 9D solution.

Data-ready signals from all sensors \(except the MS5611, which does not have one\) are routed to separate interrupt and timer capture pins on FMU. This will permit precise time-stamping of sensor data.

The two external SPI buses and six associated chip select lines allow to add additional sensors and SPI-interfaced payload as needed.

**IMU is isolated from vibrations**.



