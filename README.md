# Kobraste külaskäik ZeroTurnaroundis 2016
Kobraste Külaskäik






## js-map-demo
Selle praktilise osa jooksul ehitame väikest kaardirakendust. 
![Map demo screenshot](https://cloud.githubusercontent.com/assets/426039/13016688/ef6a8350-d1c8-11e5-8db2-79c3d606192d.png)

Rakenduse leiad selles repositooriumi kaustas "[js-map-demo](https://github.com/toomasr/koprad-2016-zt/tree/master/js-map-demo)". Programmeerimiskeeleks on jälle JavaScript, millega saime just natuke tuttavam. 

Samas, kuna see rakendus imiteerib reaalse tarkvara, ei kirjutame me kogu funktsionaalsust nullist, vaid kasutame olemasolevad teenused. Näiteks, kaart ise tuleb [Google Maps](https://maps.google.com) poolt, ning me ainult ehitame selle peal oma funktsionaalsust. 

Google Maps pakkub programeerijatele väga põhilist API -- application program interface, ehk rakenduse kasutajaliides, mille kaudu programmid saavad kaarte kasutada. Selle kirjelduse leiad [Google Maps dokumentatsiooni lehel](https://developers.google.com/maps/documentation/javascript/). 

Programmi kirjutamist me ei alusta nullist ka, vaid kasutame Google poolt pakutud näidiskoodi, mida me muudame enda sobivamaks. Näidised on vabalt kättesaadavad siin https://developers.google.com/maps/documentation/javascript/examples/.

Näiteks me valime [lihtsa kaardi näidise](https://developers.google.com/maps/documentation/javascript/examples/map-simple), valime kaardi all nupu: "JavaScript + HTML" ning kopeerime kogu tekstivälja sisu redaktorisse: 

![How to get Google maps Javascript sample code](https://cloud.githubusercontent.com/assets/426039/13016687/ef5df946-d1c8-11e5-8879-74fff2ef7feb.png)

Lisaks meil oleks vaja kaardile õhupalle (markerid) saada, joonistada 1 joont, ning lisada paneeli tekstiga. Need elemendid saad leida ning muuta sobivamaks nendest näidisest:
* [Kaardile klikkimise haldamine](https://developers.google.com/maps/documentation/javascript/examples/event-simple)
* [Õhupallid](https://developers.google.com/maps/documentation/javascript/examples/marker-labels)
* [Jooned](https://developers.google.com/maps/documentation/javascript/examples/polyline-simple)

Kauguse arvutamiseks kasutame ühe kavala geomeetrilise funktsiooni, mida saame samast JavaScripti teegist, mis pakub ka kaarti ise.

Kui sa hakkad ehitama oma kaardirakendust, mitte sellest failist, mis siin repositooriumis on, siis kaartide javascripti teegi saamiseks sul on vaja genereerida API_KEY. Seda saad teha [selle juhendi järgi](https://developers.google.com/maps/documentation/javascript/get-api-key), või kasuta minu oma, mis siin samas repositooriumis on: AIzaSyC5AN4eKkbANZ8_peU4dEudZ5Q5hKaXRCU

## A Song of JavaScript and Sanity: Liftide mäng

Viimane ülesanne mis me teeme on liftide mäng: [play.elevatorsaga.com](http://play.elevatorsaga.com/). Selles mängus pead kirjutama koodi, mis juhib lifti või mitu, ning transporteerib inimesi edasi-tagasi. 

![Elevator game screenshot](https://cloud.githubusercontent.com/assets/426039/13016686/ef43cce2-d1c8-11e5-80aa-e47ebb1bc7f5.png)

Kui sa käivitad liftide simulatsiooni, vaikimisi programmiga, siis esimesel tasemel ei suuda see program transporteerida 20 inimest ühe minutiga, ning sa ei saa edasi. Selleks et lift töötaks parem on vaja selle koodi muuta. Kirjuta koodiga lahtri sisse see lõik:

```javascript
{
    init: function(elevators, floors) {
        var elevator = elevators[0]; // Let's use the first elevator

        // Whenever the elevator is idle (has no more queued destinations) ...
        elevator.on("idle", function() {
            // let's go to all the floors (or did we forget one?)
            elevator.goToFloor(0);
            elevator.goToFloor(1);
            elevator.goToFloor(2);
        });
    },
    update: function(dt, elevators, floors) {
        // We normally don't need to do anything here
    }
}
```

Proovi nüüd simulatsiooni taaskäivitada. Mõne katse jooksul peaks lift oskama 20 inimest transporteerida, ning oleme järgmisel tasemel. 

Juhendi kuidas lifti programmeerida, ning millised operatsioonid ta oskab teha leiad [siin](http://play.elevatorsaga.com/documentation.html). 

Ära ole kurb, kui sul ei õnnestu mõni taseme liftide probleemi lahendada, need on päris keerulised. Tegelt. Ma ei saanud kaugemale kui 4. 

Liftide mängu arendaja lahendus on umbes selline, mis näitab kuidas kasutada korruste ning liftide sündmusi:

```javascript
{
    init: function(elevators, floors) {
        var rotator = 0; //järgmise lifti indeks
        for (i = 0; i < floors.length; i++) {
          var floor = floors[i]; //võta korrus
          floor.on("up_button_pressed down_button_pressed", function() { // kui korruse on nupp vajutatud
            var elevator = elevators[(rotator++) % elevators.length]; // võta järgmine elevator
            elevator.goToFloor(floor.level); // saata elevator too korrusele
          }); 
        }
        for (i = 0; i < elevators.length; i++) { // iga lifti jaoks
          var elevator = elevators[i]; // võta lift
          elevator.on("floor_button_pressed", function(floorNum) { // kui liftis on nupp vajutatud
            elevator.goToFloor(floorNum); // mine too korrusele
          });
          elevator.on("idle", function() { // kui ei lähe praegu kuhugi
            elevator.goToFloor(0); // sõida 0-korrusele.
          });
        }
    },
    update: function(dt, elevators, floors) {
    }
}
```
