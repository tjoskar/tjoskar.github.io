---
layout: post
title: Automatisera belysningen
published: True
date: 2014-09-14 12:25:00
categories: []
tags: []
---

Jag har sedan en tid tillbaka funderat på hur man kan automatisera belysningen i lägenheten. Egentligen borde det inte vara så svårt, tekniken finns ju redan och är busenkel. Efter lite experiment visade det sig också vara fallet – det är enkelt.

Så vad vill jag automatisera?

- När solen går ner ska lamporna i fönstren tändas. Timer till all ära men det är för omständligt att behöva gå runt och ändra tiden på flera ställen. Det behövs ett centralt system, speciellt i ett land som Sverige där dagens läng ändras i rasande takt på hösten och våren. Vidare vill man att alla lampor ska tänds samtidigt, nämligen när solen går ner.
- När man kommer hem ska man inte behövas mötas av mörker. Det skall lysa i lägenheten även om ingen är hemma men samtidigt ska lamporna inte lysa i onödan. Dvs. straxt innan man kommer hem ska hallampan tändas.
- Jag tycker att det ska vara mörkt i hela lägenheten när man kollar på film. Jag vet att det är lite delad mening om denna uppfattning men jag tycker om känslan som man får när man är på bio; det är totalt mörker i de mörka scenerna och i de ljusa scenerna blir man nästan bländad. Jag glömmer aldrig känslan när jag såg Matrix (revolution eller reloaded) på bio och biosalongen var totalt kolsvart till följd av den dystra och tröstlösa känslan som regissören försökte förmedla, tills nästa scenväxling när hela salongen lystes upp av en förvånansvärt ljus och vit bioduk. Detta är en känsla som jag vill återskapa hemma. När man sedan slutar kolla på filmen ska lamporna tändas igen (förutsatt att solen har gått ner).

Jag har valt att använda mig av tellstick tillsammans med en raspberry pi som står på dygnet runt. Hur man sätter upp detta system kan man läsa [här](/2014/09/02/setup-tellstick-on-raspberry-pi.html):

Om vi börjar med att lamporna ska tändas när solen går ner. Man skulle kunna lösa detta genom att ett script kollar om solen har gått ner med jämna mellanrum. Ex:

PHP:
{% highlight php %}
// PHP has to much functionality for its own good
time() < date_sunset(time(), SUNFUNCS_RET_TIMESTAMP)
{% endhighlight %}

Python:
{% highlight python %}
from astral import Astral

astral = Astral()
astral.solar_depression = 'civil'
city = astral['Stockholm']
sunset_time = city.sun(date=datetime.datetime.now(), local=True)['sunset']
now = datetime.datetime.now(pytz.timezone(city.timezone))
return now > sunset_time
{% endhighlight %}

Dock resulterar detta i att man måste spara ett state som avgör om lamporna har tänds (så att tellstick enheten inte konstant skickar ut signaler om att lamorna ska tändas). Detta skulle man självfallet enkelt kunna lösa igenom att skapa en evighets loop tillsammans med <code>sleep</code> (eller en databas). Jag har dock valt att förenkla det hela genom att använda mig av [IFTTT](https://ifttt.com). Tyvärr stödjer inte IFTTT webhooks vilket gör att det inte finns något naturligt sätt att kommunicera med min raspberry pi. Lyckligtvis finns det personer som har stött på problemet innan mig och som har [löst det](https://github.com/captn3m0/ifttt-webhook). IFTTT har stöd för att skapa inlägg i wordpress bloggar och stödjer därför <code>xmlrpc</code> och genom att skapa ett fejkat xmlrpc-gränssnitt kan man skicka olika anrop till diverse webbaserade enheter.

Det första som behövs är alltså att skapa ett webb-api för tellstick. Detta valde jag att göra i python och med ramverket flask. Ex:
{% highlight bash %}
$ virtualenv tellstick_server # setup python:
$ source tellstick_server/bin/activate
$ pip install Flask Astral
$ mkdir tellstick_server/www
$ touch tellstick_server/www/app.py
{% endhighlight %}

Min <code>app.py</code> ser ut som följande:
{% gist tjoskar/ce061987249866b64c39 %}

Sedan kan man skapa ett recept i IFTTT som liknar detta:
<a href="https://ifttt.com/view_embed_recipe/203399-turn-on-lights-on-sunset" target = "_blank" class="embed_recipe embed_recipe-l_24" id= "embed_recipe-203399"><img src= 'https://ifttt.com/recipe_embed_img/203399' alt="IFTTT Recipe: Turn on lights on sunset connects weather to wordpress" width="370px" style="max-width:100%"/></a>

Eftersom vi nu har en fungerande länk mellan IFTTT och raspberry pi kan man fortsätta använda IFTTT för att tända lampan/lamporna när man kommer hem:
<a href="https://ifttt.com/view_embed_recipe/203400-turn-on-hallway-lamp-when-i-come-home" target = "_blank" class="embed_recipe embed_recipe-l_37" id= "embed_recipe-203400"><img src= 'https://ifttt.com/recipe_embed_img/203400' alt="IFTTT Recipe: Turn on hallway lamp when I come home connects android-location to wordpress" width="370px" style="max-width:100%"/></a>

Man kan även lägga till ett recept för att släcka vissa lampor när man lämnar lägenheten:
<a href="https://ifttt.com/view_embed_recipe/203403-turn-off-hallway-lamp-when-i-leave-home" target = "_blank" class="embed_recipe embed_recipe-l_39" id= "embed_recipe-203403"><img src= 'https://ifttt.com/recipe_embed_img/203403' alt="IFTTT Recipe: Turn off hallway lamp when I leave home connects android-location to wordpress" width="370px" style="max-width:100%"/></a>


Jag använder XBMC för att titta på film och serier. Tur nog har XBMC ett ganska bra gränssnitt för att skapa plugin (även om de inte följer [PEP8](http://legacy.python.org/dev/peps/pep-0008/). En sak om många kanske inte känner till är att man kan exekvera kod vid uppstart av XBMC utan att behöva skapa ett helt plugin. När XBMC startas exekveras .xbmc/autoexec.py (för linux) i en XBMC context vilket innebär att man bland annat har tillgång till <code>xbmc</code> biblioteket.

Min .xbmc/autoexec.py ser ut på följande vis:
{% highlight python %}
execfile("custom/path/XBMC_light.py")
{% endhighlight %}

Och min XBMC_light.py ser ut som följande:
{% gist tjoskar/0569d7c6469c6b8511c8 %}

Där har ni det. Hur man med ganska enkla metoder kan automatisera belysningen hemma.

<script async type="text/javascript" src= "//ifttt.com/assets/embed_recipe.js"></script>
