Křižovatka
==========

Cílem je vytvořit malý model chytré křižovatky, která bude svůj stav zveřejňovat
do centrálního ovládacího panelu (fiktivního velína smart city).

Veškerá komunikace přes internet probíhá přes cloudový MQTT server (broker) HiveMQ, který je zdarma:

| Adresa            | d57a0d1c39d54550b147b58411d86743.s2.eu.hivemq.cloud |
| Port (Web Socket) | 8884                                                |
| SSL               | Ano                                                 |
| Username          | robot                                               |
| Heslo             | P@ssW0rd!                                           |

Máte k dispozici
----------------

-   LED světýlka na až 8 páscích po maximálně 20 ks LED na každém pásku.

    Ovládají se přes MQTT topic `/crossing/led/<i>ČÍSLO_PINU_NA_ARDUINU</i>`, kam je pásek připojen.

    Možné piny: `13`, `12`, `11`, `8`, `7`, `6`, `3`, `2`

    Tedy možné topicy + zpráva (kterou je třeba do nich zaslat):
        -   `/crossing/led/13` <- `RRGGBB,RRGGBB,RRGGBB,RRGGBB` ...
        -   `/crossing/led/12` <- `RRGGBB,RRGGBB,RRGGBB` ...
        -   `/crossing/led/11` <- `RRGGBB,RRGGBB,RRGGBB` ...
        -   `/crossing/led/8`  <- `RRGGBB,RRGGBB,RRGGBB,RRGGBB,RRGGBB` ...
        -   `/crossing/led/7`  <- `RRGGBB,RRGGBB,RRGGBB` ...
        -   `/crossing/led/3`  <- `RRGGBB,RRGGBB,RRGGBB,RRGGBB,RRGGBB,RRGGBB` ...
        -   `/crossing/led/2`  <- `RRGGBB,RRGGBB,RRGGBB` ...

        RRGGBB je vždycky RGB hexadecimální kód barvy (stejný jako v CSS bez `#`) jedné LED.
        Čárko se oddělují jednotlivé LED na pásku.
        Poznámka: Je možné zaslat i více nebo méně RGB kódů než je LEDek na pásku. Ostatní LED zůstanou v původním stavu.


-   Klasická tlačítka (až 6 kusů)

    Ovládají se přes MQTT topic `/crossing/button/<i>ČÍSLO_PINU_NA_ARDUINU</i>`, kam je tlačítko připojeno.

    Možné piny: `A5`, `A4`, `A3`, `A2`, `A1`, `A0`

    Tedy možné topicy + zpráva (která z nich příchází):
        -   `/crossing/button/A5` -> `pressed`
        -   `/crossing/button/A4` -> `pressed`
        -   `/crossing/button/A3` -> `pressed`
        -   `/crossing/button/A2` -> `pressed`
        -   `/crossing/button/A1` -> `pressed`
        -   `/crossing/button/A0` -> `pressed`

    Pozor! Každé tlačítko se musí nejprve povolit. Jinak zprávy do topicu neposílá.
    Důvod je technický, protože nelze zjistit, zda bylo tlačítko opravdu osazeno (fyzicky připojeno drátkem),
    a pokud nebylo, daný pin na Arduinu generuje náhodný šum (a tudíž náhodné stisky).

    Topicy pro prvotní povolení tlačítka:
    -   `/crossing/button-enable/A5` <- `on` nebo `off`
    -   `/crossing/button-enable/A4` <- `on` nebo `off`
    -   `/crossing/button-enable/A3` <- `on` nebo `off`
    -   `/crossing/button-enable/A2` <- `on` nebo `off`
    -   `/crossing/button-enable/A1` <- `on` nebo `off`
    -   `/crossing/button-enable/A0` <- `on` nebo `off`



### Pohodlnější zapínání celého semaforu:

Pro zjednodušení lze poslat do topicu `/crossing/semaphore/<i>ČÍSLO_PINU_NA_ARDUINU</i>` přímo celé zadání světel jednoho semaforu:

`R00` ... Semafor s červenou. Číslice jsou první pozice LED semaforu na pásku. Pozor, vždy zapsané dvojciferně 00-99
`P00` ... Semafor s červenou a oranžovou. Číslice jsou první pozice LED semaforu na pásku. Pozor, vždy zapsané dvojciferně 00-99
`G00` ... Semafor s červenou a oranžovou. Číslice jsou první pozice LED semaforu na pásku. Pozor, vždy zapsané dvojciferně 00-99
`O00` ... Semafor s červenou a oranžovou. Číslice jsou první pozice LED semaforu na pásku. Pozor, vždy zapsané dvojciferně 00-99
`D00` ... Semafor pro chodce s červenou. Číslice jsou první pozice LED semaforu na pásku. Pozor, vždy zapsané dvojciferně 00-99
`W00` ... Semafor pro chodce se zelenou. Číslice jsou první pozice LED semaforu na pásku. Pozor, vždy zapsané dvojciferně 00-99



Železniční přejezd
==================

Máte k dispozici
----------------

-   Individuální červené LED, 4 ks

    Ovládají se přes MQTT topic `/rail-crossing/led/<i>ČÍSLO_PINU_NA_ARDUINU</i>`, kam je LED připojena.

    Možné piny: `13`, `12`, `11`, `8`

    -   `/rail-crossing/led/13` <- `on` nebo `off`
    -   `/rail-crossing/led/12` <- `on` nebo `off`
    -   `/rail-crossing/led/11` <- `on` nebo `off`
    -   `/rail-crossing/led/8` <- `on` nebo `off`


-   Bzučák (piezo)

    Ovládá se přes MQTT topic `/rail-crossing/sirene`.

    Přippojuje se výhradně na pin Arduina číslo `8`.

    Tedy možné topicy + zpráva (kterou je třeba do nich zaslat):
    -   `/rail-crossing/sirene` <- `beep`


-   Servo motor pro závoru, 2 ks

    Ovládá se přes MQTT topic `/rail-crossing/barrier/<i>ČÍSLO_PINU_NA_ARDUINU</i>`, kam je servo připojeno.

    Možné piny: `10`, `9`

    Tedy možné topicy + zpráva (kterou je třeba do nich zaslat):
    -   `/rail-crossing/barrier/10` <- `open` nebo `close`
    -   `/rail-crossing/barrier/9` <- `open` nebo `close`



-   Detektor pohybu (infračervená LED + infračervený senzor), 4 ks

    Dají se naproti sobě, a až projede (fiktivní) vlak, zastíní senzoru výhled na LED a ten tím pádem vydá událost pohybu.

    Ovládá se přes MQTT topic `/rail-crossing/motion-detection/<i>ČÍSLO_PINU_NA_ARDUINU</i>`, kam je senzor připojen.

    Možné piny: `A0`, `A1`, `A2`, `A3`

    Tedy možné topicy + zpráva (kterou je třeba do nich zaslat):
    -   `/rail-crossing/motion-detection/A0` -> `ongoing`
    -   `/rail-crossing/motion-detection/A1` -> `ongoing`
    -   `/rail-crossing/motion-detection/A2` -> `ongoing`
    -   `/rail-crossing/motion-detection/A3` -> `ongoing`

    Podobně jako tlačítka je ale nutné nejprve každý senzor povolit, protože jinak by nepřipojené
    senzory generovaly náhodný šum.

    -   `/rail-crossing/motion-detection-enable/A0` <- `on` nebo `off`
    -   `/rail-crossing/motion-detection-enable/A1` <- `on` nebo `off`
    -   `/rail-crossing/motion-detection-enable/A2` <- `on` nebo `off`
    -   `/rail-crossing/motion-detection-enable/A3` <- `on` nebo `off`

    Doporučuji pro pokusné účely zaměnit infračervený senzor za tlačítko a řešit jen tlačítko.
    Není potom potřeba složitě naproti sobě nastavovat LED a senzor a dávat pozor, aby nebyly "přesvity" mezi LEDkami.
