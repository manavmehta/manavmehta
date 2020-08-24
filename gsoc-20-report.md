# Google Summer of Code 2020 @ Zulip - Project Report

I've never been a blog person but as my GSoC concludes, it's the best time to do a write-up about how I started, what I experienced, and what I loved about and gained from the program.

[![project chat](https://img.shields.io/badge/zulip-join_chat-brightgreen.svg)](https://chat.zulip.org)

<img src="https://i.imgur.com/BRMm7yn.png"/>


## About the project
* [Proposal Link](https://docs.google.com/document/d/11sXx6OWPosq0IAuREJpw_RTNSvCzNfuFF2KldbvnBh0/edit?usp=sharing)
* [Contributions: Pull Requests Link](https://github.com/zulip/zulip-desktop/pulls/manavmehta)

I would like to express my gratitude towards my mentors [Abhigyan Khaund](https://www.github.com/abhigyank) and [Akash Nimare](https://www.github.com/akashnimare) for being extremely guiding, friendly and resourceful and helping me throughout my GSoC period even before it officially began.

Zulip as an organization is extremely fun, connected and more like a well-knit-family. Contributors work dedicatedly as they are maintainers and the work should be of good quality as **Friends Don't Let Friends Ship Inaccessible Code**.

My project was primarily oriented towars making Zulip's desktop client a better overall in terms of UX and features.
I worked with my mentors, who were readily available throughout my GSoC period, to work on the high priority issues as Zulip is an end-user centric product and we want our users to have the best experience.
___

## Highlights of the work

### Merged Pull Requests
#### [#D902 - Add feature to set application language](https://github.com/zulip/zulip-desktop/pull/902)
The application used to fetch system locale and use it as the application language. A feature request came demanding an option to change the application language without changing system locale. A dropdown was implemented which would allow user to select their preferred language without changing system locale.

#### [#D924 - Handle Reset options gracefully (minor)(UX enhancement)](https://github.com/zulip/zulip-desktop/pull/924)
There were two different reset options - `Factory Reset` and `Reset App Data` - which had two different paths and caused ambiguity for the end-user. The latter one was removed and the former was refined to completely reset the app as if it were just downloaded and installed on the system.

#### [#D929 - Generate User-Agent upon starting up the app, avoiding disk storage](https://github.com/zulip/zulip-desktop/pull/929)
User Agent was repetedly reported to be incorrectly cached as the application used to store it on disk and update it only when it's not present thus, caching the old value and sending the server *wrong* version. In addition to it, in [Anders Kaseorg's](https://www.github.com/andersk) words, storing on disk wouldn't be a good option as *The disk is much less efficient than memory—up to hundreds of thousands of times slower*, *Repeatedly writing to a solid-state disk wears it out slowly over time*, *Without properly synchronizing the disk accesses (using IPC!), we might read stale values*.

To rectify this, we made the app to generate User Agent everytime it is fired up and made IPC call to fetch it from the main process into the renderer and send it to the server.

#### [#D941 - Use Electron's API to automatically switch tray icon colors on macOS based on theme(minor)(UX enhancement)](https://github.com/zulip/zulip-desktop/pull/941)
For macOS, in dark theme, Zulip Desktop had a white logo coherent to other logos int he system tray, but in light theme it had the signature logo which would break the coherence. We used Electron's [Template Image](https://www.electronjs.org/docs/api/native-image#template-image) which has a black and an alpha channel to automatically adjust to the theme and provide a more flushed experience.

#### [#D957 - Setup Transifex for better translation process](https://github.com/zulip/zulip-desktop/pull/957)
Earlier, Zulip Webapp and Zulip Mobile used [Transifex](https://www.transifex.com/zulip/zulip/dashboard/) as it's translation clients where translators can translate each string independently. Zulip Desktop used JSON modules without any service so users would have to change the translation string and make pull requests which was very chaotic and inefficient.

Transifex client was set up for Zulip Desktop which allowed translators to tarnslate the files on Transifex server and the maintainer only needs to sync the translations before making a release.

#### [#D967 - Create native Context Menu and Use Electron 8 built-in spellchecker](https://github.com/zulip/zulip-desktop/pull/967)
Earlier, Zulip Desktop used a non-native package - [electron-spellchecker](https://www.npmjs.com/package/electron-spellchecker) from [electron-userland](https://github.com/electron-userland/) - to spellcheck. Electron added support for native spellchecker and the app thus migrated from using the 3rd party module to native module.

One drawback that came with it was the lack of a context menu and thus it was created according to the needs of the desktop client.


#### [#D975 - Fix zoom issues (minor)(UX enhancement)](https://github.com/zulip/zulip-desktop/pull/975)
I, myself, first reported the issue which was very frustrated but was not consistetly reproduced but then many user's confirmed this one and upon digging in, two definitions, one from electron's `roles` and another one the app was using, were clashing. The clash was fixed and so were the zoom issues.


#### [#D993 - Replace deprecated request module with net.request](https://github.com/zulip/zulip-desktop/pull/993)
The [request](https://www.npmjs.com/package/request) package the desktop application used was deprecated and thus a need arose to use a regularly maintained module. Electron's native [net](https://www.electronjs.org/docs/api/net) API came to the rescue. net.request was chosen as the replacement which, as bonus, handled the proxy and also added the support for system certificate store. Earlier Zulip had Zulip Certificate Store where users would add certificates separately. Now, if the users have added certificate to your system store, like the way they do for Chrome/Chromium apps, there is no need to do the same for Zulip app. It's a win-win, right? 😁

### Open Pull Requests
#### [#D896 - Add feature to enable Time based DND Mode](https://github.com/zulip/zulip-desktop/pull/896)
DND (Do Not Disturb) has been present in Zulip Desktop but an idea of configuring it with some time interval which would automatically go off has been there and needed implementation. This PR adds the support for time based DND mode using [node-schedule](https://www.npmjs.com/package/node-schedule) npm package which schedules a node-schedule job at a given time from the present. The job had instruction to disable the DND mode at that time. It also has an 'Until I Disable' option if user likes to go old school. 😄

#### [#D1018 - Complete BrowserView Migration](https://github.com/zulip/zulip-desktop/pull/1018)
[Vipul Sharma](https://github.com/vsvipul) started BrowserView migration in [#D793](https://github.com/zulip/zulip-desktop/pull/793/) and [Kanishk Kakar](https://github.com/kanishk98/) picked it up in [#D831](https://github.com/zulip/zulip-desktop/pull/831/). The PR was still not merged and a lot of work had been done and it required rebasing cleaning and some redefinitions.

The PR is a WIP.

#### [#D1022 - Migrate darwin-notifications to electron's native Notifications](https://github.com/zulip/zulip-desktop/pull/1022)
Many users reported the issue on macOS where when they received a notification, it would make the sound but won't generate the push notification which was annoying.
As a potential solution, we are moving away from the 3rd party [node-mac-notifier](https://www.npmjs.com/package/node-mac-notifier) npm package to electron's native [Notification](https://www.electronjs.org/docs/api/notification#notification) API.

The PR is a WIP.
