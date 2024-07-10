## Instructions:

## Uncompress the lab (pass: cyberdefenders.org)

## Jailbroken is an iPad case investigation that exposes different aspects of IOS systems where you can evaluate your DFIR skills against an OS you usually encounter in today's case investigations as a security blue team member.

### Tools:

  * iLEAPP
  * Autopsy
  * mac_apt
  * SqliteDB Browser

## Solution

### Q1

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/7c8a7e8f-46b4-4c2f-aeee-d74816f157a0)

* `private/var/installd/Library/MobileInstallation/LastBuildInfo.plist`: iOS and Build Version

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/fda06696-5be9-4fcf-915c-26f6ca4a688a)

`9.3.5`

### Q2

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/b621a4ec-99bd-40e2-b9d5-24acc3ef95e8)

* I searched for account detail in ios
* `/private/var/mobile/Library/Accounts/Accounts3.sqlite`: Account infomation
* there's only one account named Tim Apple

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/d7c78359-979b-434b-96d4-54c27ea50e05)

`Tim Apple`

### Q3

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/9a9b5e05-489c-4564-bb66-8c37624df6cc)

* battery data is stored in the `private\var\containers\Shared\SystemGroup\4212B332-3DD8-449B-81B8-DBB62BCD3423\Library\BatteryLife\CurrentPowerlog.PLSQL`
* in table PLBatteryAgent_EventBackward_Battery, there are battery status, sort to get the latest time has level 100 (the biggest epoch timestamp)

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/046944e7-492b-4d6e-9357-165228ed1293)

* Biggest timestamp: 1586976031.21529
* Convert to date time

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/7fef4bae-1145-426f-b219-5c55d6d64f03)

`04/15/2020 06:40:31 PM`

### Q4

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/e2102770-dccf-4f04-beae-cdc1dedf4a69)

* search the browser history to indentify which search is the most times
* IOS default browser is Safari
* the history of Safari was store in `History.db`

`\private\var\mobile\Containers\Data\Application\FB1B2A1C-AC19-406F-BEEC-EC048BF504EA\Library\Safari\History.db`

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/f8edc3f4-51db-4894-964b-f27ef927f5eb)

* `kirby with legs` was the search has most times, 16 times

`kirby with legs`

### Q5

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/2ec96678-dab4-430e-bb9b-149bb6846b9c)

* Search for the podcast download history, base to this `https://salt4n6.com/tag/podcasts/`
* data is store in MTLibrary.sqlite
* in ZMTEPISODE table, the title of the first podcast

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/ef07184b-4084-4d29-be1b-30ff66958905)

`WHERE ARE WE?`

### Q6 

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/556c5878-52b0-4ed6-87c6-3661a8e850b2)

* file `com.apple.wifi.plist` in `/private/var/preferences/SystemConfiguration/` store wifi connection detail

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/f144b5f8-c46c-437e-b88d-ae2877a20819)

* name of wifi is `black lab`

`black lab`

### Q7

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/8a930c98-6d4e-41d6-8403-76535450eb8b)

* I search in Application, there is a GBA4iOS, Game Boy Advance
* it's skin file is .gbaskin file

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/7faecff2-16eb-4d2b-96d3-0d7b4f3dd81b)

`Default.gbaskin`

### Q8

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/238c683b-c0ed-4581-bf07-16213e89d2ee)

* in `CurrentPowerlog.PLSQL`, table `PLAppTimeService_Aggregate_AppRunTime` store background runtime of applications

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/05ff1ee3-688e-4df2-80e4-704689de7c1d)

* news app com.apple.news have background runtime is `197.810275`

`197.810275`

### Q9

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/16e92f2d-060b-4818-b6e8-8cd8023d42ac)

* `private\var\containers\Bundle\Application\` store all applications was installed
* 3 applications was installed
* first app install was Phoenix, but it isn't from appstore
* the first appstore installed app is `Cookie Run: OvenBreak`

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/90a4669c-46bf-4fd6-8872-ab97fc46d797)

`Cookie Run`

### Q10

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/1078dd48-3ece-40fb-be14-dcf5fb215380)

* in all installed app, there was an app `Phoenix`

* Some document show that app to jailbreak ios 9.3.5 - 9.3.6 is Phoenix Jailbreak

`Phoenix`

### Q11

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/03dd4b88-6698-4214-b06f-f7532a94869a)

* there was 2 apps from appstore
* `Cookie Run` and `Pokémon Quest`

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/7fe410bc-c744-47fd-ad36-aab00da4f2bb)

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/f64d53cd-eb76-45ec-b284-339e0dbc06a5)

`2`

### Q12

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/c6cae00f-0100-4e65-b42a-e479e94eabed)

* `\private\var\mobile\Documents\Save States\`
* the most recently game was obtained is `Legend of Zelda, The - The Minish Cap`
* it has only 1 save state

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/3a8e6df3-9f65-4b8e-b1fe-f1a30d1a427b)

`1`

### Q13

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/4b666cb5-a2eb-4d4f-8784-d5652beb4457)

* in Q5, when I found 2 podcasts, one of them is podcast of `Duolingo` with many tracks

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/75c622b1-7a05-4452-9649-5b57e804f770)

* after identifing, this is Spanish

`Spanish`

### Q14

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/f9480c57-502d-492a-8b12-d57dc7163576)

* User use Ipad to record the page of book he reading in real life, so it might be the camera
* the video was stored in `\private\var\mobile\Media\DCIM\100APPLE\`
* `IMG_0008.MOV`

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/38f4907c-5762-4195-a4eb-fad20fe73e5b)

`85`

### Q15

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/7f6b721c-388d-43ec-830e-e53d337ebd19)

* check the NoteStore.sqlite

`strings .\private\var\mobile\Containers\Shared\AppGroup\4466A521-8AF9-4E09-800B-C3203BB70E0E\NoteStore.sqlite | findstr /I 'should buy'`

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/cd2f72a7-beb4-4f8d-af57-ab841b690840)

`Racing for the PS4.Did you find me? Then you should Buy Crash Bandicoot Nitro-Fueled`

`Crash Bandicoot Nitro-Fueled Racing`

### Q16

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/c78718da-135b-4f6f-9c7e-1ed78bb57c43)

* `/private/var/mobile/Library/Preferences/com.apple.MobileSMS.plist`: SMS, iMessage and FaceTime

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/081d4f98-128c-489d-b210-bf203621ff25)

`com.apple.MobileSMS`

### Q17

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/f4b6cbef-389e-4a30-8af3-60a044bdcea2)

* in directory `\private\var\mobile\Library\Calendar\`, file Calendar.sqlitedb store the reminder of user

![ảnh](https://github.com/LDV-SpaceK/Cyberdefenders.org/assets/151914246/9f02219c-bab4-43b5-be79-fd7cfd4c0b7a)

* in CalendarItem table , there are two reminder, `Get milk` and `Go to bed BEFORE 5 am`

`milk`








