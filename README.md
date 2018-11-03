# e-Kréta API dokumentáció
Krétás API-oknak nem hivatalos gyűjteménye

Ezeket a lekérdezéseket egy [SSL Capture](https://play.google.com/store/apps/details?id=com.minhui.networkcapture) nevű Androidos alkalmazással szereztem

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
curl -H "HOST: xxxxxxxxxxx.e-kreta.hu" --data "institute_code=xxxxxxxxxxx&userName=xxxxxxxxxxx&password=xxxxxxxxxxx&grant_type=password&client_id=919e0c1c-76a2-4646-a2fb-7085bbbf3c56" https://xxxxxxxxxxx.e-kreta.hu/idp/api/v1/Token
```
* a mobil alkalmazás használja
* csak diák azonosítójával tudtam tesztelgetni, de elvileg a tanárinak is ugyanígy kéne működnie
* HOST: az intézményhez tartozó krétás weboldal címe
* institute_code: az intézmény azonosítója
* userName: a felhasználó azonosítója
* password: a felhasználó jelszava
* grant_type:  ¯\\_(ツ)_/¯
* client_id:  ¯\\_(ツ)_/¯

#### A szerver válasza:
```json
{"access_token":"XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"}
```

## Felhasználónév-jelszó páros ellenőrzése

* csak egy gyors ellenőrzés
* a böngészős kréta használja

```bash
curl --data "{
   \"UserName\": \"xxxxxxxxxxx\", \"Password\": \"xxxxxxxxxxx\"
 }" -H "Host: xxxxxxxxxxxxx.e-kreta.hu"
 -e https://xxxxxxxxxxx.e-kreta.hu/Adminisztracio/Login
 https://xxxxxxxxxxx.e-kreta.hu/Adminisztracio/Login/LoginCheck
```
#### A szerver válasza:
```json
{"ErrorCode":"Ok","ErrorMessage":"Sikeres bejelentkezés.","Success":true,"WarningMessage":""}
```

## Felhasználó adatainak lekérdezése
### Jegyek, hiányzások, faliújság, szülő és osztályfőnök adatainak lekérdezése

* a mobil alkalmazás használja
* HOST: az intézményhez tartozó krétás weboldal címe
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)

```bash
curl -H "HOST: xxxxxxxxxxx.e-kreta.hu" -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Student
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
* HOST: az intézményhez tartozó krétás weboldal címe
* kell hozzá a Bearer azonosító (lásd: bejelentkezés)
* fromDate: a vizsgált időintervallum kezdete (ÉÉÉÉ-HH-NN)
* toDate: a vizsgált időintervallum vége (ÉÉÉÉ-HH-NN)

```bash
curl -H "HOST: xxxxxxxxxxx.e-kreta.hu" -H "Authorization: Bearer XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" https://xxxxxxxxxxx.e-kreta.hu/mapi/api/v1/Lesson?fromDate=2018-09-03&toDate=2018-09-09
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
