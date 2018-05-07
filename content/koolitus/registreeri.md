---
title: "Registreeri koolitusele"
date: 2018-05-03T12:39:20+03:00
hidden: true
---

<form name="register-training" action="/koolitus/registreeritud" netlify>
    <section>
        <input type="radio" onchange="handleSelection()" name="buy-type" value="package" required>Soovin paketi osta<br>
        <input type="radio" onchange="handleSelection()" name="buy-type" value="days">Soovin ise päevi kombineerida<br>
        <br>
        <input type="radio" onchange="handleSelection()" name="location" value="tallinn" required>Tulen Tallinnasse<br>
        <input type="radio" onchange="handleSelection()" name="location" value="tartu">Tulen Tartusse<br>
    </section>
    <div class="selection selection-all">
        <section>
            <h2>Vali teema ja kuupäev</h2>
            <p>Üks päev on 9-17 ehk 8h pikk (koos pausidega).</p><br>
            <div class="selection selection-package">
                <p>Pakettide sisu leiad <a target="_blank" href="/koolitused">koolituste pealehelt</a></p>
                <div class="selection selection-tallinn">
                    <h3>Tallinn</h3>
                    <input type="radio" onchange="handlePackage()" value="tallinn-package1-6.6" name="package" required>Pakett 1 - 6.-7. juuni<br>
                    <input type="radio" onchange="handlePackage()" value="tallinn-package2-6.6" name="package">Pakett 2 - 6.-8. juuni<br>
                    <input type="radio" onchange="handlePackage()" value="tallinn-package1-6.9" name="package">Pakett 1 - 9.-10. juuni<br>
                </div>
                <div class="selection selection-tartu">
                    <h3>Tartu</h3>
                    <input type="radio" onchange="handlePackage()" value="tartu-package1-6.27" name="package" required>Pakett 1 - 27.-28. juuni<br>
                    <input type="radio" onchange="handlePackage()" value="tartu-package2-6.27" name="package">Pakett 2 - 27.-29. juuni<br>
                    <input type="radio" onchange="handlePackage()" value="tartu-package1-7.1" name="package">Pakett 1 - 30. juuni - 1. juuli<br>
                </div>
            </div>
            <div class="selection selection-days">
                <p>Päevade sisu leiad <a target="_blank" href="/koolitused">koolituste pealehelt</a></p>
                <div class="selection selection-tallinn">
                    <h3>Tallinn</h3>
                    <input type="checkbox" onchange="handleDay(this)" value="tallinn-day1-6.6" name="day">Päev 1 - 6.6.2018 (K)<br>
                    <input type="checkbox" onchange="handleDay(this)" value="tallinn-day2-6.7" name="day">Päev 2 - 7.6.2018 (N)<br>
                    <input type="checkbox" onchange="handleDay(this)" value="tallinn-day3-6.8" name="day">Päev 3 - 8.6.2018 (R)<br>
                    <br>
                    <input type="checkbox" onchange="handleDay(this)" value="tallinn-day1-6.9" name="day">Päev 1 - 9.6.2018 (L)<br>
                    <input type="checkbox" onchange="handleDay(this)" value="tallinn-day2-6.10" name="day">Päev 2 - 10.6.2018 (P)<br>
                    <br>
                </div>
                <div class="selection selection-tartu">
                    <h3>Tartu</h3>
                    <input type="checkbox" onchange="handleDay(this)" value="tartu-day1-6.27" name="day">Päev 1 - 27.6.2018 (K)<br>
                    <input type="checkbox" onchange="handleDay(this)" value="tartu-day2-6.28" name="day">Päev 2 - 28.6.2018 (N)<br>
                    <input type="checkbox" onchange="handleDay(this)" value="tartu-day3-6.29" name="day">Päev 3 - 29.6.2018 (R)<br>
                    <br>
                    <input type="checkbox" onchange="handleDay(this)" value="tartu-day1-6.30" name="day">Päev 1 - 30.6.2018 (L)<br>
                    <input type="checkbox" onchange="handleDay(this)" value="tartu-day2-7.1" name="day">Päev 2 - 1.7.2018 (P)<br>
                </div>
            </div>
            <br>
            <h3 id="price"></h3>
            <input id="price-input" name="user-price" value="0" class="hidden">
            <script>
                function handleSelection() {
                    const buyTypeRadio = document.querySelector('input[name=buy-type]:checked')
                    const locationRadio = document.querySelector('input[name=location]:checked')
                    if (!buyTypeRadio || !locationRadio) return
                    // Reset elements
                    document.querySelectorAll('.selection').forEach(function(item){item.style.display = 'none'})
                    // Show elements
                    const buyType = buyTypeRadio.value
                    const location = locationRadio.value
                    if (buyType && location)
                        document.querySelector('.selection-all').style.display = 'block'
                    if (location === "tallinn")
                        document.querySelectorAll('.selection-tallinn').forEach(function(el){el.style.display = 'block'})
                    else
                        document.querySelectorAll('.selection-tartu').forEach(function(el){el.style.display = 'block'})
                    if (buyType === "package")
                        document.querySelectorAll('.selection-package').forEach(function(el){el.style.display = 'block'})
                    else
                        document.querySelectorAll('.selection-days').forEach(function(el){el.style.display = 'block'})
                }
                function handleDay(element) {
                    var location = element.value.split("-")[0]
                    var countDays = 0
                    document.querySelectorAll('input[name=day]:checked').forEach(function(input) {
                        var val = input.value
                        if (!val.includes(location)) return
                        countDays++
                    })
                    var price = 0
                    var step = 100
                    for (var i = 0; i < countDays; i++) {
                        price += step
                        step -= 30
                        if (step < 50)
                            step = 50
                    }
                    setPrice(price)
                }
                function handlePackage() {
                    var packageElement = document.querySelector('input[name=package]:checked').value
                    if (packageElement.includes('package1'))
                        setPrice(170)
                    else if (packageElement.includes('package2'))
                        setPrice(220)
                }
                function setPrice(price) {
                    document.querySelector('#price').innerHTML = "Kursuse hind kokku: " + price + "€"
                    document.querySelector('#price-input').value = price
                }
            </script>
            <style>
                .selection {
                    display: none;
                }
            </style>
        </section>
        <section>
            <h2>Isiklik info</h2>
            <p class="width">Nimi:</p><input name="name" required><br>
            <p class="width">E-mail:</p><input name="email" type="email" required><br>
            <p class="width">Telefon:</p><input name="tel"><p>(viimase hetke teavitused)</p><br>
            <p class="width">Soovitaja nimi:</p><input name="friend"><p>(Sõbra soodustus)</p><br>
        </section>
        <section>
            <h2>Lisainfo</h2>
            <input type="radio" name="pc" value="windows" required>Tulen Windowsi läpakaga<br>
            <input type="radio" name="pc" value="mac">Tulen Mac OSi läpakaga<br>
            <input type="radio" name="pc" value="linux">Tulen Linuxi läpakaga<br>
            <input type="radio" name="pc" value="none">Vajan läpakat<br>
            <br>
            <p class="width">Sõbra e-mail: </p><input name="friend-emails"><p>(saadame talle ka kutse)</p><br>
            <p class="width">Kommentaar:</p><input name="comments">
        </section>
        <section>
            <br>
            <input type="submit" value="Registreeri">
            <br><br>
            <p>Järgmiseks tuleb e-mailile arve, mille maksmisel kinnitatakse registratsioon.</p>
        </section>
    </div>
</form>