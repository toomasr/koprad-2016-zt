# Kobraste külaskäik ZeroTurnaroundis 2016
Kobraste Külaskäik






## js-map-demo
Selle praktilise osa jooksul ehitame väikest kaardirakendust. 
![Map demo screenshot](https://dl.dropboxusercontent.com/u/17251331/js-map-demo-screenshot.png)

Rakenduse leiad selles repositooriumi kaustas "[js-map-demo](https://github.com/toomasr/koprad-2016-zt/tree/master/js-map-demo)". Programmeerimiskeeleks on jälle JavaScript, millega saime just natuke tuttavam. 

Samas, kuna see rakendus imiteerib reaalse tarkvara, ei kirjutame me kogu funktsionaalsust nullist, vaid kasutame olemasolevad teenused. Näiteks, kaart ise tuleb [Google Maps](maps.google.com) poolt, ning me ainult ehitame selle peal oma funktsionaalsust. 

Google Maps pakkub programeerijatele väga põhilist API -- application program interface, ehk rakenduse kasutajaliides, mille kaudu programmid saavad kaarte kasutada. Selle kirjelduse leiad [Google Maps dokumentatsiooni lehel](https://developers.google.com/maps/documentation/javascript/). 

Programmi kirjutamist me ei alusta nullist ka, vaid kasutame Google poolt pakutud näidiskoodi, mida me muudame enda sobivamaks. Näidised on vabalt kättesaadavad siin https://developers.google.com/maps/documentation/javascript/examples/.

Näiteks me valime [lihtsa kaardi näidise](https://developers.google.com/maps/documentation/javascript/examples/map-simple), valime kaardi all nupu: "JavaScript + HTML" ning kopeerime kogu tekstivälja sisu redaktorisse: 

![How to get Google maps Javascript sample code](https://dl.dropboxusercontent.com/u/17251331/how-to-use-maps-samples.png)

Lisaks meil oleks vaja kaardile õhupalle (markerid) saada, joonistada 1 joont, ning lisada paneeli tekstiga. Need elemendid saad leida ning muuta sobivamaks nendest näidisest:
* [Kaardile klikkimise haldamine](https://developers.google.com/maps/documentation/javascript/examples/event-simple)
* [Õhupallid](https://developers.google.com/maps/documentation/javascript/examples/marker-labels)
* [Jooned](https://developers.google.com/maps/documentation/javascript/examples/polyline-simple)

Kauguse arvutamiseks kasutame ühe kavala geomeetrilise funktsiooni, mida saame samast JavaScripti teegist, mis pakub ka kaarti ise.

Kui sa hakkad ehitama oma kaardirakendust, mitte sellest failist, mis siin repositooriumis on, siis kaartide javascripti teegi saamiseks sul on vaja genereerida API_KEY. Seda saad teha [selle juhendi järgi](https://developers.google.com/maps/documentation/javascript/get-api-key), või kasuta minu oma, mis siin samas repositooriumis on: AIzaSyC5AN4eKkbANZ8_peU4dEudZ5Q5hKaXRCU


