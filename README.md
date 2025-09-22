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

Scenario

Stel je voor dat je een nederzetting opgraaft met drie duidelijke, opeenvolgende bewoningslagen (fasen). Je weet uit het veldwerk dat Fase 1 de oudste is, gevolgd door Fase 2, en daarna de jongste, Fase 3.

Je wilt testen hoe goed een Bayesiaans model deze chronologie kan bepalen. Daarvoor simuleer je een "perfecte" dataset waarbij je de ware kalenderjaren van de monsters zelf bepaalt:

    Fase 1: Eén monster van rond 750 v.Chr.

    Fase 2: Twee monsters, één van 650 v.Chr. en één van 600 v.Chr.

    Fase 3: Eén monster van rond 525 v.Chr.

OxCal-code

De onderstaande code wordt ingevoerd in OxCal. De Simulate() functie wordt gebruikt in plaats van de gebruikelijke R_Date() functie.
Code snippet

Plot()
{
 // We definiëren een overkoepelende volgorde (Sequence)
 // voor de hele stratigrafie.
 Sequence("Simulatie Voorbeeld Nederzetting")
 {
  // Een grens (Boundary) voor het begin van de activiteit.
  Boundary("Start");

  // De eerste en oudste fase.
  Phase("Fase 1")
  {
   // Simuleer een C14-monster met een 'ware' datum van 750 v.Chr.
   // en een standaardafwijking van 30 jaar.
   Simulate("Monster_1", -749, 30);
  };

  // De tweede fase, die na Fase 1 komt.
  Phase("Fase 2")
  {
   Simulate("Monster_2", -649, 30);
   Simulate("Monster_3", -599, 30);
  };

  // De laatste en jongste fase.
  Phase("Fase 3")
  {
   Simulate("Monster_4", -524, 30);
  };

  // Een grens voor het einde van de activiteit.
  Boundary("Einde");
 };
};

N.B.: Jaren voor Christus worden als negatieve getallen ingevoerd. Technisch gezien is 750 v.Chr. het jaar -749, omdat er geen jaar 0 is.

Uitleg van de code

    Plot(): De container voor de hele visualisatie.

    Sequence(): Dit is het belangrijkste commando. Het vertelt OxCal dat alle elementen die erin staan (Phases, Boundaries) in een strikte chronologische volgorde moeten plaatsvinden. De bovenste is de oudste.

    Boundary(): Definieert het begin- of eindpunt van een Sequence of Phase.

    Phase(): Groepeert één of meerdere dateringen die (archeologisch gezien) gelijktijdig zijn. Binnen een Phase is er geen chronologische volgorde.

    Simulate("Naam", Jaartal, Foutmarge): Dit is de kern van de simulatie.

        "Naam": De naam die je aan het gesimuleerde monster geeft (bv. "Monster_1").

        Jaartal: Het "ware" kalenderjaar (in AD/BC) dat je wilt simuleren.

        Foutmarge: De standaardafwijking (1σ) die je aan de gesimuleerde C14-meting wilt meegeven. Een waarde van 30 ($^{14}$C-jaren) is realistisch.

Wat gebeurt er als je dit uitvoert? ⚙️

Wanneer je deze code uitvoert, doet OxCal het volgende:

    Het neemt het "ware" kalenderjaar (bv. -749, dus 750 v.Chr.).

    Het kijkt op de kalibratiecurve (bv. IntCal20) welke C14-leeftijd (in BP) hiermee correspondeert.

    Het voegt een willekeurige 'meetfout' toe op basis van de opgegeven foutmarge (bv. 30 jaar).

    Het resultaat is een set van gesimuleerde, ongekalibreerde C14-dateringen (bv. 2580±30 BP).

Waarom is dit nuttig? 🤔

Het grote voordeel is dat je de "ware" chronologie kent. Je kunt de gesimuleerde C14-dateringen (de output) nu in een nieuw, leeg OxCal-model plakken (dit keer met R_Date in plaats van Simulate).

Vervolgens laat je dat model draaien en vergelijk je de berekende Bayesiaanse ouderdomsmodellen met de "ware" data die je zelf had ingevoerd. Zo kun je controleren of je model-structuur (Sequence, Phase, etc.) nauwkeurig en precies genoeg is om de werkelijkheid te reconstrueren. Het is een cruciale stap om de robuustheid van je chronologische interpretatie te valideren.

I've created the HTML file as requested.

It includes:

    Explanation: A clear description of the archaeological scenario and the corresponding OxCal code, making it easy to understand the context.

    Interactive Simulation: A button that, when clicked, runs a JavaScript function to simulate the C14 dating process for the predefined samples.

    Dynamic Results: The output shows the "true" known dates alongside the newly generated, simulated C14 measurements, which change with each click to represent measurement variability.

    Modern Design: The page is styled using Tailwind CSS for a clean, responsive, and professional look.

You can now open the oxcal-simulation.html file in any web browser to see the model in action.
