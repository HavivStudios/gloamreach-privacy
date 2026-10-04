# Gloamreach - Privacy policy

**App:** Gloamreach (`com.havivstudios.gloamreach`)
**Publisher:** Adi Haviv
**Contact:** adi.haviv@gmail.com
**Effective:** 2026-10-04
**Version of this document:** 2 (see [Changes](#changes) at the end)

This is the document that gets hosted at the address `storeconfig.json` calls
`privacyPolicyUrl`, and that the game's Settings sheet opens under **Privacy policy**.
It is written to be read, not to be survived. Every claim in it is a claim about code in
this repository, and the tests named at the bottom of each section are what stop it
drifting away from that code.

---

## The short version

Gloamreach is a single-player idle game. It keeps your progress in a file on your own
device. It has no account of its own, asks you for no name, no email and no location,
and runs no analytics of its own. Three Google services can see something, and only
three: **AdMob**, which the game starts at every launch so that a rewarded advert is
ready when you want one; **Play Games**, if you sign in for cloud saves; and **Play
Billing**, if you buy gems. AdMob is the one that runs whether or not you ever watch an
advert, and the section on adverts below says exactly what it receives.

---

## The services this game talks to

Exactly three, and every one of them is configured in
`Assets/Resources/Content/storeconfig.json`. If a service is not in this table, this game
does not talk to it.

| Service | id | Who runs it | What reaches it |
| --- | --- | --- | --- |
| Google Play Games Services | `gpgs` | Google | Only while you are signed in (see *Cloud saves* below): your Play Games gamertag and avatar, your save file while cloud saves are on, and the Play Games SDK's own usage and stability data. The game never asks for your account and never stores it. |
| Google AdMob | `admob` | Google | From every launch (after the consent check, below): your device's IP address, which can estimate a general location; your advertising ID and app set ID; how you interact with the app and its adverts (app launch, taps, video views); and performance data such as launch time. Google uses these for advertising, analytics and fraud prevention. |
| Google Play Billing | `billing` | Google | The product id you are buying. The purchase happens inside Google's own sheet. |

There is no analytics SDK, no crash-reporting SDK, no attribution SDK and no social
SDK. Unity's own analytics, performance reporting and crash reporting are all switched
off in `ProjectSettings/UnityConnectSettings.asset`, and no code in `Assets/Scripts`
references any of them.

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

None of these is uploaded anywhere except through cloud saves, below.

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
- **How to turn it off:** Settings > **Cloud off**. The toggle ships on, so that a
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
- **How to change your mind.** Android's own **Settings > Privacy > Ads** lets you
  delete your advertising ID or opt out of personalisation, and the game honours it.
  Gloamreach does not yet carry an in-game control to reopen the consent form; that is
  listed as remaining hardening in `Docs/store/SETUP.md`.
- **Pre-launch builds show Google's test adverts**, not real ones - `storeconfig.json`
  ships with Google's sample ad unit ids and a test flag, and `StoreConfigTests` fails
  if that stops being true before go-live.

Google's ad-data practices: <https://policies.google.com/technologies/ads>.

---

## Purchases (Google Play Billing)

Gems can be bought. The purchase runs entirely inside Google Play's own sheet: **no
card number, no billing address and no payment detail ever reaches this game or this
developer.** What the game learns is that a product id was bought, so it can credit the
gems. Settings > **Restore purchases** asks Play for what you already own.

---

## No accounts, no profiles

Gloamreach has no login of its own and builds no profile. The game itself does not track
you across apps or websites, does not sell or share data with data brokers, and has no
advertising partners beyond AdMob. What AdMob does with the data it receives, including
personalising adverts where your consent allows it, is described above and governed by
Google's ad-data practices.

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

- Turn **Cloud off** in Settings, or simply never sign in to Play Games - nothing is
  uploaded either way.
- Decline personalised adverts in AdMob's consent form, where it is shown.
- Delete or opt out of your advertising ID in Android's **Settings > Privacy > Ads**.
- **Delete save** in Settings wipes the game on this device and starts it over.
- Uninstall Gloamreach and Android removes every file listed above.
- Delete the cloud slot from your Google account's Play Games settings.

---

## Contact

Questions, or a request about data this app holds: **adi.haviv@gmail.com**. There is no
account to close and no server of ours to delete you from - the two levers that exist
are the ones in the list above, and we will help you find them.

---

## Changes

| Date | Version | What changed |
| --- | --- | --- |
| 2026-08-24 | 1 | First published, alongside the first release build. |
| 2026-10-04 | 2 | Corrected what AdMob receives and when: it starts at every launch to load an advert in advance, not only when you tap one, and it receives an approximate location from your IP address, app interactions and performance data as well as the advertising ID. Added the Play Games gamertag and avatar, and said that Play Games can sign you in automatically. Removed two sentences that said nothing leaves the phone unless you tap an advert, which were not true. |

Any later change is a new row here and a new effective date at the top. A change that
adds a service adds a row to *The services this game talks to* as well - and a table
that disagrees with `storeconfig.json` fails `StoreListingTests`.
