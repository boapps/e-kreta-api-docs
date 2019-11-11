
# e-Kréta API dokumentáció

*Krétás API-knak nem hivatalos gyűjteménye*

Ezen lekérdezések nagy részét egy [SSL Capture](https://play.google.com/store/apps/details?id=com.minhui.networkcapture) nevű Androidos alkalmazással szereztem.
A webes KRÉTA API dokumentációját [itt találod](https://github.com/Xerren09/eKreta-WebAPI-documentation). ([Xerren09](https://github.com/Xerren09) készítette)

## Más, nem hivatalos Krétás projektek:
 * [Spongyarend](http://www.sceurpien.com/spongyarend/) Ingyenes órarend generátor alkalmazás a KRÉTA rendszer állományaiból
 * [eFilc](https://github.com/bru02/eFilc) Webes Kréta kliens diákoknak, Toldys extrákkal ([weboldal](https://www.bru02.tk/e-filc/))
 * [KFGnaplo](https://github.com/hexadec/KFGnaplo) Android kliens a Krétához, ami értesítéseket küld az új jegyekről, helyettesítésekről (még csak a Karinthy Frigyes Gimnáziumhoz van)
 * [Szivacs Naplo](https://github.com/boapps/Szivacs-Naplo) Android kliens a Kréta rendszerhez
 * [Kréti](https://github.com/szilardhuber/kreta-bot) Bot, ami Krétás értesítéseket küld egy chatbe
 * [kfgApp](https://github.com/baltitenger/KFGapp) Egy app a Karinthy Gimnáziumhoz, Kréta API-t is használ
 * [CardPass](https://github.com/Xerren09/CardPass-Entry) Beléptetőkártyás rendszer, ami a Kréta rendszert használja, hogy eldöntse késett -e a tanuló
 * [eKreta@thegergo02](https://github.com/thegergo02/eKreta-thegergo02) Cinnamon asztali környezethez egy desklet, megjeleníti a jegyeidet és a Kréta API-t használja
 * [gKréta](https://github.com/thegergo02/gKreta) Az [Electron](https://electronjs.org/) frameworkon alapuló kliens a KRÉTA rendszerhez.
 * [eSzivacs-PC](https://github.com/pepyta/eSzivacs-PC) [Electron](https://electronjs.org/)on alapuló PC-s kliens.
 * [e-kreta-cli](https://github.com/szekelymilan/e-kreta-cli) Egy asztali, konzolos (terminál) kliens a Krétához.
 * [zsírkréta](https://github.com/forcemagic/zsirkreta) Egy Androidos kliens a Krétához
 * [e-Vonalzó](https://github.com/danielszenasi/eVonalzo) Még egy webes Kréta kliens [weboldal](https://evonalzo.netlify.com/sign-in) [graphql api](https://evonalzo.netlify.com/.netlify/functions/kreta)
 * [gokreta](https://github.com/thegergo02/gokreta) Go implementációja a Kréta API-nak.
 * [kosz](https://github.com/Gbr22/kosz) Nem hivatalos Kréta webapp. https://kreta.gbr22.me/
 * [CursedKreta](https://github.com/thegergo02/cursedkreta) Konzolos (terminál) kliens a Krétához, go-ban.
 

## Figyelem! Ismert problémák az API-val:

**A KRÉTA API-ja nem követi az HTTP standardot:**

Ez azért nem célszerű, mert így egyes nyelvekekben (pl.: Swift, Dart), amikben nem lehet kisbetűs Headert beállítani, mert bizonyos lekérdezések (iskolák listája) nem lehetségesek a KRÉTA hivatalos API-jából.
Többek között emiatt (és a nem kikapcsolható reklámok miatt) azt a megoldást találtuk ki, hogy az iskolák listáját oránként frissítjuk és [saját szerveren](https://e-szivacs.org) hosztoljuk.

**Nem célszerű használat és a *client_id*:**
Megkeresésünkre a Krétások azt írták e-mailbe, hogy visszafejtésből megszerzett kulcsot nem lenne szabad használni, ami szerintünk azért sem helyes, mert azokhoz a lekérésekhez mindenki ugyanazt a kulcsot használja, továbbá ezt a kulcsot nem csak visszafejtésből lehet megszerezni, hanem konkrétan midnen bejelentkezéskor  a KRÉTA által kiadott token tartalmazza ugyanezt a *client_id*-nek hívott kulcsot.

**Kötelező header**

Jelenlegi tudásunk szerint a KRÉTA blokkolja azokat a lekérdezéseket, amelyek *User-Agent*-je egyezik a "szivacs_naplo" szöveggel, akár nagybetűvel, akár kisbetűvel. Ez a blokkolás viszont (még) nem az összes Krétás szerverre vonatkozik, csak a "2-kaffee" nevű, leggyakrabban a "klik"-el kezdődű intézménykódú iskolák által használt szeverrel. Magyarul kb. 80%-a az iskolálnak Szivacs Naplóval blokkolva van.
**Mindezek ellenéré tudtunkal még nincsen kötelező header.**


## Iskolák lekérdezése
#### Az összes iskola ahol be van vezetve az e-Kréta:  
A kréta kérésére eltávolítottam ezt az információt, mert a titkos api kulcsuk benne van.

#### Egy iskola adatainak lekérése ID alapján:
A kréta kérésére eltávolítottam ezt az információt, mert a titkos api kulcsuk benne van.

#### A szerver válasza:  
```json
{
  "InstituteId": 3928,
  "InstituteCode": "appteszt",
  "Name": "PedApp Teszt Intézmény",
  "Url": "https://appteszt.ekreta.hu",
  "City": "Budapest",
  "AdvertisingUrl": "",
  "FeatureToggleSet": {
    "JustificationFeatureEnabled": "false"
  }
}
```

## API linkek lekérdezése
#### Lekéri a KRÉTA API linkjét:
```bash
curl http://kretamobile.blob.core.windows.net/configuration/ConfigurationDescriptor.json
```
* igazából egy mezei böngészőből is [végrehajtható](http://kretamobile.blob.core.windows.net/configuration/ConfigurationDescriptor.json)
* mire jó: ha az API linkje változna, akkor nem kell frissíteni az appot (rájött: [thegergo02](https://github.com/thegergo02))

#### A szerver válasza:  
```json
{
  "GlobalMobileApiUrlDEV": "https://kretaglobalmobileapiuat.ekreta.hu",
  "GlobalMobileApiUrlTEST": "https://kretaglobalmobileapitest.ekreta.hu",
  "GlobalMobileApiUrlUAT": "https://kretaglobalmobileapiuat.ekreta.hu",
  "GlobalMobileApiUrlPROD": "https://kretaglobalmobileapi.ekreta.hu"
}
```

* [Szénási Dániel](https://github.com/danielszenasi) felfedezte, hogy az uat-os api-n [van swagger oldal](https://kretaglobalmobileapiuat.ekreta.hu/swagger/index.html).

## Bejelentkezés
### Lekér egy Bearer kódot amit majd azonosításra fogunk használni később

```bash
curl --data "institute_code=xxxxxxxxxxx&userName=xxxxxxxxxxx&password=xxxxxxxxxxx&grant_type=password&client_id=919e0c1c-76a2-4646-a2fb-7085bbbf3c56" https://xxxxxxxxxxx.e-kreta.hu/idp/api/v1/Token -H "Content-Type: application/x-www-form-urlencoded; charset=utf-8" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```
* a mobil alkalmazás használja
* OAuth 2.0-es "hitelesítési keretrendszer" (felfedezte: [chriskoder](https://github.com/chriskoder))
* csak diák azonosítójával tudtam tesztelgetni, de elvileg a tanárinak is ugyanígy kéne működnie
* institute_code: az intézmény azonosítója
* userName: a felhasználó azonosítója
* password: a felhasználó jelszava
* grant_type: Az OAuth 2 kötelező paramétere, elvileg lehet:
   * Authorization Code
   * Implicit
   * **Password** `grant_type=password`
   * Client Credentials
   * Device Code
   * **Refresh Token** `grant_type=refresh_token`
   Amikor Password-el lekérjük az access_token-t akkor egy refresh_token-t is kapunk, amivel később a jelszó nélkül is frissíthetjük az access_token-ünket.
* client_id:  ¯\\_(ツ)_/¯

#### A szerver válasza:
```json
{
 "access_token":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
 "refresh_token": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
 ...
 }
```

### `access_token` frissítése
```bash
curl --data "institute_code=xxxxxxxxxxx&refresh_token=xxxxxxxxxxx&grant_type=refresh_token&client_id=919e0c1c-76a2-4646-a2fb-7085bbbf3c56" https://xxxxxxxxxxx.e-kreta.hu/idp/api/v1/Token -H "Content-Type: application/x-www-form-urlencoded; charset=utf-8" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```
* ugyanúgy működik, mint a Bearer kód lekérdezése
* itt is kapunk egy refresh_token-t amit újra fel tudunk használni a következő frissítéshez

## E-ügyintézéses üzenetek lekérése

* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)

```bash
curl https://eugyintezes.e-kreta.hu/integration-kretamobile-api/v1/kommunikacio/postaladaelemek/sajat -H "Authorization: Bearer XXXXXXXXXXXXXXX" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* Ha nincs még üzeneted, akkor egy 500-as hibakóddal egy `An error has occured!` üzenetet kapsz, ez itt "normális"

#### A szerver válasza:
```json
[
    {
        "azonosito": 0000,
        "isElolvasva": false,
        "isToroltElem": false,
        "tipus": {
            "azonosito": 1,
            "kod": "BEERKEZETT",
            "rovidNev": "Be\u00e9rkezett \u00fczenet",
            "nev": "Be\u00e9rkezett \u00fczenet",
            "leiras": "Be\u00e9rkezett \u00fczenet"
        },
        "uzenet": {
            "azonosito": 00000,
            "kuldesDatum": "0000-00-00T00:00:00",
            "feladoNev": "XXXXX XXXXXX",
            "feladoTitulus": "tan\u00e1r",
            "szoveg": "xxxxx",
            "targy": "xxxxxxxxx",
            "cimzettLista": [
                {
                    "azonosito": 0000000,
                    "kretaAzonosito": 0000000,
                    "nev": "XXXXXX",
                    "tipus": {
                        "azonosito": 4,
                        "kod": "OSZTALY_TANULO",
                        "rovidNev": "Oszt\u00e1ly - Tanul\u00f3",
                        "nev": "Oszt\u00e1ly - Tanul\u00f3",
                        "leiras": "Oszt\u00e1ly - Tanul\u00f3"
                    }
                }
		...
            ],
            "csatolmanyok": [
                {
                    "azonosito": 000000,
                    "fajlNev": "xxxxxxx.xxx"
                }
		...
	    ]
        }
    },
    ...
]

```
* Az "uzenet" nek a "szoveg"-e maximum 100 karakter hosszú lehet, ha a teljes szöveget akarjuk, akkor egy másik lekérdezés kell:

```bash
curl https://eugyintezes.e-kreta.hu/integration-kretamobile-api/v1/kommunikacio/postaladaelemek/0000 -H "Authorization: Bearer XXXXXXXXXXXXXXX" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* itt a 0000 a legkülső "azonosito"-t jelöli

A szerver válasza:

```json
    {
        "azonosito": 0000,
        "isElolvasva": true,
        "isToroltElem": false,
        "tipus": {
            "azonosito": 1,
            "kod": "BEERKEZETT",
            "rovidNev": "Be\u00e9rkezett \u00fczenet",
            "nev": "Be\u00e9rkezett \u00fczenet",
            "leiras": "Be\u00e9rkezett \u00fczenet"
        },
        "uzenet": {
            "azonosito": 00000,
            "kuldesDatum": "0000-00-00T00:00:00",
            "feladoNev": "XXXXX XXXXXX",
            "feladoTitulus": "tan\u00e1r",
            "szoveg": "xxxxx",
            "targy": "xxxxxxxxx",
            "cimzettLista": [
                {
                    "azonosito": 0000000,
                    "kretaAzonosito": 0000000,
                    "nev": "XXXXXX",
                    "tipus": {
                        "azonosito": 4,
                        "kod": "OSZTALY_TANULO",
                        "rovidNev": "Oszt\u00e1ly - Tanul\u00f3",
                        "nev": "Oszt\u00e1ly - Tanul\u00f3",
                        "leiras": "Oszt\u00e1ly - Tanul\u00f3"
                    }
                }
		...
            ],
            "csatolmanyok": [
                {
                    "azonosito": 000000,
                    "fajlNev": "xxxxxxx.xxx"
                }
		...
	    ]
        }
    }
```

## E-ügyintézéses üzenetek olvasottnak jelölése
* ettől az "isElolvasva" true lesz

```bash
curl https://eugyintezes.e-kreta.hu//integration-kretamobile-api/v1/kommunikacio/uzenetek/olvasott -H "Authorization: Bearer XXXXXXXXXXXXXXX" --data "{"isOlvasott":true,"uzenetAzonositoLista":[0000]}" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

## Bejelentett számonkérések lekérése

```bash
curl https://xxxxxxxxxx.e-kreta.hu/mapi/api/v1/BejelentettSzamonkeres?DatumTol=null&DatumIg=null -H "Authorization: Bearer XXXXXXXXXXXXXXX" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

A szerver válasza:

```json
[
  {
    "Uid": "0000",
    "Id": 0000,
    "Datum": "0000-00-00T00:00:00Z",
    "HetNapja": "Kedd",
    "Oraszam": 0,
    "Tantargy": "kémia",
    "Tanar": "Xxxxxx Xxxxxx",
    "SzamonkeresMegnevezese": "xxxxxxxxx",
    "SzamonkeresModja": "Írásbeli röpdolgozat",
    "BejelentesDatuma": "0000-00-00T00:00:00Z"
  }
  ...
]
```

* Uid: az id csak stringként (pl.: 2 -> "2")
* Hogy miért kell itt (és sehol máshol) a dátumon kívül egy HetNapja is, fogalmam sincs!
* az óraszámm (szerintem) azt mutatja, hogy hányadik órában lesz a dolgozat

## Felhasználó adatainak lekérdezése
### Jegyek, hiányzások, faliújság, szülő és osztályfőnök adatainak lekérdezése

* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Student?fromDate=xx-xx-xx&toDate=xx-xx-xx -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* fromDate: ettől a dátumtól kezdődően mutasson jegyeket, hiányzást és feljegyzést (a "Date" legyen nagyobb vagy egyenlő ennél)
* toDate: eddig a dátumig bezárólag mutasson jegyeket, hiányzást és feljegyzést (a "Date" legyen kissebb vagy egyenlő ennél)

Ha nem szeretnénk dátumhoz kötni, lehet az xx-xx-xx helyére null-t is írni vagy az egészet le lehet hagyni.

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Student?fromDate=null&toDate=null -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```
```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Student -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

#### A szerver válasza:
```json
{
  "StudentId": 0000000,
  "SchoolYearId": 0000000,
  "Name": "Xxxxx Xxxxx",
  "NameOfBirth": "Xxxxx Xxxxx",
  "PlaceOfBirth": "Xxxxx Xxxxx",
  "MothersName": "Xxxxx Xxxxx",
  "AddressDataList": [
    "Xxxxx Xxxxx, Xxxxx Xxxxx xxxx    "
  ],
  "DateOfBirthUtc": "0000-00-00T22:00:00Z",
  "InstituteName": "Xxxxx Xxxxx Xxxxx Xxxxx Xxxxx Xxxxx",
  "InstituteCode": "xxxxxxxxxxx",
  "Evaluations": [
    {
      "EvaluationId": 12345678,
      "Form": "Mark",
      "FormName": "Elégtelen (1) és Jeles (5) között az öt alapértelmezett érték",
      "Type": "MidYear",
      "TypeName": "Évközi jegy/értékelés",
      "Subject": "Xxxxxxxx",
      "SubjectCategory": null,
      "SubjectCategoryName": "Xxxxxxxx",
      "Theme": "xxxxxxx",
      "IsAtlagbaBeleszamit": true,
      "Mode": "Gyakorlati feladat",
      "Weight": "100%",
      "Value": "Jeles(5)",
      "NumberValue": 5,
      "SeenByTutelaryUTC": null,
      "Teacher": "Xxxxxxxx Xxxxxxxx",
      "Date": "2019-06-07T00:00:00",
      "CreatingTime": "2019-06-07T08:00:00.000",
      "Jelleg": {
        "Id": 1,
        "Nev": "Ertekeles",
        "Leiras": "Értékelés"
      },
      "JellegNev": "Ertekeles",
      "ErtekFajta": {
        "Id": 1,
        "Nev": "Osztalyzat",
        "Leiras": "Osztályzat"
      }
    },
    ...
  ],
  "SubjectAverages": [
    {
      "Subject": "Xxxxxxxx",
      "SubjectCategory": null,
      "SubjectCategoryName": "Xxxxxxxx xx Xxxxxxxx",
      "Value": 0.0,
      "ClassValue": 0.00,
      "Difference": -0.00
    },
    ...
  ],
  "Absences": [
    {
      "AbsenceId": 0000000,
      "Type": "Absence",
      "TypeName": "Hiányzás",
      "Mode": "Lesson",
      "ModeName": "Tanórai mulasztás",
      "Subject": "Xxxxxx xxxxx",
      "SubjectCategory": null,
      "SubjectCategoryName": "Xxxxxxxx xx Xxxxxxxx",
      "DelayTimeMinutes": 0,
      "Teacher": "Xxxxxxxx Xxxxxxxx",
      "LessonStartTime": "0000-00-00T00:00:00",
      "NumberOfLessons": 0,
      "CreatingTime": "0000-00-00T00:00:00.000",
      "JustificationState": "BeJustified",
      "JustificationStateName": "Igazolandó mulasztás",
      "JustificationType": "UnJustified",
      "JustificationTypeName": "Igazolatlan",
      "SeenByTutelaryUTC": null
    },
    ...
      ],
  "Notes": [
    {
      "NoteId": 0000000,
      "Type": "Elektronikus üzenet",
      "Title": "Xxxxxxxx",
      "Content": "Xxxxxx xxxxxx x xxxxxx xxxxxx xxx xxxxxx.",
      "SeenByTutelaryUTC": null,
      "Teacher": "Xxxxxx Xxxxxx",
      "Date": "0000-00-00T00:00:00",
      "CreatingTime": "0000-00-00T00:00:00.000"
    },
    ...
      ],
  "Lessons": null,
  "Events": null,
  "FormTeacher": {
    "TeacherId": 0000000,
    "Name": "Xxxxxxx Xxxxxxx",
    "Email": null,
    "PhoneNumber": null
  },
  "Tutelaries": [
    {
      "TutelaryId": 0000,
      "Name": "Xxxxxx Xxxxxxx",
      "Email": "",
      "PhoneNumber": "000000000"
    }
  ]
}
```
#### Jegyek:
* Type: lehet "HalfYear": félévi; "MidYear": évközi; "EndYear": év végi
* Form: lehet "Mark": sima jegy; "Text": szöveges értékelés
* Fejlesztőknek figyelem: az `EvaluationId`-t NEM SZABAD egyedi azonosítóként kezelni, az összetartozó (de nyilván különböző) magatartás és szorgalom jegyek ugyanazt az "id"-t kapják. Tehát két jegy néha ugyanazt az id-t kapja. Megoldás lehet az `EvaluationId` végére illeszteni (concatenatelni stringként) a `Jelleg`-nek az `Id`-jét.

## Bejelentett számonkérések lekérdezése
### Elvileg lekéri a tanárok által bejelentett dolgozatokat
* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/BejelentettSzamonkeres -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
 ```

#### A szerver válasza nekem: semmi, mert nálunk egy tanár sem használja nálunk ezt a funkciót
 
## Órarend lekérése
### Lekéri két adott időpont között megtartott (vagy elmaradt) tanórákat

* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)
* fromDate: a vizsgált időintervallum kezdete (ÉÉÉÉ-HH-NN)
* toDate: a vizsgált időintervallum vége (ÉÉÉÉ-HH-NN)

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Lesson?fromDate=2018-09-03&toDate=2018-09-09 -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

#### A szerver válasza:
```json
[
  {
    "LessonId": 0000000,
    "CalendarOraType": "TanitasiOra",
    "Count": 1,
    "Date": "0000-00-00T00:00:00",
    "StartTime": "0000-00-00T00:00:00",
    "EndTime": "0000-00-00T00:00:00",
    "Subject": "XXXXXXXXXXX",
    "SubjectCategory": null,
    "SubjectCategoryName": "XXXXXXXX",
    "ClassRoom": "XXXXX",
    "ClassGroup": "00.x",
    "Teacher": "Xxxxxxx Xxxxx",
    "DeputyTeacher": "",
    "State": "Registered",
    "StateName": "Naplózott tanóra",
    "PresenceType": "Present",
    "PresenceTypeName": "A tanuló részt vett a tanórán",
    "TeacherHomeworkId": null,
    "IsTanuloHaziFeladatEnabled": true,
    "Theme": "Xxxxxxx",
    "Homework": null
  },
  ...
]
```
## Token frissítés
### Lekér egy új Bearer kódot amit majd azonosításra fogunk használni később

```bash
curl --data "refresh_token=XXXXXXXXXXX&grant_type=refresh_token&client_id=919e0c1c-76a2-4646-a2fb-7085bbbf3c56" https://xxxxxxxxxxx.e-kreta.hu/idp/api/v1/Token -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```
* a mobil alkalmazás használja
* refresh_token: A refresh_token amit kaptál amikor beléptél 
* grant_type: refresh_token
* client_id:  ¯\\_(ツ)_/¯

#### A szerver válasza:
```json
{
 "access_token":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
 "refresh_token": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
 ...
 }
 ```

## Tanulói házi feladat lekérése

* Lekéri egy tanuló által felírt házit egy ID alapján
* Az ID-t csak abból az órából lehet lekérni, amiben felírtak házit, tehát végig kell vizsgálni az összes órát például 1 héten, hogy lekérjük az aheti házikat
* Ha tanár töltötte fel a házit, amihez az ID tartozik, akkor a válasz üres

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"  https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/HaziFeladat/TanuloHaziFeladatLista/HAZIFELADATID -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* HAZIFELADATID: egy ID, amit az órarendből kérhetünk le (TeacherHomeworkId)

Válasz:

```json
[
  {
    "Uid": "0000",
    "Id": 0000,
    "TanuloNev": "Xxxxxx Xxxxxx",
    "FeladasDatuma": "2019-00-00T00:00:00.000",
    "FeladatSzovege": "XXXXXXXX",
    "RogzitoId": 000000,
    "TanuloAltalTorolt": false,
    "TanarAltalTorolt": false
  },
  ...
]
```

## Tanulói házi felírása

* Tanuló fel tud írni házi feladatot, amit aztán a többi diák (és gondolom a tanár is) lát
* Lehet, hogy néhány iskolában le van tiltva

```bash
curl -X POST -H "Authorization:Bearer XXXXXXXXXXXXXXXXX" -H "Content-Type:application/json; charset=utf-8" -H "Host:klik00000000.e-kreta.hu" -d '{"OraId":"00000000","OraDate":"0000. 00. 00. 00:00:00","OraType":"TanitasiOra","HataridoUtc":"0000. 00. 00. 22:00:00","FeladatSzovege":"XXXXXXXX"}' "https://klik0000000.e-kreta.hu/mapi/api/v1/HaziFeladat/CreateTanuloHaziFeladat" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

Válasz:
```json
{
  "TanarHaziFeladatId": 00000,
  "HozzaadottTanuloHaziFeladatId": 0000,
  "HozzaadottTanuloHaziBejelentesDatuma": "0000-00-00T00:00:00.00000+00:00"
}
```

* Azt nem értem, hogy miért "TanarHaziFeladatId"-nek hívják, amikor a tanuló teszi fel, de itt vannak ilyen furcsaságok

## Házi törlése

* igazából nem törli a házit a rendszerből, csak átírja a "TanuloAltalTorolt" értékét true-ra, ezt a hivatalos kréta azzal jelöli, hogy áthúzza a szöveget

```bash
curl -X DELETE -H "Authorization:Bearer XXXXXXXXXXXXXXXXX" -H "Content-Type:application/json; charset=utf-8" -H "Host:klik00000000.e-kreta.hu" -d '{"id":"000"}' "https://klik0000000.e-kreta.hu/mapi/api/v1/HaziFeladat/DeleteTanuloHaziFeladat/000" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```# e-Kréta API dokumentáció
Krétás API-oknak nem hivatalos gyűjteménye

Ezeket a lekérdezéseket egy [SSL Capture](https://play.google.com/store/apps/details?id=com.minhui.networkcapture) nevű Androidos alkalmazással szereztem.

A böngészős KRÉTA API dokumentációját [itt találod](https://github.com/Xerren09/eKreta-WebAPI-documentation). ([Xerren09](https://github.com/Xerren09) készítette)

## Nem hivatalos Krétás projectek:
 * [Spongyarend](http://www.sceurpien.com/spongyarend/) Ingyenes órarend generátor alkalmazás a KRÉTA rendszer állományaiból
 * [eFilc](https://github.com/bru02/eFilc) Webes Kréta kliens diákoknak, Toldys extrákkal ([weboldal](https://www.bru02.tk/e-filc/))
 * [KFGnaplo](https://github.com/hexadec/KFGnaplo) Android kliens a Krétához, ami értesítéseket küld az új jegyekről, helyettesítésekről (még csak a Karinthy Frigyes Gimnáziumhoz van)
 * [Szivacs Naplo](https://github.com/boapps/Szivacs-Naplo) Android kliens a Kréta rendszerhez
 * [Kréti](https://github.com/szilardhuber/kreta-bot) Bot, ami Krétás értesítéseket küld egy chatbe
 * [kfgApp](https://github.com/baltitenger/KFGapp) Egy app a Karinthy Gimnáziumhoz, Kréta API-t is használ
 * [CardPass](https://github.com/Xerren09/CardPass-Entry) Beléptetőkártyás rendszer, ami a Kréta rendszert használja, hogy eldöntse késett -e a tanuló
 * [eKreta@thegergo02](https://github.com/thegergo02/eKreta-thegergo02) Cinnamon asztali környezethez egy desklet, megjeleníti a jegyeidet és a Kréta API-t használja
 * [gKréta](https://github.com/thegergo02/gKreta) Az [Electron](https://electronjs.org/) frameworkon alapuló kliens a KRÉTA rendszerhez.
 * [eSzivacs-PC](https://github.com/pepyta/eSzivacs-PC) [Electron](https://electronjs.org/)on alapuló PC-s kliens.
 * [e-kreta-cli](https://github.com/szekelymilan/e-kreta-cli) Egy asztali, konzolos (terminál) kliens a Krétához.
 * [zsírkréta](https://github.com/forcemagic/zsirkreta) Egy Androidos kliens a Krétához
 * [e-Vonalzó](https://github.com/danielszenasi/eVonalzo) Még egy webes Kréta kliens [weboldal](https://evonalzo.netlify.com/sign-in) [graphql api](https://evonalzo.netlify.com/.netlify/functions/kreta)
 * [gokreta](https://github.com/thegergo02/gokreta) Go implementációja a Kréta API-nak.
 * [kosz](https://github.com/Gbr22/kosz) Nem hivatalos Kréta webapp. https://kreta.gbr22.me/
 * [CursedKreta](https://github.com/thegergo02/cursedkreta) Konzolos (terminál) kliens a Krétához, go-ban.
 

## A Kréta API-ja nem követi az HTTP standardot:

FRISSÍTÉS: a krétások e-mailben írtak, hogy nem lenne szabad az ő api kulcsukat használni, mert szerintük, ez "nem tartozik a tisztességes és jogszerű magatartások körébe"

~~Ugyanis az API nem fogadja el ha a header key-t ("apiKey") nagybetűvel küldöd.
Példa okaként [Go](https://golang.org) net/http csomagja minden header key-t nagybetűvel kezd. Persze ez nem akkora nagy probléma mivel van rá workaround, de figyelni kell erre is.~~

~~Viszont vannak olyan nyelvek (pl.: Swift, Dart), amikben nem lehet case-sensitive headert beállítani és így bizonyos lekérdezések (pl.: iskolák listája) nem lehetséges a Kréta API-ból. Erre készült a [kreta-api-mirror](https://github.com/boapps/kreta-api-mirror), amiből egyszerűen le lehet kérni a [raw jsont](https://raw.githubusercontent.com/boapps/kreta-api-mirror/master/school-list.json). Ezt [palmarci](https://github.com/palmarci) [script-je](https://github.com/palmarci/eszivacsupdate) frissíti.~~

[Forrás](https://www.w3.org/Protocols/rfc2616/rfc2616-sec4.html#sec4.2)

## Kötelező header
NE HASZNÁLJÁTOK MÁR!

## Iskolák lekérdezése
#### Az összes iskola ahol be van vezetve az e-Kréta:  
A kréta kérésére eltávolítottam ezt az információt, mert a titkos api kulcsuk benne van.

#### Egy iskola adatainak lekérése ID alapján:
A kréta kérésére eltávolítottam ezt az információt, mert a titkos api kulcsuk benne van.

#### A szerver válasza:  
```json
{
  "InstituteId": 3928,
  "InstituteCode": "appteszt",
  "Name": "PedApp Teszt Intézmény",
  "Url": "https://appteszt.ekreta.hu",
  "City": "Budapest",
  "AdvertisingUrl": "",
  "FeatureToggleSet": {
    "JustificationFeatureEnabled": "false"
  }
}
```

## API linkek lekérdezése
#### Lekéri a KRÉTA API linkjét:
```bash
curl http://kretamobile.blob.core.windows.net/configuration/ConfigurationDescriptor.json
```
* igazából egy mezei böngészőből is [végrehajtható](http://kretamobile.blob.core.windows.net/configuration/ConfigurationDescriptor.json)
* mire jó: ha az API linkje változna, akkor nem kell frissíteni az appot (rájött: [thegergo02](https://github.com/thegergo02))

#### A szerver válasza:  
```json
{
  "GlobalMobileApiUrlDEV": "https://kretaglobalmobileapiuat.ekreta.hu",
  "GlobalMobileApiUrlTEST": "https://kretaglobalmobileapitest.ekreta.hu",
  "GlobalMobileApiUrlUAT": "https://kretaglobalmobileapiuat.ekreta.hu",
  "GlobalMobileApiUrlPROD": "https://kretaglobalmobileapi.ekreta.hu"
}
```

* [Szénási Dániel](https://github.com/danielszenasi) felfedezte, hogy az uat-os api-n [van swagger oldal](https://kretaglobalmobileapiuat.ekreta.hu/swagger/index.html).

## Bejelentkezés
### Lekér egy Bearer kódot amit majd azonosításra fogunk használni később

```bash
curl --data "institute_code=xxxxxxxxxxx&userName=xxxxxxxxxxx&password=xxxxxxxxxxx&grant_type=password&client_id=919e0c1c-76a2-4646-a2fb-7085bbbf3c56" https://xxxxxxxxxxx.e-kreta.hu/idp/api/v1/Token -H "Content-Type: application/x-www-form-urlencoded; charset=utf-8" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```
* a mobil alkalmazás használja
* OAuth 2.0-es "hitelesítési keretrendszer" (felfedezte: [chriskoder](https://github.com/chriskoder))
* csak diák azonosítójával tudtam tesztelgetni, de elvileg a tanárinak is ugyanígy kéne működnie
* institute_code: az intézmény azonosítója
* userName: a felhasználó azonosítója
* password: a felhasználó jelszava
* grant_type: Az OAuth 2 kötelező paramétere, elvileg lehet:
   * Authorization Code
   * Implicit
   * **Password** `grant_type=password`
   * Client Credentials
   * Device Code
   * **Refresh Token** `grant_type=refresh_token`
   Amikor Password-el lekérjük az access_token-t akkor egy refresh_token-t is kapunk, amivel később a jelszó nélkül is frissíthetjük az access_token-ünket.
* client_id:  ¯\\_(ツ)_/¯

#### A szerver válasza:
```json
{
 "access_token":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
 "refresh_token": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
 ...
 }
```

### `access_token` frissítése
```bash
curl --data "institute_code=xxxxxxxxxxx&refresh_token=xxxxxxxxxxx&grant_type=refresh_token&client_id=919e0c1c-76a2-4646-a2fb-7085bbbf3c56" https://xxxxxxxxxxx.e-kreta.hu/idp/api/v1/Token -H "Content-Type: application/x-www-form-urlencoded; charset=utf-8" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```
* ugyanúgy működik, mint a Bearer kód lekérdezése
* itt is kapunk egy refresh_token-t amit újra fel tudunk használni a következő frissítéshez

## E-ügyintézéses üzenetek lekérése

* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)

```bash
curl https://eugyintezes.e-kreta.hu/integration-kretamobile-api/v1/kommunikacio/postaladaelemek/sajat -H "Authorization: Bearer XXXXXXXXXXXXXXX" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* Ha nincs még üzeneted, akkor egy 500-as hibakóddal egy `An error has occured!` üzenetet kapsz, ez itt "normális"

#### A szerver válasza:
```json
[
    {
        "azonosito": 0000,
        "isElolvasva": false,
        "isToroltElem": false,
        "tipus": {
            "azonosito": 1,
            "kod": "BEERKEZETT",
            "rovidNev": "Be\u00e9rkezett \u00fczenet",
            "nev": "Be\u00e9rkezett \u00fczenet",
            "leiras": "Be\u00e9rkezett \u00fczenet"
        },
        "uzenet": {
            "azonosito": 00000,
            "kuldesDatum": "0000-00-00T00:00:00",
            "feladoNev": "XXXXX XXXXXX",
            "feladoTitulus": "tan\u00e1r",
            "szoveg": "xxxxx",
            "targy": "xxxxxxxxx",
            "cimzettLista": [
                {
                    "azonosito": 0000000,
                    "kretaAzonosito": 0000000,
                    "nev": "XXXXXX",
                    "tipus": {
                        "azonosito": 4,
                        "kod": "OSZTALY_TANULO",
                        "rovidNev": "Oszt\u00e1ly - Tanul\u00f3",
                        "nev": "Oszt\u00e1ly - Tanul\u00f3",
                        "leiras": "Oszt\u00e1ly - Tanul\u00f3"
                    }
                }
		...
            ],
            "csatolmanyok": [
                {
                    "azonosito": 000000,
                    "fajlNev": "xxxxxxx.xxx"
                }
		...
	    ]
        }
    },
    ...
]

```
* Az "uzenet" nek a "szoveg"-e maximum 100 karakter hosszú lehet, ha a teljes szöveget akarjuk, akkor egy másik lekérdezés kell:

```bash
curl https://eugyintezes.e-kreta.hu/integration-kretamobile-api/v1/kommunikacio/postaladaelemek/0000 -H "Authorization: Bearer XXXXXXXXXXXXXXX" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* itt a 0000 a legkülső "azonosito"-t jelöli

A szerver válasza:

```json
    {
        "azonosito": 0000,
        "isElolvasva": true,
        "isToroltElem": false,
        "tipus": {
            "azonosito": 1,
            "kod": "BEERKEZETT",
            "rovidNev": "Be\u00e9rkezett \u00fczenet",
            "nev": "Be\u00e9rkezett \u00fczenet",
            "leiras": "Be\u00e9rkezett \u00fczenet"
        },
        "uzenet": {
            "azonosito": 00000,
            "kuldesDatum": "0000-00-00T00:00:00",
            "feladoNev": "XXXXX XXXXXX",
            "feladoTitulus": "tan\u00e1r",
            "szoveg": "xxxxx",
            "targy": "xxxxxxxxx",
            "cimzettLista": [
                {
                    "azonosito": 0000000,
                    "kretaAzonosito": 0000000,
                    "nev": "XXXXXX",
                    "tipus": {
                        "azonosito": 4,
                        "kod": "OSZTALY_TANULO",
                        "rovidNev": "Oszt\u00e1ly - Tanul\u00f3",
                        "nev": "Oszt\u00e1ly - Tanul\u00f3",
                        "leiras": "Oszt\u00e1ly - Tanul\u00f3"
                    }
                }
		...
            ],
            "csatolmanyok": [
                {
                    "azonosito": 000000,
                    "fajlNev": "xxxxxxx.xxx"
                }
		...
	    ]
        }
    }
```

## E-ügyintézéses üzenetek olvasottnak jelölése
* ettől az "isElolvasva" true lesz

```bash
curl https://eugyintezes.e-kreta.hu//integration-kretamobile-api/v1/kommunikacio/uzenetek/olvasott -H "Authorization: Bearer XXXXXXXXXXXXXXX" --data "{"isOlvasott":true,"uzenetAzonositoLista":[0000]}" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

## Bejelentett számonkérések lekérése

```bash
curl https://xxxxxxxxxx.e-kreta.hu/mapi/api/v1/BejelentettSzamonkeres?DatumTol=null&DatumIg=null -H "Authorization: Bearer XXXXXXXXXXXXXXX" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

A szerver válasza:

```json
[
  {
    "Uid": "0000",
    "Id": 0000,
    "Datum": "0000-00-00T00:00:00Z",
    "HetNapja": "Kedd",
    "Oraszam": 0,
    "Tantargy": "kémia",
    "Tanar": "Xxxxxx Xxxxxx",
    "SzamonkeresMegnevezese": "xxxxxxxxx",
    "SzamonkeresModja": "Írásbeli röpdolgozat",
    "BejelentesDatuma": "0000-00-00T00:00:00Z"
  }
  ...
]
```

* Uid: az id csak stringként (pl.: 2 -> "2")
* Hogy miért kell itt (és sehol máshol) a dátumon kívül egy HetNapja is, fogalmam sincs!
* az óraszámm (szerintem) azt mutatja, hogy hányadik órában lesz a dolgozat

## Felhasználó adatainak lekérdezése
### Jegyek, hiányzások, faliújság, szülő és osztályfőnök adatainak lekérdezése

* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Student?fromDate=xx-xx-xx&toDate=xx-xx-xx -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* fromDate: ettől a dátumtól kezdődően mutasson jegyeket, hiányzást és feljegyzést (a "Date" legyen nagyobb vagy egyenlő ennél)
* toDate: eddig a dátumig bezárólag mutasson jegyeket, hiányzást és feljegyzést (a "Date" legyen kissebb vagy egyenlő ennél)

Ha nem szeretnénk dátumhoz kötni, lehet az xx-xx-xx helyére null-t is írni vagy az egészet le lehet hagyni.

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Student?fromDate=null&toDate=null -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```
```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Student -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

#### A szerver válasza:
```json
{
  "StudentId": 0000000,
  "SchoolYearId": 0000000,
  "Name": "Xxxxx Xxxxx",
  "NameOfBirth": "Xxxxx Xxxxx",
  "PlaceOfBirth": "Xxxxx Xxxxx",
  "MothersName": "Xxxxx Xxxxx",
  "AddressDataList": [
    "Xxxxx Xxxxx, Xxxxx Xxxxx xxxx    "
  ],
  "DateOfBirthUtc": "0000-00-00T22:00:00Z",
  "InstituteName": "Xxxxx Xxxxx Xxxxx Xxxxx Xxxxx Xxxxx",
  "InstituteCode": "xxxxxxxxxxx",
  "Evaluations": [
    {
      "EvaluationId": 12345678,
      "Form": "Mark",
      "FormName": "Elégtelen (1) és Jeles (5) között az öt alapértelmezett érték",
      "Type": "MidYear",
      "TypeName": "Évközi jegy/értékelés",
      "Subject": "Xxxxxxxx",
      "SubjectCategory": null,
      "SubjectCategoryName": "Xxxxxxxx",
      "Theme": "xxxxxxx",
      "IsAtlagbaBeleszamit": true,
      "Mode": "Gyakorlati feladat",
      "Weight": "100%",
      "Value": "Jeles(5)",
      "NumberValue": 5,
      "SeenByTutelaryUTC": null,
      "Teacher": "Xxxxxxxx Xxxxxxxx",
      "Date": "2019-06-07T00:00:00",
      "CreatingTime": "2019-06-07T08:00:00.000",
      "Jelleg": {
        "Id": 1,
        "Nev": "Ertekeles",
        "Leiras": "Értékelés"
      },
      "JellegNev": "Ertekeles",
      "ErtekFajta": {
        "Id": 1,
        "Nev": "Osztalyzat",
        "Leiras": "Osztályzat"
      }
    },
    ...
  ],
  "SubjectAverages": [
    {
      "Subject": "Xxxxxxxx",
      "SubjectCategory": null,
      "SubjectCategoryName": "Xxxxxxxx xx Xxxxxxxx",
      "Value": 0.0,
      "ClassValue": 0.00,
      "Difference": -0.00
    },
    ...
  ],
  "Absences": [
    {
      "AbsenceId": 0000000,
      "Type": "Absence",
      "TypeName": "Hiányzás",
      "Mode": "Lesson",
      "ModeName": "Tanórai mulasztás",
      "Subject": "Xxxxxx xxxxx",
      "SubjectCategory": null,
      "SubjectCategoryName": "Xxxxxxxx xx Xxxxxxxx",
      "DelayTimeMinutes": 0,
      "Teacher": "Xxxxxxxx Xxxxxxxx",
      "LessonStartTime": "0000-00-00T00:00:00",
      "NumberOfLessons": 0,
      "CreatingTime": "0000-00-00T00:00:00.000",
      "JustificationState": "BeJustified",
      "JustificationStateName": "Igazolandó mulasztás",
      "JustificationType": "UnJustified",
      "JustificationTypeName": "Igazolatlan",
      "SeenByTutelaryUTC": null
    },
    ...
      ],
  "Notes": [
    {
      "NoteId": 0000000,
      "Type": "Elektronikus üzenet",
      "Title": "Xxxxxxxx",
      "Content": "Xxxxxx xxxxxx x xxxxxx xxxxxx xxx xxxxxx.",
      "SeenByTutelaryUTC": null,
      "Teacher": "Xxxxxx Xxxxxx",
      "Date": "0000-00-00T00:00:00",
      "CreatingTime": "0000-00-00T00:00:00.000"
    },
    ...
      ],
  "Lessons": null,
  "Events": null,
  "FormTeacher": {
    "TeacherId": 0000000,
    "Name": "Xxxxxxx Xxxxxxx",
    "Email": null,
    "PhoneNumber": null
  },
  "Tutelaries": [
    {
      "TutelaryId": 0000,
      "Name": "Xxxxxx Xxxxxxx",
      "Email": "",
      "PhoneNumber": "000000000"
    }
  ]
}
```
#### Jegyek:
* Type: lehet "HalfYear": félévi; "MidYear": évközi; "EndYear": év végi
* Form: lehet "Mark": sima jegy; "Text": szöveges értékelés
* Fejlesztőknek figyelem: az `EvaluationId`-t NEM SZABAD egyedi azonosítóként kezelni, az összetartozó (de nyilván különböző) magatartás és szorgalom jegyek ugyanazt az "id"-t kapják. Tehát két jegy néha ugyanazt az id-t kapja. Megoldás lehet az `EvaluationId` végére illeszteni (concatenatelni stringként) a `Jelleg`-nek az `Id`-jét.

## Bejelentett számonkérések lekérdezése
### Elvileg lekéri a tanárok által bejelentett dolgozatokat
* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/BejelentettSzamonkeres -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
 ```

#### A szerver válasza nekem: semmi, mert nálunk egy tanár sem használja nálunk ezt a funkciót
 
## Órarend lekérése
### Lekéri két adott időpont között megtartott (vagy elmaradt) tanórákat

* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)
* fromDate: a vizsgált időintervallum kezdete (ÉÉÉÉ-HH-NN)
* toDate: a vizsgált időintervallum vége (ÉÉÉÉ-HH-NN)

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Lesson?fromDate=2018-09-03&toDate=2018-09-09 -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

#### A szerver válasza:
```json
[
  {
    "LessonId": 0000000,
    "CalendarOraType": "TanitasiOra",
    "Count": 1,
    "Date": "0000-00-00T00:00:00",
    "StartTime": "0000-00-00T00:00:00",
    "EndTime": "0000-00-00T00:00:00",
    "Subject": "XXXXXXXXXXX",
    "SubjectCategory": null,
    "SubjectCategoryName": "XXXXXXXX",
    "ClassRoom": "XXXXX",
    "ClassGroup": "00.x",
    "Teacher": "Xxxxxxx Xxxxx",
    "DeputyTeacher": "",
    "State": "Registered",
    "StateName": "Naplózott tanóra",
    "PresenceType": "Present",
    "PresenceTypeName": "A tanuló részt vett a tanórán",
    "TeacherHomeworkId": null,
    "IsTanuloHaziFeladatEnabled": true,
    "Theme": "Xxxxxxx",
    "Homework": null
  },
  ...
]
```
## Token frissítés
### Lekér egy új Bearer kódot amit majd azonosításra fogunk használni később

```bash
curl --data "refresh_token=XXXXXXXXXXX&grant_type=refresh_token&client_id=919e0c1c-76a2-4646-a2fb-7085bbbf3c56" https://xxxxxxxxxxx.e-kreta.hu/idp/api/v1/Token -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```
* a mobil alkalmazás használja
* refresh_token: A refresh_token amit kaptál amikor beléptél 
* grant_type: refresh_token
* client_id:  ¯\\_(ツ)_/¯

#### A szerver válasza:
```json
{
 "access_token":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
 "refresh_token": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
 ...
 }
 ```

## Tanulói házi feladat lekérése

* Lekéri egy tanuló által felírt házit egy ID alapján
* Az ID-t csak abból az órából lehet lekérni, amiben felírtak házit, tehát végig kell vizsgálni az összes órát például 1 héten, hogy lekérjük az aheti házikat
* Ha tanár töltötte fel a házit, amihez az ID tartozik, akkor a válasz üres

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"  https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/HaziFeladat/TanuloHaziFeladatLista/HAZIFELADATID -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* HAZIFELADATID: egy ID, amit az órarendből kérhetünk le (TeacherHomeworkId)

Válasz:

```json
[
  {
    "Uid": "0000",
    "Id": 0000,
    "TanuloNev": "Xxxxxx Xxxxxx",
    "FeladasDatuma": "2019-00-00T00:00:00.000",
    "FeladatSzovege": "XXXXXXXX",
    "RogzitoId": 000000,
    "TanuloAltalTorolt": false,
    "TanarAltalTorolt": false
  },
  ...
]
```

## Tanulói házi felírása

* Tanuló fel tud írni házi feladatot, amit aztán a többi diák (és gondolom a tanár is) lát
* Lehet, hogy néhány iskolában le van tiltva

```bash
curl -X POST -H "Authorization:Bearer XXXXXXXXXXXXXXXXX" -H "Content-Type:application/json; charset=utf-8" -H "Host:klik00000000.e-kreta.hu" -d '{"OraId":"00000000","OraDate":"0000. 00. 00. 00:00:00","OraType":"TanitasiOra","HataridoUtc":"0000. 00. 00. 22:00:00","FeladatSzovege":"XXXXXXXX"}' "https://klik0000000.e-kreta.hu/mapi/api/v1/HaziFeladat/CreateTanuloHaziFeladat" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

Válasz:
```json
{
  "TanarHaziFeladatId": 00000,
  "HozzaadottTanuloHaziFeladatId": 0000,
  "HozzaadottTanuloHaziBejelentesDatuma": "0000-00-00T00:00:00.00000+00:00"
}
```

* Azt nem értem, hogy miért "TanarHaziFeladatId"-nek hívják, amikor a tanuló teszi fel, de itt vannak ilyen furcsaságok

## Házi törlése

* igazából nem törli a házit a rendszerből, csak átírja a "TanuloAltalTorolt" értékét true-ra, ezt a hivatalos kréta azzal jelöli, hogy áthúzza a szöveget

```bash
curl -X DELETE -H "Authorization:Bearer XXXXXXXXXXXXXXXXX" -H "Content-Type:application/json; charset=utf-8" -H "Host:klik00000000.e-kreta.hu" -d '{"id":"000"}' "https://klik0000000.e-kreta.hu/mapi/api/v1/HaziFeladat/DeleteTanuloHaziFeladat/000" -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* az url végén a 000 helyére is az id kell

## Tanári házi feladat lekérése

* Lekéri egy tanár által felírt házit egy ID alapján
* Az ID-t csak abból az órából lehet lekérni, amiben felírtak házit, tehát végig kell vizsgálni az összes órát például 1 héten, hogy lekérjük az aheti házikat
* Ha diák töltötte fel, akkor üres

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"  https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/HaziFeladat/TanarHaziFeladat/HAZIFELADATID -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* HAZIFELADATID: egy ID, amit az órarendből kérhetünk le (TeacherHomeworkId)


## TODO:
- faliújság


* az url végén a 000 helyére is az id kell

## Tanári házi feladat lekérése

* Lekéri egy tanár által felírt házit egy ID alapján
* Az ID-t csak abból az órából lehet lekérni, amiben felírtak házit, tehát végig kell vizsgálni az összes órát például 1 héten, hogy lekérjük az aheti házikat
* Ha diák töltötte fel, akkor üres

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"  https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/HaziFeladat/TanarHaziFeladat/HAZIFELADATID -H "User-Agent: "-H "User-Agent: Kreta.Ellenorzo"
```

* HAZIFELADATID: egy ID, amit az órarendből kérhetünk le (TeacherHomeworkId)


## TODO:
- faliújság
