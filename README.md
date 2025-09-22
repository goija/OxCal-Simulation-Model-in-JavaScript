# OxCal-Simulation-Model-in-JavaScript

OxCal-Simulation-Model-in-JavaScript" raakt de kern van hoe moderne archeologische dateringssoftware functioneert. Hoewel het hart van OxCal (de rekenkundige 'engine') is geschreven in de programmeertaal C++, speelt JavaScript (JS) een cruciale en onmisbare rol in de architectuur, de gebruikersinterface en de verwerking van de output van het programma. Dit is met name relevant voor de krachtige simulatiefuncties die in de bronnen worden beschreven.

Hieronder volgt een uiteenzetting van hoe deze drie componenten—OxCal, simulatiemodellen en JavaScript—met elkaar verweven zijn.

1. De Rol van JavaScript in de Architectuur van OxCal

De moderne versies van OxCal (versie 4 en hoger) zijn sterk afhankelijk van webtechnologieën, en dus van JavaScript, voor zowel de online als de downloadbare versie.

    De Webinterface (GUI): De meest gebruikte versie van OxCal is de online webapplicatie. De volledige grafische gebruikersinterface (GUI) die u in uw browser ziet—zoals de vensters, knoppen, menu's en de interactieve plots—is gebouwd met JavaScript. Deze interface maakt het mogelijk om complexe modellen te bouwen zonder direct de programmeertaal CQL2 te hoeven schrijven, hoewel het direct bewerken van de code mogelijk blijft.

    Lokale Installatie via Node.js: Zelfs wanneer u OxCal downloadt om lokaal op uw computer te draaien, installeert u in feite een lokale webserver die draait op Node.js. Node.js is een JavaScript-runtime-omgeving. Dit betekent dat u lokaal dezelfde webinterface gebruikt die door JavaScript wordt aangedreven, wat zorgt voor een consistente gebruikerservaring.

    De outputbestanden (.js): Misschien wel de meest directe link is dat de volledige output van een OxCal-analyse wordt opgeslagen in een ECMAScript/JavaScript (.js)-bestand. Dit bestand bevat alle resultaten in een gestructureerd dataformaat: de gekalibreerde data, de gemodelleerde waarschijnlijkheidsverdelingen, de grenzen, de Agreement Indices, etc. De grafieken en tabellen die u in de outputvensters ziet, worden dynamisch gegenereerd doordat uw browser de data uit dit .js-bestand leest en visualiseert.

2. Het Simulatiemodel: De R_Simulate Functie

Een "simulatiemodel" in de context van OxCal verwijst naar het gebruik van de R_Simulate()-functie. In tegenstelling tot R_Date(), die een gemeten ¹⁴C-leeftijd (BP) omzet naar een kalenderdatum, doet R_Simulate() het omgekeerde: het neemt een bekende kalenderdatum met een verwachte meetonzekerheid en genereert een willekeurige, gesimuleerde ¹⁴C-leeftijd die een laboratorium voor een monster van die leeftijd zou kunnen produceren.

De bronnen benadrukken meerdere cruciale toepassingen voor deze simulaties:

    Onderzoeksopzet en Steekproefgrootte Bepalen: Dit is een van de krachtigste toepassingen. Voordat men een kostbaar dateringsprogramma start, kunnen archeologen simulaties uitvoeren om te bepalen hoeveel dateringen nodig zijn om een specifieke onderzoeksvraag met de gewenste precisie te beantwoorden. U kunt bijvoorbeeld simuleren hoeveel dateringen nodig zijn om de begin- en einddatum van een fase binnen een marge van 50 jaar vast te stellen. Dit stelt onderzoekers in staat om hun dateringsstrategie formeel te onderbouwen in onderzoeksvoorstellen en publicaties.

    Effecten van de Kalibratiecurve Evalueren: Simulaties kunnen helpen inschatten of dateringen wel effectief zullen zijn in periodes met "plateaus" of "wiggles" in de kalibratiecurve, waar een enkele ¹⁴C-leeftijd kan corresponderen met een zeer breed of multimodaal kalenderjaarbereik. Dit helpt om realistische verwachtingen te scheppen over de haalbare precisie.

    Modelvalidatie en Robuustheid Testen: Door hypothetische datasets te genereren, kunnen onderzoekers de robuustheid van hun Bayesiaanse modelstructuur testen. Onderzoekers kunnen bijvoorbeeld de impact van verschillende priors (archeologische aannames) of de invloed van potentiële uitschieters onderzoeken.

    Ontwikkeling van Nieuwe Methodes: De R_Simulate-functie is zo fundamenteel dat deze zelfs wordt gebruikt als basis voor volledig nieuwe dateringsmethoden. Eén studie stelt een "fine-dating"-techniek voor die gebaseerd is op het genereren van zeer grote referentietabellen met tienduizenden gesimuleerde dateringen om de precisie te verhogen.

3. De Synergie: Hoe JavaScript en Simulaties Samenkomen

De combinatie van deze elementen vormt de moderne OxCal-workflow:

    Een onderzoeker ontwerpt een simulatiemodel in de JavaScript-gebaseerde GUI van OxCal, gebruikmakend van de R_Simulate-functie om een reeks hypothetische dateringen te genereren die een archeologisch scenario nabootsen.

    De analyse wordt uitgevoerd door de C++-'engine' van OxCal, die de MCMC-simulaties uitvoert.

    De resultaten van deze simulatie worden opgeslagen in een gestructureerd .js-bestand.

    De JavaScript-code in de outputvensters van de browser leest dit .js-bestand en genereert de interactieve grafieken en tabellen. Deze visualisaties zijn cruciaal om de resultaten van de simulaties te interpreteren, bijvoorbeeld door te plotten hoe de precisie van een Boundary toeneemt naarmate er meer gesimuleerde dateringen aan het model worden toegevoegd.

Kortom, een "OxCal-Simulation-Model-in-JavaScript" beschrijft perfect de praktijk waarbij een archeoloog een simulatiemodel (met R_Simulate) opzet en uitvoert binnen de JavaScript-interface van OxCal, waarna de resultaten worden verwerkt en gevisualiseerd met behulp van dezelfde JavaScript-technologie. Het is de combinatie van een krachtige statistische functie (R_Simulate) en een moderne, toegankelijke webtechnologie (JavaScript) die OxCal tot zo'n veelzijdig instrument maakt voor het bouwen van robuuste chronologieën.
