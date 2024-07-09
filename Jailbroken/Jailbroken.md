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






