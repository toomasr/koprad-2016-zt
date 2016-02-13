# Kobraste külaskäik ZeroTurnaroundis 2016
Kobraste Külaskäik

## JavaScript Kaardi Demo

Selle praktilise osa jooksul ehitame väikese kaardirakenduse.

![Map demo screenshot](https://cloud.githubusercontent.com/assets/426039/13016688/ef6a8350-d1c8-11e5-8db2-79c3d606192d.png)

Rakenduse leiad siit kaustas "[js-map-demo](https://github.com/toomasr/koprad-2016-zt/tree/master/js-map-demo)". Programmeerimiskeeleks on jälle JavaScript, millega oleme natukene juba tutvunud.

Me ei kirjuta kogu funktsionaalsust nullist, vaid kasutame olemasolevat teenust. Kaart ise tuleb [Google Maps](https://maps.google.com) poolt ning me ehitame selle peal oma funktsionaalsuse.

Google Maps pakkub programeerijatele APIt -- application program interface, mille kaudu ka sinu programmid saavad selliseid kaarte kasutada. Selle kirjelduse leiad [Google Maps dokumentatsiooni lehel](https://developers.google.com/maps/documentation/javascript/).

Programmi kirjutamist me ei alusta nullist ka, vaid kasutame Google poolt pakutud näidiskoodi, mida me muudame enda sobivamaks. Näidised on vabalt kättesaadavad [Google Maps näidete](https://developers.google.com/maps/documentation/javascript/examples/) alt.

Näiteks me valime [lihtsa kaardi näidise](https://developers.google.com/maps/documentation/javascript/examples/map-simple), valime kaardi all nupu: "JavaScript + HTML" ning kopeerime kogu tekstivälja sisu redaktorisse:

![How to get Google maps Javascript sample code](https://cloud.githubusercontent.com/assets/426039/13016687/ef5df946-d1c8-11e5-8879-74fff2ef7feb.png)

Lisaks meil oleks vaja kaardile õhupalle (markereid) saada, joonistada 1 joon ning lisada paneel tekstiga. Need elemendid leiad ja saad muuta sobivamaks siin näidistes:
* [Kaardile klikkimise haldamine](https://developers.google.com/maps/documentation/javascript/examples/event-simple)
* [Õhupallid](https://developers.google.com/maps/documentation/javascript/examples/marker-labels)
* [Jooned](https://developers.google.com/maps/documentation/javascript/examples/polyline-simple)

Kauguse arvutamiseks kasutame ühe kavalat geomeetrilise funktsiooni, mis olemas samas JavaScripti teegis.

Kui sa hakkad ehitama oma kaardirakendust, mitte sellest failist, mis siin repositooriumis on, siis kaartide javascripti teegi saamiseks sul on vaja genereerida API_KEY. Seda saad teha [selle juhendi järgi](https://developers.google.com/maps/documentation/javascript/get-api-key), või kasutada minu oma, mis siin samas repositooriumis on: AIzaSyC5AN4eKkbANZ8_peU4dEudZ5Q5hKaXRCU

## A Song of JavaScript and Sanity: Liftide mäng

Viimane ülesanne mis me teeme on liftide mäng [play.elevatorsaga.com](http://play.elevatorsaga.com/). Selles mängus pead kirjutama koodi, mis juhib lifti või mitut ning transpordib inimesi edasi-tagasi.

![Elevator game screenshot](https://cloud.githubusercontent.com/assets/426039/13016686/ef43cce2-d1c8-11e5-80aa-e47ebb1bc7f5.png)

Kui sa käivitad liftide simulatsiooni, vaikimisi programmiga, siis esimesel tasemel ei suuda see program transportida 20 inimest ühe minutiga ning sa ei saa edasi järgmisesse tasemesse. Selleks et lift töötaks paremini on vaja selle koodi muuta. Kirjuta koodiga lahtri sisse see lõik:

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

Proovi nüüd simulatsioon taaskäivitada. Mõne katse jooksul peaks lift oskama 20 inimest transportida ning oleme järgmisel tasemel.

Juhendi kuidas lifti programmeerida, ning milliseid operatsioone ta oskab teha leiad [siin](http://play.elevatorsaga.com/documentation.html). 

Ära ole kurb, kui sul ei õnnestu mõne taseme liftide probleemi lahendada, need on päris keerulised. Tegelt. Mina ei saanud kaugemale kui 4.

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
