# Gloamreach - Privacy policy

**App:** Gloamreach (`com.havivstudios.gloamreach`)
**Publisher:** Adi Haviv
**Contact:** adi.haviv@gmail.com
**Effective:** 2026-08-24
**Version of this document:** 1 (see [Changes](#changes) at the end)

This is the document that gets hosted at the address `storeconfig.json` calls
`privacyPolicyUrl`, and that the game's Settings sheet opens under **Privacy policy**.
It is written to be read, not to be survived. Every claim in it is a claim about code in
this repository, and the tests named at the bottom of each section are what stop it
drifting away from that code.

---

## The short version

Gloamreach is a single-player idle game. It keeps your progress in a file on your own
device. It has no account of its own, asks for no name, no email and no location, and
runs no analytics. Three Google services can see something, and only three: **Play
Games** if you use cloud saves, **AdMob** when you choose to watch an advert for a
reward, and **Play Billing** if you buy gems. Turn cloud saves off and never tap an
advert, and nothing about you leaves the phone.

---

## The services this game talks to

Exactly three, and every one of them is configured in
`Assets/Resources/Content/storeconfig.json`. If a service is not in this table, this game
does not talk to it.

| Service | id | Who runs it | What reaches it |
| --- | --- | --- | --- |
| Google Play Games Services | `gpgs` | Google | Your save file, only while cloud saves are on and you are signed in. Google also sees the Play Games account you signed in with - the game never asks for it and never stores it. |
| Google AdMob | `admob` | Google | An ad request when you tap a rewarded advert, carrying your device's advertising ID and whatever the consent form you answered permits. |
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

- **Nothing is uploaded until you sign in to Play Games.** The sign-in prompt is
  Google's, it is the consent step, and declining it means no save ever leaves the
  device. The game carries on perfectly well offline.
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

- **Consent is asked first.** Before the first ad request the game shows Google's UMP
  consent form where the law requires one (`AdMobAdapter.EnsureConsentAndSdk`), and no
  ad is requested until consent has been resolved. If you refuse, the game does not
  request ads.
- **The advertising ID.** A rewarded advert carries your device's Google advertising
  ID, which is what the `com.google.android.gms.permission.AD_ID` permission in the
  installed app is for. It is Google's identifier, resettable and deletable by you.
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

## No accounts, no profiles, no tracking

Gloamreach has no login of its own and builds no profile. It does not track you across
apps or websites, does not sell or share data with data brokers, and has no advertising
partners beyond AdMob as described above.

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
- Never tap a rewarded advert - no ad request is ever made.
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

Any later change is a new row here and a new effective date at the top. A change that
adds a service adds a row to *The services this game talks to* as well - and a table
that disagrees with `storeconfig.json` fails `StoreListingTests`.
