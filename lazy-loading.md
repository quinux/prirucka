Lazy loading
============

<!--
TODO
- unveil.js fold problém: buď musím nastavit velký treshold (600), ale pak nevidím ani animaci nebo se na vysokých displejích nezobrazují obrázky cca ve druhé půlce výšky
- unveil.js a iOS scrolling - opravdu se musí načítat až po skončení scrollování?
-->


Ve světě webového frontendu označuje [Lazy loading](http://cs.wikipedia.org/wiki/Lazy_loading) techniku, která zajistí načtení obsahu stránky až ve chvíli kdy jej uživatel potřebuje.

Nejčastěji lazyloading (neboli odložené načítání) aplikujeme na obrázky až ve chvíli kdy na ně uživatel nascrolluje. V tomhle příkladu čekají na načtení všechny obrázky mimo viewport:

<img class="picture" src="content/schemes/lazyloading.jpg" width="1000" height="575" alt="Lazy loading">

## K čemu je to dobré?

Ke zrychlení načítání stránky. Lazyloading nám ušetří jak datový objem tak počet requestů. Z pohledu responzivního webdesignu jde o nepostradatelnou techniku.

## Načítání obrázků s Unveil.js

Odložené načítání má mnoho [různých variant](http://jecas.cz/lazy-loading-obrazky), mně se z mnoha důvodů osvědčilo použití [Unveil.js](https://github.com/luis-almeida/unveil):

	<img src="bg.png" data-src="img1.jpg" alt="…">

Javascript hlídá scrollování stránky a když uživatel naroluje k obrázku, zkopíruje se obsah `data-src` do `src`.

Aby to bylo dokonale přístupné, musíte ještě uvést variantu pro čtečky a další scénáře kdy se javascript nenačte:

	<img src="bg.png" data-src="img1.jpg" alt="…">
	<noscript>
	  <img src="img1.jpg" alt="…">
	</noscript>

Pak už zbývá jen inicializace:

	$(document).ready(function() {
	  $("img").unveil();
	});

V dokumentaci Unveil.js jsou vidět i další možnosti. Umí totiž:

* na vysokokapacitním displeji načíst alternativní obrázek (`data-src-retina="img2-retina.jpg"`)
* dát lazyloadingu náskok, aby se obrázek nezačal načítat až ve chvíli kdy na něj uživatel naroluje (parametr [Treshold](https://github.com/luis-almeida/unveil#threshold))
* pomocí [Callbacku](https://github.com/luis-almeida/unveil#callback) přidat třeba animaci, která nám obrázek při načtení „vyfejduje“ (interakce se stránkou pak působí lépe)

Důležitý tip: řada webařů (včetně autora Unveil [v demonstračním příkladu](http://luis-almeida.github.io/unveil/)) propadla vášnivé lásce k preloaderům a do `src=""` jako výchozí obrázek dává takovýten točící se kolovrátek. Nedělejte to prosím.  Na uživatele působí daleko lépe, když tam před načtením obrázku vidí barevnou plochu mírně odlišenou od barvy pozadí.

## Na co všechno se lazyloading ještě může hodit?

V době responzivního webdesignu a pomalých mobilních připojení mu předpovídám velkou budoucnost.

Mě osobně se bude hodit na načítání jakéhokoliv obsahu náročnějšího na rychlost načítání stránky — kromě vlastních obrázků nebo videí je to embedovaný obsah třetí strany (Disqus komentáře, Youtube, sdílecí tlačítka). Nicméně tipuji, že pro některé weby bude zajímavé jej používat například i pro načtení složitější verze textového obsahu.


## Příklad k vyzkoušení

Demonstrace použití Unveil.js s fadeout animací načtených obrázků.

<p data-height="270" data-theme-id="502" data-slug-hash="ILhbK" data-user="machal" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/machal/pen/ILhbK'>CSS3 Border Image</a> by Martin Michálek (<a href='http://codepen.io/machal'>@machal</a>) on <a href='http://codepen.io'>CodePen</a></p>
<script async src="http://codepen.io/assets/embed/ei.js"></script>

## Zajímavé odkazy

* Bohumil Jahoda má o [využití lazyloadingu](http://jecas.cz/lazy-loading) celou sérii článků
* [jQuery Waypoints](http://imakewebthings.com/jquery-waypoints/) nedělá lazyloading samotný, jen umí spouštět různé akce když uživatel nascrolluje na nějaké místo stránky
* [Ajax Include Pattern](https://github.com/filamentgroup/Ajax-Include-Pattern/) pro lazyloading obsahu

