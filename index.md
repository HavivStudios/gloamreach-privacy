# Gloamreach - Privacy policy

**App:** Gloamreach (`com.havivstudios.gloamreach`)
**Publisher:** Adi Haviv
**Contact:** havivgamestudios@gmail.com
**Effective:** 2026-11-06 (version 5 adds the iPhone version; the Android rows are unchanged since 2026-10-13)
**Version of this document:** 5 (see [Changes](#changes) at the end)

This is the document that gets hosted at the address `storeconfig.json` calls
`privacyPolicyUrl`, and that the game's Settings sheet opens under **Privacy policy**.
It is written to be read, not to be survived. Every claim in it is a claim about code in
this repository, and the tests named at the bottom of each section are what stop it
drifting away from that code.

---

## The short version

Gloamreach is a single-player idle game. It keeps your progress in a file on your own
device. It has no account of its own and asks you for no name, no email and no location.
Four Google services can see something, and only four: **AdMob**, which the game starts
at every launch so that a rewarded advert is ready when you want one; **Firebase**, which
receives gameplay stats and crash reports so the game can be balanced and fixed - only
once the consent check allows it, never while the **Share gameplay stats** switch in Settings is off,
and never for adverts; **Play Games**, if you sign in for cloud saves; and **Play Billing**,
if you buy gems. AdMob is the one that runs whether or not you ever watch an advert, and
the section on adverts below says exactly what it receives.

---

## The services this game talks to

Exactly five, and every one of them is configured in
`Assets/Resources/Content/storeconfig.json`. If a service is not in this table, this game
does not talk to it.

| Service | id | Who runs it | What reaches it |
| --- | --- | --- | --- |
| Google Play Games Services | `gpgs` | Google | Only while you are signed in (see *Cloud saves* below): your Play Games gamertag and avatar, your save file while cloud saves are on, and the Play Games SDK's own usage and stability data. The game never asks for your account and never stores it. |
| Google AdMob | `admob` | Google | From every launch (after the consent check, below): your device's IP address, which can estimate a general location; your advertising ID and app set ID; how you interact with the app and its adverts (app launch, taps, video views); and performance data such as launch time. Google uses these for advertising, analytics and fraud prevention. |
| Google Play Billing | `billing` | Google | The product id you are buying. The purchase happens inside Google's own sheet. |
| Google Firebase (Analytics and Crashlytics) | `firebase` | Google | Only after the consent check has settled and allows it, and never while the **Share gameplay stats** switch in Settings is off (see *Gameplay stats and crash reports* below): the game's own gameplay events with the play time and date they happened at; random installation ids Firebase creates (no advertising ID - its collection is switched off in the build); the device model, Android version and language; a coarse location (country or region) that Google derives from a masked IP address; the purchases Play reports to it; and, when the game crashes, the stack trace, the device state at the time and the last 20 gameplay events before it. |
| Apple (Game Center, iCloud and the App Store) - the iPhone version only | `ios` | Apple | On iPhone, in place of the three Google Play services above: your Game Center player id while you are signed in to Game Center; your save file, kept in your own private iCloud database while cloud saves are on (you can delete it from Settings); and the product id you are buying, inside Apple's own purchase sheet. The iPhone version uses the same AdMob and Firebase rows above, under their own iPhone app ids. |

Firebase Analytics and Firebase Crashlytics are the only analytics and crash-reporting
SDKs in the game, and no other Firebase product is in the build. There is no attribution
SDK and no social SDK. Unity's own analytics, performance reporting and crash reporting
are all switched off in `ProjectSettings/UnityConnectSettings.asset`, and no code in
`Assets/Scripts` references any of them.

*Checked by:* `StoreListingTests.ThePrivacyPolicyNamesEveryConfiguredServiceAndNothingElse`.

---

## What is stored on your device

The whole game is four files in the app's own private folder
(`Android/data/com.havivstudios.gloamreach/files`, which is
`Application.persistentDataPath`). No other app can read them, and Android deletes all
of them when Gloamreach is uninstalled.

| File | What it is |
| --- | --- |
| `save.json` | Your game: hero, gold, gems, upgrades, professions, zones, settings. |
| `save.json.bak` | The previous `save.json`, kept by every write so one bad write cannot cost you the game. |
| `save.corrupt-<stamp>.json` | A save that could not be read, set aside instead of thrown away. Three are kept; the oldest goes. |
| `crash.log` | Unhandled errors, capped in size, kept so a crash can be explained. It holds an error message and a stack trace from this app's own code - no save data and nothing about you. |

None of these is uploaded anywhere except through cloud saves, below. Your answer to
**Share gameplay stats** is kept in `save.json`, so it survives a restart.

Firebase keeps its own small files in the same private folder: the random installation
ids it created, and any gameplay stats or crash report waiting for a connection. They go
when the app is uninstalled, like everything else there.

The game also stores nothing outside that folder. It reads no contacts, no photos, no
files of yours, no calendar, no microphone and no camera, and it never asks for a
location.

---

## Cloud saves (Google Play Games)

Cloud saves put a copy of `save.json` into your own Google Play Games *saved games*
slot, named `gloamreach-progress`, so a new phone can pick your game up where the old one
left it.

- **Nothing is uploaded unless you are signed in to Play Games.** Sign-in is Google's,
  not the game's: if you already have a Play Games profile that allows automatic sign-in,
  Google signs you in when the game starts and shows a banner saying so; otherwise the
  game only asks when you choose to sign in from Settings. Not signed in means no save
  ever leaves the device, and the game carries on perfectly well offline.
- **What is uploaded** is the save file and nothing more - the same progress you can
  see on the screen. No advertising ID, no device identifier of ours, no message.
- **How to turn it off:** switch off **Cloud sync** in Settings. The switch ships on, so that a
  player who never opens Settings is not the one who loses a game to a lost phone, but
  it does nothing at all until you have signed in.
- **How to delete what is there:** the slot belongs to your Google account, and Google's
  Play Games settings let you delete saved-game data for an app.

Google's handling of that data is Google's: <https://policies.google.com/privacy>.

---

## Adverts (AdMob)

There is one kind of advert in Gloamreach: a **rewarded** advert you choose to watch in
exchange for something in the game. No banners, no interstitials, nothing that appears
without you tapping for it.

- **When AdMob runs.** At every launch the game asks AdMob for one rewarded advert in
  advance, so that an offer is ready the moment you reach it rather than after a wait.
  That means the AdMob SDK starts, and sends the data listed in the table above, even if
  you never watch an advert. Nothing is shown until you tap for it.
- **Consent is asked first.** Before AdMob starts, the game runs Google's UMP consent
  check and shows its consent form where the law requires one
  (`AdMobAdapter.EnsureConsentAndSdk`). Your answer decides whether adverts may be
  personalised. If the check reports that ads may not be requested at all, AdMob is not
  started.
- **The advertising ID.** AdMob reads your device's Google advertising ID, which is what
  the `com.google.android.gms.permission.AD_ID` permission in the installed app is for.
  It is Google's identifier, resettable and deletable by you.
- **How to change your mind.** Where the law gives you a standing right to change your
  consent answer (the EEA, the UK and Switzerland, as Google's consent SDK reports it),
  the game's Settings sheet shows **Ad privacy choices**, which reopens the same consent
  form. Everywhere, Android's own **Settings > Privacy > Ads** lets you delete your
  advertising ID or opt out of personalisation, and the game honours it.
- **Pre-launch builds show Google's test adverts**, not real ones - `storeconfig.json`
  ships with Google's sample ad unit ids and a test flag, and `StoreConfigTests` fails
  if that stops being true before go-live.

Google's ad-data practices: <https://policies.google.com/technologies/ads>.

---

## Gameplay stats and crash reports (Firebase)

From version 1.0.4 the game sends its own gameplay stats to Google's Firebase Analytics,
and crash reports to Firebase Crashlytics. They exist so that one developer can see where
players get stuck, where they stop playing and what crashes - and then fix the game. They
are not used for adverts.

- **What a stat is.** A short named event from the game itself: the tutorial's steps, a
  zone reached, a boss fought, a hero or profession level, an upgrade bought, an advert
  watched, the game opened or put away. Each carries the play time and the date it
  happened at and, where it has one, the thing it is about (a zone's id, an upgrade's id)
  and a number (a level, an amount). The full list is in the game's source,
  `Docs/balance/EVENTS.md`. Nothing you typed, no save file, no name.
- **Who it is tied to.** A random id Firebase creates when the game is installed. It is
  not your name, not your Google account and not your advertising ID, and the game never
  joins it with anything else. Uninstalling the game throws it away; a reinstall gets a
  new one.
- **Nothing before consent.** The build tells Firebase, before any of the game's code
  runs, to store nothing. The game changes that only once Google's consent check (the
  same one AdMob uses) has settled: where consent is required (the EEA, the UK and
  Switzerland) stats are kept only if you agreed to the storing of information on your
  device; elsewhere they are kept unless you turn them off. Ad storage, ad user data and
  ad personalisation are never granted to Firebase, and its collection of the advertising
  ID is switched off. Crash reports follow the same answer as the stats.
- **How to turn it off.** Switch off **Share gameplay stats** in Settings - its knob
  moves to the off end of the track. From that moment no gameplay stats and no crash reports are sent, and the
  answer is kept until you change it.
- **How long it is kept.** Google keeps the event data for the Analytics property's
  retention period (Google's default, two months) and Crashlytics keeps crash reports for
  90 days. Once a day Firebase copies the stats into the developer's own Google BigQuery
  project, where each day's copy is deleted after 60 days; from there they are copied to
  the developer's own computer to build balancing reports, keyed only by the random id
  above.
- **Deleting it.** Because the id is random and the game never shows it to you, there is
  no way for us to find "your" stats by your name or email. Switching off **Share gameplay stats**
  stops new ones; what was already sent expires as described above.

Google's handling of Firebase data: <https://firebase.google.com/support/privacy>.

---

## Purchases (Google Play Billing)

Gems can be bought. The purchase runs entirely inside Google Play's own sheet: **no
card number, no billing address and no payment detail ever reaches this game or this
developer.** What the game learns is that a product id was bought, so it can credit the
gems. Settings > **Restore purchases** asks Play for what you already own. Firebase
Analytics, where it is allowed to run (above), also records that a purchase happened - the
product and its price.

---

## On iPhone (Game Center, iCloud, the App Store and tracking)

The iPhone version is the same game with Apple's services in place of Google Play's. AdMob and Firebase
work as described above, under the iPhone app's own ids, with the differences listed here.

- **Game Center and iCloud saves.** Cloud saves on iPhone use your Game Center sign-in and your own
  **private iCloud database**. If you are already signed in to Game Center, the game picks that up without
  asking; otherwise Apple's own Game Center sheet appears only when you choose **Sync now** or turn on
  **Cloud sync** in Settings. What is stored is the save file, a few numbers about it (hero level, zone, rebirth count, play time
  and when it was saved, so the game can tell which copy is newer) and your Game Center player id, so it
  knows whose save it is. It sits in your own iCloud account, not on a server of ours, and we cannot read
  it. The player id goes nowhere else. Not signed in means no save leaves the device.
- **Deleting the iCloud copy.** Settings > **Delete iCloud save** deletes it and turns **Cloud sync** off
  on that device. Removing Gloamreach's data in iOS Settings > [your name] > iCloud deletes it too.
- **Purchases (the App Store).** Gems are bought in Apple's own purchase sheet: no card number or payment
  detail reaches this game or this developer, only that a product id was bought. Settings > **Restore
  purchases** asks the App Store again and re-delivers a purchase whose gems were not credited.
- **Tracking (App Tracking Transparency).** On first launch the consent form comes first, where the law
  requires one. Only if you did not refuse consent there does iOS ask whether Gloamreach may track you.
  Allowing it lets AdMob read your device's advertising identifier (IDFA) for personalised adverts.
  Refusing means AdMob gets no IDFA and adverts are not personalised. Nothing in the game depends on your
  answer, and you can change it at any time in iOS Settings > Privacy & Security > Tracking.
- **Firebase on iPhone** never collects the IDFA, whatever you answer: its collection is switched off in
  the build, as on Android. **Share gameplay stats** in Settings works the same way.
- **Your choices on iPhone** are the list below, with Delete iCloud save, the Tracking setting and
  uninstalling in place of the Android-only lines. Uninstalling removes every file on the device, and
  leaves the iCloud copy until you delete it.

Apple's handling of Game Center, iCloud and App Store data is Apple's:
<https://www.apple.com/legal/privacy/>.

---

## No accounts, no profiles

Gloamreach has no login of its own and builds no profile of you. Its gameplay stats are
tied to a random id, never to a name, an account or your advertising ID, and are used
only to balance and fix the game. The game itself does not track you across apps or
websites, does not sell or share data with data brokers, and has no advertising partners
beyond AdMob; Firebase is not used for adverts. What AdMob does with the data it
receives, including personalising adverts where your consent allows it, is described
above and governed by Google's ad-data practices.

---

## Children

Gloamreach is a dark-fantasy role-playing game. **It is not directed at children under
13**, is not enrolled in Google Play's Designed for Families programme, and its consent
request is not tagged for under-age users
(`AdMobAdapter.BuildConsentRequest` sets `TagForUnderAgeOfConsent` to false). The
content rating this app declares and the questionnaire answers behind it are in
`Docs/store/DATA-SAFETY.md`.

---

## Your choices, in one list

- Switch off **Cloud sync** in Settings, or simply never sign in to Play Games - nothing is
  uploaded either way.
- Switch off **Share gameplay stats** in Settings - no gameplay stats and no crash reports are
  sent from then on. Where the consent form is shown, declining the storing of
  information on your device does the same.
- Decline personalised adverts in AdMob's consent form, where it is shown - and change
  that answer later from Settings > **Ad privacy choices** where it is offered.
- Delete or opt out of your advertising ID in Android's **Settings > Privacy > Ads**.
- **Delete save** in Settings wipes the game on this device and starts it over.
- Uninstall Gloamreach and Android removes every file listed above.
- Delete the cloud slot from your Google account's Play Games settings.

---

## Contact

Questions, or a request about data this app holds: **havivgamestudios@gmail.com**. There is no
account to close and no server of ours to delete you from - the levers that exist are the
ones in the list above, and we will help you find them.

---

## Changes

| Date | Version | What changed |
| --- | --- | --- |
| 2026-08-24 | 1 | First published, alongside the first release build. |
| 2026-10-04 | 2 | Corrected what AdMob receives and when: it starts at every launch to load an advert in advance, not only when you tap one, and it receives an approximate location from your IP address, app interactions and performance data as well as the advertising ID. Added the Play Games gamertag and avatar, and said that Play Games can sign you in automatically. Removed two sentences that said nothing leaves the phone unless you tap an advert, which were not true. Added the in-game **Ad privacy choices** control. Contact address changed to the studio's. |
| 2026-10-09 | 3 | Added Firebase (Analytics and Crashlytics), which version 1.0.4 of the game uses for gameplay stats and crash reports: what it receives, that nothing is kept before the consent check allows it, the new **Sharing stats** switch in Settings, how long the data is kept, and that it is never used for adverts. Removed "runs no analytics of its own", which stopped being true with 1.0.4. Versions 1.0.3 and earlier send nothing to Firebase. |
| 2026-10-13 | 4 | Version 1.1.0 names the two Settings switches by what they are - **Share gameplay stats** (1.0.4 called it **Sharing stats on/off**) and **Cloud sync** (was **Cloud on/off**) - and shows on or off by the switch itself. Nothing about what is collected or sent changed. |
| 2026-11-05 | 5 | Added *On iPhone*: Game Center sign-in and saves in your private iCloud database, **Delete iCloud save**, App Store purchases and **Restore purchases**, Apple's tracking prompt and what refusing it means. It takes effect with the first iPhone release; nothing about the Android version changed. |

Any later change is a new row here and a new effective date at the top. A change that
adds a service adds a row to *The services this game talks to* as well - and a table
that disagrees with `storeconfig.json` fails `StoreListingTests`.
