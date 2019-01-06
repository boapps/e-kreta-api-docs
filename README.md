# e-Kréta API dokumentáció
Krétás API-oknak nem hivatalos gyűjteménye

Ezeket a lekérdezéseket egy [SSL Capture](https://play.google.com/store/apps/details?id=com.minhui.networkcapture) nevű Androidos alkalmazással szereztem.

A böngészős KRÉTA API dokumentációját [itt találod](https://github.com/Xerren09/eKreta-WebAPI-documentation). ([Xerren09](https://github.com/Xerren09) készítette)

## Nem hivatalos Krétás projectek:
 * [Spongyarend](http://spongyarend.com/) Ingyenes órarend generátor alkalmazás a KRÉTA rendszer állományaiból
 * [eFilc](https://github.com/bru02/eFilc) Webes Kréta kliens diákoknak, Toldys extrákkal
 * [KFGnaplo](https://github.com/hexadec/KFGnaplo) Android kliens a Krétához, ami értesítéseket küld az új jegyekről, helyettesítésekről (még csak a Karinthy Frigyes Gimnáziumhoz van)
 * [eSzivacs 2](https://github.com/boapps/e-Szivacs-2) Android kliens a Kréta rendszerhez
 * [Kréti](https://github.com/szilardhuber/kreta-bot) Bot, ami Krétás értesítéseket küld egy chatbe
 * [kfgApp](https://github.com/baltitenger/KFGapp) Egy app a Karinthy Gimnáziumhoz, Kréta API-t is használ
 * [CardPass](https://github.com/Xerren09/CardPass-Entry) Beléptetőkártyás rendszer, ami a Kréta rendszert használja, hogy eldöntse késett -e a tanuló
 * [eKreta@thegergo02](https://github.com/thegergo02/eKreta-thegergo02) Cinnamon asztali környezethez egy desklet, megjeleníti a jegyeidet és a Kréta API-t használja

## Iskolák lekérdezése
#### Az összes iskola ahol be van vezetve az e-Kréta:  
```bash
curl -H "apiKey: 7856d350-1fda-45f5-822d-e1a2f3f1acf0"  https://kretaglobalmobileapi.ekreta.hu/api/v1/Institute
```
* apiKey: kötelező bizonyos lekérdezésekhez, mindenkinek ugyanaz:
    * 7856d350-1fda-45f5-822d-e1a2f3f1acf0
* https://kretaglobalmobileapi.ekreta.hu/api/v1/Institute: ez az URL ahova a lekérdezést küldjük (böngészőből nem működik)

#### A szerver válasza:  
```json
[
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
  },
  ...
]
```

## Ismeretlen lekérdezés
#### Nem tudom mit csinál:
```bash
curl http://kretamobile.blob.core.windows.net/configuration/ConfigurationDescriptor.json
```
* igazából egy mezei böngészőből is [végrehajtható](http://kretamobile.blob.core.windows.net/configuration/ConfigurationDescriptor.json)

#### A szerver válasza:  
```json
{
  "GlobalMobileApiUrlDEV": "https://kretaglobalmobileapiuat.ekreta.hu",
  "GlobalMobileApiUrlTEST": "https://kretaglobalmobileapitest.ekreta.hu",
  "GlobalMobileApiUrlUAT": "https://kretaglobalmobileapiuat.ekreta.hu",
  "GlobalMobileApiUrlPROD": "https://kretaglobalmobileapi.ekreta.hu"
}
```

## Bejelentkezés
### Lekér egy Bearer kódot amit majd azonosításra fogunk használni később

```bash
curl --data "institute_code=xxxxxxxxxxx&userName=xxxxxxxxxxx&password=xxxxxxxxxxx&grant_type=password&client_id=919e0c1c-76a2-4646-a2fb-7085bbbf3c56" https://xxxxxxxxxxx.e-kreta.hu/idp/api/v1/Token
```
* a mobil alkalmazás használja
* csak diák azonosítójával tudtam tesztelgetni, de elvileg a tanárinak is ugyanígy kéne működnie
* institute_code: az intézmény azonosítója
* userName: a felhasználó azonosítója
* password: a felhasználó jelszava
* grant_type:  ¯\\_(ツ)_/¯
* client_id:  ¯\\_(ツ)_/¯

#### A szerver válasza:
```json
{
 "access_token":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
 "refresh_token": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
 ...
 }
```

## Felhasználó adatainak lekérdezése
### Jegyek, hiányzások, faliújság, szülő és osztályfőnök adatainak lekérdezése

* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Student
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
      "EvaluationId": 00000000,
      "Form": "Mark",
      "FormName": "Elégtelen (1) és Jeles (5) között az öt alapértelmezett érték",
      "Type": "MidYear",
      "TypeName": "Évközi értékelés",
      "Subject": "Xxxxxxxx",
      "SubjectCategory": null,
      "SubjectCategoryName": "Xxxxxxxx",
      "Theme": "Xxxxxxxx",
      "Mode": "Írásbeli röpdolgozat",
      "Weight": "100%",
      "Value": "Jeles(5)",
      "NumberValue": 5,
      "SeenByTutelaryUTC": null,
      "Teacher": "Xxxxxxxx Xxxxxxxx",
      "Date": "0000-00-00T00:00:00",
      "CreatingTime": "0000-00-00T00:00:00.000"
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

## Órarend lekérése
### Lekéri két adott időpont között megtartott (vagy elmaradt) tanórákat

* a mobil alkalmazás használja
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)
* fromDate: a vizsgált időintervallum kezdete (ÉÉÉÉ-HH-NN)
* toDate: a vizsgált időintervallum vége (ÉÉÉÉ-HH-NN)

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Lesson?fromDate=2018-09-03&toDate=2018-09-09
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
curl --data "refresh_token=XXXXXXXXXXX&grant_type=refresh_token&client_id=919e0c1c-76a2-4646-a2fb-7085bbbf3c56" https://xxxxxxxxxxx.e-kreta.hu/idp/api/v1/Token
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
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"  https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/HaziFeladat/TanuloHaziFeladatLista/HAZIFELADATID
```

* HAZIFELADATID: egy ID, amit az órarendből kérhetünk le (TeacherHomeworkId)


## Tanári házi feladat lekérése

* Lekéri egy tanár által felírt házit egy ID alapján
* Az ID-t csak abból az órából lehet lekérni, amiben felírtak házit, tehát végig kell vizsgálni az összes órát például 1 héten, hogy lekérjük az aheti házikat
* Ha diák töltötte fel, akkor üres

```bash
curl -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"  https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/HaziFeladat/TanarHaziFeladat/HAZIFELADATID
```

* HAZIFELADATID: egy ID, amit az órarendből kérhetünk le (TeacherHomeworkId)


## TODO:
- faliújság
