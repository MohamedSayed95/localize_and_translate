# localize_and_translate

Flutter localization in easy steps

[![Pub](https://img.shields.io/badge/Get%20library-pub-blue)](https://pub.dev/packages/localize_and_translate)
[![Example](https://img.shields.io/badge/Example-Ex-success)](https://pub.dev/packages/localize_and_translate#-example-tab-)


## Screenshots
<img src="https://github.com/msayed-net/localize_and_translate/blob/master/screenshot1.png?raw=true" alt="screenshot" width="200"/><span>  </span><img src="https://github.com/msayed-net/localize_and_translate/blob/master/screenshot2.png?raw=true" alt="screenshot" width="200"/>


## Tutorial
### Video
* Arabic : [https://www.youtube.com/watch?v=nfDYussovfQ](https://www.youtube.com/watch?feature=player_embedded&v=nfDYussovfQ)
* English : soon..


### Methods
| Method        | Job           |
| ------------- |:-------------:|
| `init()` |initialize things, before runApp() |
| `translate('word')` |word translation |
| `translate('word',{"key":"value"})` |word translation with replacement arguments|
| `setNewLanguage(context,newLanguage:'en',restart: true, remember: true,)` |change language |
| `isDirectionRTL()` |is Direction RTL check |
| `currentLanguage` |Active language code |
| `locale` |Active Locale |
| `locals()` |Locales list |
| `delegates` |Localization Delegates |

### Installation
* add `.json` translation files as assets
- For example : `'assets/langs/ar.json'` | `'assets/langs/en.json'`
- structure should look like
``` json
{
    "appTitle": "تطبيق تجريبى", 
    "buttonTitle": "English", 
    "googleTest": "تجربة ترجمة جوجل", 
    "textArea": "هذا مجرد نموذج للتأكد من اداء الأداة"
}
```
- define them as assets in pubspec.yaml
``` yaml
flutter:
  assets:
    - assets/langs/en.json
    - assets/langs/ar.json
```

### Migrate from 2.x.x to 3.x.x
- Replace
```dart
LIST_OF_LANGS = ['ar', 'en'];
LANGS_DIR = 'assets/langs/';
await translator.init();
```
- With
```dart
await translator.init(
    localeDefault: LocalizationDefaultType.device,
    languagesList: <String>['ar', 'en'],
    assetsDirectory: 'assets/langs/',
    apiKeyGoogle: '<Key>', // NOT YET TESTED
);
```

### Initialization
- Add imports to main.dart
- Make `main()` `async` and do the following
- Ensure flutter activated `WidgetsFlutterBinding.ensureInitialized()` 
- Initialize `await translator.init();` with neccassry parameters
- Inside `runApp()` wrap entry class with `LocalizedApp()`
- Note : make sure you define it's child into different place "NOT INSIDE"

``` dart
import 'package:flutter/material.dart';
import 'package:localize_and_translate/localize_and_translate.dart';

main() async {
  // if your flutter > 1.7.8 :  ensure flutter activated
  WidgetsFlutterBinding.ensureInitialized();

  await translator.init(
    localeDefault: LocalizationDefaultType.device,
    languagesList: <String>['ar', 'en'],
    assetsDirectory: 'assets/langs/',
    apiKeyGoogle: '<Key>', // NOT YET TESTED
  );

  runApp(
    LocalizedApp(
      child: MyApp(),
    ),
  );
}
```

- `LocalizedApp()` child example -> `MaterialApp()`

``` dart
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Home(),
      localizationsDelegates: translator.delegates, // Android + iOS Delegates
      locale: translator.locale, // Active locale
      supportedLocales: translator.locals(), // Locals list
    );
  }
}
```

### Usage

* use `translate("appTitle")` 
* use `googleTranslate("test", from: 'en', to: 'ar')` 
* use `setNewLanguage(context, newLanguage: 'ar', remember: true, restart: true);`

``` dart
class Home extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      drawer: Drawer(),
      appBar: AppBar(
        title: Text(translator.translate('appTitle')),
        // centerTitle: true,
      ),
      body: Container(
        width: double.infinity,
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          children: <Widget>[
            SizedBox(height: 50),
            Text(
              translator.translate('textArea'),
              textAlign: TextAlign.center,
              style: TextStyle(fontSize: 35),
            ),
            OutlineButton(
              onPressed: () {
                translator.setNewLanguage(
                  context,
                  newLanguage: translator.currentLanguage == 'ar' ? 'en' : 'ar',
                  remember: true,
                  restart: true,
                );
              },
              child: Text(translator.translate('buttonTitle')),
            ),
            OutlineButton(
              onPressed: () async {
                print(await translator.translateWithGoogle(
                  key: 'رجل',
                  from: 'ar',
                ));
              },
              child: Text(translator.translate('googleTest')),
            ),
          ],
        ),
      ),
    );
  }
}

```

### :heart: this
[![Fork](https://img.shields.io/github/forks/msayed-net/localize_and_translate?style=social)](https://github.com/msayed-net/localize_and_translate/fork)
[![Star](https://img.shields.io/github/stars/msayed-net/localize_and_translate?style=social)](https://github.com/msayed-net/localize_and_translate/stargazers)
[![Watch](https://img.shields.io/github/watchers/msayed-net/localize_and_translate?style=social)](https://github.com/msayed-net/localize_and_translate/) 


## Project Created & Maintained By
### [![Mohamed Sayed](./logo.png)](https://msayed.net)
Software Engineer | In :heart:  with Flutter


## Note : All Contibutions Are Welcomed
### Contributions
* `translate()` accept keys : by [mo-ah-dawood](https://github.com/mo-ah-dawood)
* Change template from plugin to package : by [mo-ah-dawood](https://github.com/mo-ah-dawood)
