
# Oppdatering oktober 2019 - Offisielle data tilgjengelige

Offisielle data for 2020-kommunegrenser er tilgjengelig fra https://www.geonorge.no Da er det lite produktivt å ajourholde en uoffisell versjon i dette reposet. Jeg har derfor slettet en del datasett fra /geodata - mappen. 

Videre har jeg oppdatert en håndfull kommunenavn som ble noe 
annet enn vi trodde i fjor, samt retta feilen med at 
Vestland fylke ikke heter Vestlandet. 

# Kommune-og-regionreform 2017-2020

Kloning av Kyrre Moe's [utmerkede datasett](https://github.com/kyrrelm/kommune-og-regionreform). 

## Work in progress, usikker på sluttformat

Jeg løser et konkret behov for å "oversette" dagens fylke- og kommunestruktur til slik den blir etter 2020. P.t. litt usikker på 
hvordan jeg løser et par av spissfindighetene med grensejusteringer, og dermed også
hvordan mitt datasett til dette formålet blir seende ut. Muligens avviker dette på interessante
måter fra Kyrres opprinnelige data. 

## Delelinjer - kommuner som deles 

Tysfjord kommune deles i 2, og Snillfjord kommune deles i 3. 
 ~~Jeg har ikke funnet offisielle kartdata for de nye kommunegrensene. Derfor har jeg digitalisert ut fra tilgjengelige kart~~ . Ny versjon laget ved å finne overlapp mellom nye og gamle kommunegrenser hentet fra https://www.geonorge.no
 
  * [Tysfjord](https://www.arcgis.com/apps/webappviewer/index.html?id=cb62a943bb994d13aaf9312a0cc05739&extent=501730.1301%2C7503082.6721%2C664290.4552%2C7594099.5208%2C25833) ~~Her fant jeg en arcgis mapserver-tjeneste. Ingen vektordata, men effektivt å tegne etter alternativ D-linja.~~ Laget ved å kombinere nye og gamle kommunepolygon. 
  * [Snillfjord](https://www.snillfjord.kommune.no/hoering-nye-kommunegrenser-ved-deling-av-snillfjord-kommune.5948422-86835.html) ~~PDF, georefert ut fra tydelige knekkpunkt på 2018-kommunegrenser. Digitalisering er ikke like presis som for Tysfjord.~~ Laget ved å kombinere nye og gamle kommunepolygon. 

Ettersom vi nå har offisielle data er disse delelinjene på mange måter overflødige. Men de er likevel nyttige i en del "klipppe"-operasjoner, og inngår i en del dataflyt jeg trenger. 
  
Formatet er Geopackage, i kartprojeksjon UTM sone 33 (EPSG:25833), i ´geodata/deltekommuner.gpkg´. 



-------
# Kyrres orginale readme nedenfor 


### Savner du en oversiktlig json-struktur med endringer i fylkesnummer/kommunenummer som følge av kommune-og-regionreformen? Det gjorde jeg, derfor har jeg laget det! Jeg har laget et json-dokument med en mapping mellom gamle og nye fylkes og kommunenummer, samt eventuellle navneendringer.


Regionreformen og vedtatte fylkeskommunesammenslåinger fører til at over 270 kommuner må/har endre(t) kommunenummer frem mot 2020. Ssb, difi og statens kartverk tilbyr alle API'er som tildels gir deg informasjon om sammenslåingen. Men etter min erfaring tilbyr de ikke  all den informasjonen man trenger. Jeg har derfor opprettet dette git-repoet, som inneholder en mapping mellom alle gamle og nye fylkesnavn og fylkesnummer og gamle og nye kommunenavn og kommunenummernummer, samt datoen de trer i kraft som følge av reformen.

## Oppbygging av data

Dokumentet består av et array indelt etter fylker. Hvert fylke har et navn, fylkesnummer, og en dato som forteller når fylket, med tilhørende fylkesnummer, slår i kraft. For fylker som ikke er slått sammen (Møre og Romsdal, Rogaland og Nordland) er dato satt til 1.12.1946, som er første gang fylkes/kommunenummer ble benyttet (folketelling).

Alle fylker inneholder et array over alle kommuner i fylket. Kommuner inneholder følgende felter:
```
{
  "fra": "gammelt kommunenummer",
  "til": "nytt kommunenummer",
  "gammeltNavn": "navn på den gamle kommunen",
  "nyttNavn": "navn på den nye kommunene",
  "dato": "datoen sammenslåingen trer i kraft",
}
```
Alle fylker inneholder også et array med gamle fylker. Hvis fylket er et resultat av en sammenslåing av andre fylker finner man disse fylkene her på formen:
```
{
  "fra": "gammelt fylkesnummer",
  "til": "nytt fylkesnummer",
  "gammeltNavn": "navn på det gamle fylket",
  "nyttNavn": "navn på det nye fylket",
  "dato": "datoen sammenslåingen trer i kraft",
}
```

## Særtilfeller og annen info om datasettet

De fleste sammenslåingene trer i kraft 1.1.2020, men du vil også finne sammenslåinger som trer/tredde i kraft 1.1.2017 og 1.1.2018.

En del kommuner bytter også kommunenummer flere ganger i perioden 2017-2020. et eksempel på dette er Holmestrand:
```
1.1.2018: Holestrand 0702 --> Holmestrand 0715
1.1.2020: Holestrand 0715 --> Holmestrand 3802
```
Trondheim har spesielt mange slike tilfeller som et resultat av at Nord og Sør-Trøndelag slo seg sammen til Trøndelag i 1.1.2018.

### Kommuner som blir delt i flere nye kommuner
I all hovedsak har kommuner fått nytt komunenummer som følge av nye fylkesnummer, flytting fra et fylke til et annet eller sammenslåing av flere kommuner. Det finnes likevel tilfeller av kommuner som blir splittet i flere kommuner, som følge av at kommunen blir del av flere nye kommuner. Disse kommunene vil derfor forekomme flere ganger med samme fra verdi (i.e. gammelt kommunenummer). Disse kommunene er:

#### Snillfjord:
```
1.1.2020: Snillfjord 5012 --> Heim 5055
1.1.2020: Snillfjord 5012 --> Hitra 5056
1.1.2020: Snillfjord 5012 --> Orkland 5059
```
#### Tysfjor:
```
1.1.2020: Tysfjord 1850 --> Narvik 1806
1.1.2020: Tysfjord 1850 --> Hamarøy 1875
```

## Hvordan har datasettet blitt generert

Dataen er i all hovedsak generert ved å programatisk omforme regjeringens tabell over nye kommune- og fylkesnummer fra 2020, som man finner her:

https://www.regjeringen.no/no/aktuelt/nye-kommune--og-fylkesnummer-fra-2020/id2576659/?utm_source=www.regjeringen.no&utm_medium=rss&utm_campaign=Nye%20kommune-%20og%20fylkesnummer%20fra%202020

I tillegg er det gjort en del manuell masering dataen for å få et homogent datasett, og for å få med sammenslåingersom skjedde i 2017.

## Andre kilder som har blitt benyttet

https://www.regjeringen.no/no/tema/kommuner-og-regioner/kommunereform/Hvorfor-kommunereform/nye-kommuner/id2470015/

https://no.wikipedia.org/wiki/Liste_over_norske_kommunenummer

https://no.wikipedia.org/wiki/Fylkesnummer

## Feil i datasettet?

Dataen er i all hovedsak programatisk generert. Det er likevel gjort en del manuelt arbeid, så det er mulig det er feil i datasettet som jeg ikke har oppdaget. Hvis du finner en feil, vær så snill og opprett et `issue` på det, så skal jeg fikse det fortløpende. 

