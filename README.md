# WebJET CMS - demo

Ukážkový (demo) projekt vo WebJET CMS, obsahuje integrované ukážkové šablóny [bare](https://github.com/webjetcms/templates-bare), [creative](https://github.com/webjetcms/templates-creative) a zálohu MariaDB databázy pre jednoduchšie spustenie. Vychádza z [basecms](https://github.com/webjetcms/basecms) projektu.

Pre spustenie požiadajte InterWay o prístup k WebJET Maven repozitáru a k zálohe ukážkovej databázy. Premenujte súbor ```gradle.sample.properties``` na ```gradle.properties``` a nastavte v ňom prihlasovacie údaje k WebJET Maven repozitáru.

Pre priame otvorenie vo VS Code ešte premenujte súbor ```.settings/default-org.eclipse.buildship.core.prefs``` na ```org.eclipse.buildship.core.prefs```. Ak vám VS Code zobrazuje chybu v projekte kliknite na menu ```View/Command Palette``` a do okna zadajte: ```Java: Clean Java Language Server Workspace```. To vyvolá reštart VS Code a znova inicializovanie Java prostredia. Po reštarte by vám projekt nemal zobrazovať žiaden červený priečinok/chybu.

## Obnovenie databázy

Ukážková databáza je pre server MySQL/MariaDB, stiahnutý ZIP súbor zálohy databázy rozbaľte. Vytvorte novú databázovú schému nasledovným príkazom (samozrejme zmeňte hodnotu hesla):

```sql
CREATE DATABASE democms_web DEFAULT CHARACTER SET utf8mb4 DEFAULT COLLATE utf8mb4_general_ci;
CREATE USER democms_web IDENTIFIED BY 'heslo';
GRANT ALL PRIVILEGES ON democms_web.* TO `democms_web`@`%`;
FLUSH PRIVILEGES;
```

a obnovte databázu príkazom:

```sh
mysql -u democms_web -p -h localhost democms_web < democms_web.sql
```

pričom nastavte namiesto hodnoty ```localhost``` adresu vášho MariaDB servera (ak nie je spustený lokálne).

Zadané heslo a adresu servera nastavte v súbore ```src/main/resources/poolman.xml```.

## Spustenie servera

Aplikačný server spustíte príkazom:

```
gradlew.bat appStart
```

a následne otvoríte stránku ```http://localhost``` v prehliadači.

## Integrované dizajnové šablóny

Tento projekt a záloha databázy obsahuje integráciu viacerých dizajnových šablón. Z nich môžete vychádzať pri tvorbe vášho projektu. Všetky sa nachádzajú v priečinku ```src/main/webapp/templates```.

### basecms

Základná šablóna ešte v starom JSP formáte, je tu len na ukážku a pochopenie používania v starých projektoch. Nové projekty odporúčame založiť výlučne na [Thymeleaf šablónach](http://docs.webjetcms.sk/v2022/#/frontend/thymeleaf/README).

### Bare

Základná šablóna v Thymeleaf formáte s využitím Bootstrap a npm modulov. Odporúčame ju použiť ako východiskovú šablónu pre prípravu vašich dizajnov. Umožňuje ľahko prototypovať zmeny aj bez spusteného WebJETu s využitím príkazu ```npm run start```.

Viac informácií v dokumentácii k [Bare šablóne](http://docs.webjetcms.sk/v2022/#/frontend/examples/template-bare/README).

### Creative

Jednostránková šablóna vychádzajúca z Bare založená na [Start Bootstrap - Creative](https://startbootstrap.com/theme/creative) šablóne.

Používanie/úprava súborov je podobná ako pri [Bare šablóne](http://docs.webjetcms.sk/v2022/#/frontend/examples/template-bare/README).

## Aktualizácia WebJETu

V súbore [build.gradle](build.gradle) je sekcia ```ext``` v ktorej je nastavená verzia WebJET CMS použitá v projekte:

```javascript
ext {
    webjetVersion = "2022.0-SNAPSHOT";
}
```

v ukážke je to verzia ```2022.0-SNAPSHOT```, pričom ```SNAPSHOT``` znamená, že sa jedná a najnovšiu verziu radu 2022. Najnovšia verzia môže vždy obsahovať rozpracovanú funkcionalitu, takže zvážte jej použitie podľa [zoznamu zmien](http://docs.webjetcms.sk/v2022/#/CHANGELOG).

Zoznam všetkých dostupných verzií nájdete na v dokumentácii v [sekcii inštalácia](http://docs.webjetcms.sk/v2022/#/install/README).

Ak používate SNAPSHOT verziu, nasledovným príkazom vykonáte znova načítanie najnovšej verzie z Maven servera:

```
gradlew.bat compileJava --refresh-dependencies --info
```

