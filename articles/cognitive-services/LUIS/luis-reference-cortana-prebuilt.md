---
title: Cortana prebuilt app reference | Microsoft Docs
description: Reference for the Cortana personal assistant, a prebuilt application from Language Understanding Intelligent Services (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
---

# Cortana prebuilt app reference
This reference lists the intents and entities that the Cortana prebuilt app recognizes.

## Cortana prebuilt intents
The prebuilt personal assistant application can identify the following intents:

| Intent | Example utterances |
|--------| ------------------|
| builtin.intent.alarm.alarm_other|update my 7:30 alarm to be eight o'clock <br/>change my alarm from 8am to 9am |
| builtin.intent.alarm.delete_alarm|delete an alarm <br/>delete my alarm "wake up"|
| builtin.intent.alarm.find_alarm|what time is my wake-up alarm set for? <br/> is my wake-up alarm on? |
| builtin.intent.alarm.set_alarm|turn on my wake-up alarm<br/>can you set an alarm for 12 called take antibiotics?|
| builtin.intent.alarm.snooze|snooze alarm for 5 minutes<br/>snooze alarm|
| builtin.intent.alarm.time_remaining| how much longer do I have until "wake-up"? <br/> how much time until my next alarm?|
| builtin.intent.alarm.turn_off_alarm | turn off my 7am alarm <br/> turn off my wake-up alarm |
| builtin.intent.calendar.change_calendar_entry| change an appointment<br/>move my meeting from today to tomorrow|
|builtin.intent.calendar.check_availability|is tim busy on Friday?<br/>is brian free on Saturday|
|builtin.intent.calendar.connect_to_meeting|connect to a meeting<br/>join the meeting online|
|builtin.intent.calendar.create_calendar_entry|I need to schedule an appointment with tony for Friday<br/>make an appointment with lisa at 2pm on Sunday|
|builtin.intent.calendar.delete_calendar_entry|delete my appointment<br/>remove the meeting at 3 pm tomorrow from my calendar|
|builtin.intent.calendar.find_calendar_entry|show my calendar<br/>display my weekly calendar|
|builtin.intent.calendar.find_calendar_when|when is my next meeting with jon?<br/>what time is my appointment with larry on Monday?|
|builtin.intent.calendar.find_calendar_where|show me the location of my 6 o'clock meeting<br/>where is that meeting with jon?|
|builtin.intent.calendar.find_calendar_who|who is this meeting with?<br/>who else is invited?|
|builtin.intent.calendar.find_calendar_why|what are the details of my 11 o'clock meeting?<br/>what is that meeting with jon about?|
|builtin.intent.calendar.find_duration|how long is my next meeting<br/>how many minutes long is my meeting this afternoon|
|builtin.intent.calendar.time_remaining|how much time until my next appointment?<br/>how much more time do I have before the meeting?|
|builtin.intent.communication.add_contact|save this number and put the name as jose<br/>put jason in my contacts list|
|builtin.intent.communication.answer_phone|answer<br/>answer the call|
|builtin.intent.communication.assign_nickname|edit nickname for nickolas<br/>add a nickname for john doe|
|builtin.intent.communication.call_voice_mail|voice mail<br/>call voicemail|
|builtin.intent.communication.find_contact|show me my contacts<br/>find contacts|
|builtin.intent.communication.forwarding_off|stop forwarding my calls<br/>switch off call forwarding|
|builtin.intent.communication.forwarding_on|set call forwarding to send calls to home phone<br/>forward calls to home phone|
|builtin.intent.communication.ignore_incoming|do not answer that call<br/>not now, I'm busy|
|builtin.intent.communication.ignore_with_message|don't answer that call but send a message instead<br/>ignore and send a text back|
|builtin.intent.communication.make_call|call bob and john<br/>call mom and dad|
|builtin.intent.communication.press_key|dial star<br/>press the 1 2 3|
|builtin.intent.communication.read_aloud|read text<br/>what did she say in the message|
|builtin.intent.communication.redial|redial my last call<br/>redial|
|builtin.intent.communication.send_email|email to mike waters mike that dinner last week was splendid<br/>send an email to bob|
|builtin.intent.communication.send_text|send text to bob and john<br/>message|
|builtin.intent.communication.speakerphone_off|take me off speaker<br/>turn off speakerphone|
|builtin.intent.communication.speakerphone_on|speakerphone mode<br/>put on speakerphone|
|builtin.intent.mystuff.find_attachment|find the document john emailed me yesterday <br/>find the doc from john|
|builtin.intent.mystuff.find_my_stuff|I want to edit my shopping list from yesterday<br/>find my chemistry notes from September
|builtin.intent.mystuff.search_messages|open message <br/>messages|
|builtin.intent.mystuff.transform_my_stuff|share my shopping list with my husband<br/>delete my shopping list|
|builtin.intent.ondevice.open_setting|open cortana settings <br/>jump to notifications|
|builtin.intent.ondevice.pause|turn off music<br/>music off|
|builtin.intent.ondevice.play_music|I want to hear owner of a lonely heart<br/>play some gospel music|
|builtin.intent.ondevice.recognize_song|tell me what this song is<br/>analyze and retrieve title of song|
|builtin.intent.ondevice.repeat|repeat this track<br/>play this song again|
|builtin.intent.ondevice.resume|restart music<br/>start music again|
|builtin.intent.ondevice.skip_back|back up a track<br/>previous song|
|builtin.intent.ondevice.skip_forward|next song<br/>skip ahead a track|
|builtin.intent.ondevice.turn_off_setting|wifi off<br/>disable airplane mode|
|builtin.intent.ondevice.turn_on_setting|wifi on<br/>turn on bluetooth|
|builtin.intent.places.add_favorite_place|add this address to my favorites<br/>save this location to my favorites|
|builtin.intent.places.book_public_transportation|buy a train ticket<br/>book a tram pass|
|builtin.intent.places.book_taxi|can you find me a ride at this hour?<br/>find a taxi|
|builtin.intent.places.check_area_traffic|what's the traffic like on 520<br/>how is the traffic on i-5|
|builtin.intent.places.check_into_place|check into joya<br/>check in here|
|builtin.intent.places.check_route_traffic|show me the traffic on the way to kirkland<br/>how is the traffic to seattle?|
|builtin.intent.places.find_place|where do I live <br/>give me the top 3 Japanese restaurants|
|builtin.intent.places.get_address|show me the address of guu on robson street<br/>what is the address of the nearest starbucks?|
|builtin.intent.places.get_coupon|find deals on tvs in my area<br/>discounts in mountain view|
|builtin.intent.places.get_distance|how far away is holiday inn?<br/>what's the distance to tahoe|
|builtin.intent.places.get_hours|what are bar del corso's hours on Mondays?<br/>when is the library open?|
|builtin.intent.places.get_menu|show me applebee's menu<br/>what's on the menu at sizzler|
|builtin.intent.places.get_phone_number|give the number for home depot<br/>what is the phone number of the nearest starbucks?|
|builtin.intent.places.get_price_range|how much does dinner at red lobster cost<br/>how expensive is purple cafe|
|builtin.intent.places.get_reviews|show me reviews for local hardware stores<br/>I want to see restaurant reviews|
|builtin.intent.places.get_route|give me directions to|it is possible to get to pizza hut on foot|
|builtin.intent.places.get_star_rating|how many stars does the French laundry have?<br/>is the aquarium in monterrey good?|
|builtin.intent.places.get_transportation_schedule|what time does the san francisco ferry leave?<br/>when is the latest train to seattle?|
|builtin.intent.places.get_travel_time|what's the driving time to denver from SF<br/>how long will it take to get to san francisco from here|
|builtin.intent.places.make_call|call Dr. smith in bellevue<br/>call the home depot on 1st street|
|builtin.intent.places.rate_place|give a rating for this restaurant<br/>rate il fornaio 5 stars on yelp|
|builtin.intent.places.show_map|what's my current location <br/>what's my location|
|builtin.intent.places.takes_reservations|is it possible to make a reservation at the olive garden<br/>does the art gallery accept reservations|
|builtin.intent.reminder.change_reminder|change a reminder<br/>move up my picture day reminder 30 minutes|
|builtin.intent.reminder.create_single_reminder|remind me to wake up at 7 am<br/>remind me to go trick or treating with luca at 4:40pm|
|builtin.intent.reminder.delete_reminder|delete this reminder<br/>delete my picture reminder|
|builtin.intent.reminder.find_reminder|do I have any reminders set up for today<br/>do I have a reminder set for luca's party|
|builtin.intent.reminder.read_aloud|read reminder out loud<br/>read that reminder|
|builtin.intent.reminder.snooze|snooze that reminder until tomorrow<br/>snooze this reminder|
|builtin.intent.reminder.turn_off_reminder|turn off reminder<br/>dismiss airport pickup reminder|
|builtin.intent.weather.change_temperature_unit|change from fahrenheit to kelvin<br/>change from fahrenheit to celsius|
|builtin.intent.weather.check_weather|show me the forecast for my upcoming trip to seattle<br/>how will the weather be this weekend|
|builtin.intent.weather.check_weather_facts|what is the weather like in hawaii in December?<br/>what was the temperature this time last year?|
|builtin.intent.weather.compare_weather|give me a comparison between the temperature high and lows of dallas and houston, tx<br/>how does the weather compare to slc and nyc|
|builtin.intent.weather.get_frequent_locations|give me my most frequent location<br/>show most often stops|
|builtin.intent.weather.get_weather_advisory|weather warnings<br/>show weather advisory for sacramento|
|builtin.intent.weather.get_weather_maps|display a weather map<br/>show me weather maps for africa|
|builtin.intent.weather.question_weather|will it be foggy tomorrow morning?<br/>will I need to shovel snow this weekend?|
|builtin.intent.weather.show_weather_progression|show local weather radar<br/>begin radar|
|builtin.intent.none|how old are you<br/>open camera|

## Prebuilt entities 

Here are some examples of entities the prebuilt personal assistant application can identify:

|Entity	|Example of entity in utterance |
|-------|------------------|
|builtin.alarm.alarm_state | turn `off` my wake-up alarm	<br/> is my wake-up alarm `on`	| 
|builtin.alarm.duration |snooze for `10 minutes`	<br/> snooze alarm for `5 minutes`	|
|builtin.alarm.start_date | set an alarm for `monday` at 7 am	<br/> set an alarm for `tomorrow` at noon	|
|builtin.alarm.start_time | create an alarm for `30 minutes`	<br/> set the alarm to go off `in 20 minutes`	|
|builtin.alarm.title | is my `wake up` alarm on	<br/> can you set an alarm for quarter to 12 Monday to Friday called `take antibiotics`	|
|builtin.calendar.absolute_location | create an appointment for tomorrow at `123 main street`	<br/> the meeting will take place in `cincinnati` on the 5th of june	|
|builtin.calendar.contact_name|put a marketing meeting on my calendar and be sure that `joe` is there <br/>I want to set up a lunch at il fornaio and invite `paul` |	
|builtin.calendar.destination_calendar|add this to my `work` schedule	<br/>put this on my `personal` calendar	|
|builtin.calendar.duration| set up an appointment for `an hour` at 6 tonight <br/>	book a `2 hour` meeting with joe |	
|builtin.calendar.end_date | create a calendar entry called vacation from tomorrow until `next monday` <br/>	block my time as busy until `monday, october 5th` |	
|builtin.calendar.end_time | the meeting ends at `5:30 PM` <br/> schedule it from 11 to `noon`	|	
|builtin.calendar.implicit_location | cancel the appointment at the dmv	<br/> change the location of miles' birthday to `poppy restaurant` |	
|builtin.calendar.move_earlier_time | push the meeting forward `an hour` <br/> move the dentist's appointment up `30 minutes`		|
|builtin.calendar.move_later_time | move my dentist appointment `30 minutes`	<br/> push the meeting out `an hour` |	
|builtin.calendar.original_start_date | reschedule my appointment at the barber from 'Tuesday' to Wednesday	<br/> move my meeting with ken from `monday` to Tuesday	|
|builtin.calendar.original_start_time | reschedule my meeting from `2:00` to 3	<br/> change my dentist appointment from `3:30` to 4 |	
|builtin.calendar.start_date | what time does my party start on `flag day` <br/> schedule lunch for the `friday after next` at noon	
|builtin.calendar.start_time | I want to schedule it for `this morning`	<br/> I want to schedule it in the `morning` |	
|builtin.calendar.title | `vet appointment`	 <br/> `dentist` Tuesday |
|builtin.communication.audio_device_type | make the call using `bluetooth`	<br/> call using my `headset` |	
|builtin.communication.contact_name | text `bob jones`	<br/> | call `sarah`|
|builtin.communication.destination_platform | call dave in `london`	<br/> call his `work line` |	
|builtin.communication.from_relationship_name | show calls from my `daughter` <br/> read the email from `mom`	|	
|builtin.communication.key | dial `star` <br/> 	press the `hash` key	|
|builtin.communication.message | email carly to say `i'm running late` <br/> text angus smith `good luck on your exam` |	
|builtin.communication.message_category | new email marked for `follow up`	<br/> new email marked `high priority` |	
|builtin.communication.message_type | send an `email`	<br/> read my `text` messages aloud	|
|builtin.communication.phone_number | I want to dial `1-800-328-9459` <br/> 	call `555-555-5555` |	
|builtin.communication.relationship_name | text my `husband` <br/>	email `family`|	
|builtin.communication.slot_attribute | change the `recipient` <br/>	change the `text` |	
|builtin.comminication.source_platform | call him from `skype` <br/> call him from my `personal line` |
|builtin.mystuff.attachment| with documents `attached` <br/> find the email `attachment` bob sent |
|builtin.mystuff.contact_name| find the spreadsheet lisa sent to `me` <br/> where's the document i sent to `susan` last night |
|builtin.mystuff.data_source | `c:\dev\` <br/> my `desktop` <br/> |`"resolution": { "resolution_type": "metadataItems", "metadataType": CanonicalEntity", "value": "desktop" }`|
|builtin.mystuff.data_type | locate the `document` i worked on last night <br/> |`"resolution": {"resolution_type": "metadataItems", "metadataType": "CanonicalEntity", "value":Document" }` <br/> bring up mike's `visio diagram`	|
|builtin.mystuff.end_date| show me the docs i worked on `between yesterday and today`	<br/> find what doc i was working on `before thursday the 31st` |
|builtin.mystuff.end_time| find files i saved `before noon` <br/> find what doc i was working on `before 4pm`|		
|builtin.mystuff.file_action| open the spreadsheet i `saved` yesterday <br/> find the spreadsheet kevin `created`| 
|builtin.mystuff.from_contact_name | find the proposal `jason` sent me <br/> open `isaac` 's last email |
|builtin.mystuff.keyword | show me the `french conjugation` files <br/> find the `marketing plan` i drafted yesterday |
|builtin.mystuff.location | the document i edited at `work` <br/> photos i took in `paris` |
|builtin.mystuff.message_category | look for my `new` emails <br/> search for my `high priority` email |
|builtin.mystuff.message_type | check my `email` <br/>	show me my `text` messages	|
|builtin.mystuff.source_platform | search my `hotmail` email for email from john	<br/> find the document i sent from `work` |
|builtin.mystuff.start_date | find notes from `january` <br/> find the email i send rob `after january 1st`	|
|builtin.mystuff.start_time | find that email i sent rob sometime before 2pm but `after noon`? <br/> find the worksheet kristin sent to me that I edited `last night` |	
|builtin.mystuff.title | `c:\dev\mystuff.txt` <br/> `*.txt` |
|builtin.mystuff.transform_action | `download` the file john sent me <br/> `open` my annotation guidelines doc |
|builtin.note.note_text | create a grocery list including `pork chops, applesauce and milk` <br/> make a note to `buy milk` |
|builtin.note.title | make a note called `grocery list` <br/> make a note called `people to call` |
|builtin.ondevice.music_artist_name | play everything by `rufus wainwright` <br/> play `garth brooks` music |	
|builtin.ondevice.music_genre | show `classic rock` songs <br/>	play my `classical` music from the baroque period |
|builtin.ondevice.music_playlist | shuffle all britney spears from `workout` playlist <br/> play `breakup` playlist
|builtin.ondevice.music_song_name | play `summertime` <br/> play `me and bobby mcgee` |	
|builtin.ondevice.setting_type | `quiet hours` <br/> `airplane mode` |
|builtin.places.absolute_location | take me to the intersection of `5th and pike` <br/> no, I want directions to `1 microsoft way redmond wa 98052` |
|builtin.places.atmosphere | look for `interesting` places to go out	<br/> where can I find a `casual` restaurant |	
|builtin.places.audio_device_type |call the post office on `hands free` <br/> call papa john's with `speakerphone` |	
|builtin.places.avoid_route | avoid the `toll road` <br/> get me to san francisco avoiding the `construction on 101` |	
|builtin.places.cuisine | `halal` deli near mountain view	<br/> `kosher` fine dining on the peninsula	|
|builtin.places.date | make a reservation for `next friday the 12th` <br/>	is mashiko open on `mondays` ? |
|builtin.places.discount_type | find a `coupon` for macy's <br/> find me a `coupon` |
|builtin.places.distance | is there a good diner `within 5 miles` of here <br/> find ones `within 15 miles` |	
|builtin.places.from_absolute_location | directions from `45 elm street` to home <br/> get me directions from `san francisco` to palo alto	|
|builtin.places.from_place_name | driving from the `post office` to 56 center street <br/> get me directions from `home depot` to lowes	|
|builtin.places.from_place_type | directions to downtown from `work` <br/>  get me directions from the `drug store` to home	|
|builtin.places.meal_type | nearby places for `dinner` <br/> find a good place for a business `lunch` |	
|builtin.places.nearby | show me some cool shops `near` me	<br/> are there any good Lebanese restaurants `around` here? |
|builtin.places.open_status | when is the mall `closed` <br/> get me the `opening` hours of the store |	
|builtin.places.place_name | take me to `central park` <br/> look up the `eiffel tower` |	
|builtin.places.place_type | `atms` <br/> `post office` |
|builtin.places.prefer_route | show directions by the `shortest` route <br/> take the `fastest` route |	
|builtin.places.price_range| give me places that are `moderately affordable` <br/>	I want an `expensive` one	|
|builtin.places.product | where can I get `fresh fish` around here	<br/> where around here sells `bare minerals` |
|builtin.places.public_transportation_route | bus schedule for the `m2` bus	<br/> bus route `3x` |	
|builtin.places.rating | show `3 star` restaurants <br/> show results that are `3 stars or higher`	|
|builtin.places.reservation_number | book a table for `seven` people <br/> 	make a reservation for `two` at il fornaio |
|builtin.places.results_number | show me the `10` coffee shops closest to here <br/> show me top `3` aquariums	
|builtin.places.service_provided | where can I go to `whale watch` by bus ?	<br/> I need a mechanic to `fix my brakes` |	
|builtin.places.time | I want places that are open on Saturday at `8 am`| is mashiko open `now` |
|builtin.places.transportation_company | train schedules for `new jersey transit` <br/> can I get there on `bart` |	
|builtin.places.transportation_type | where is a music store i can get to `on foot`? <br/>	give me `biking` directions to mashiko|
|builtin.places.travel_time | I want to be able to drive `less than 15 minutes` <br/>	I want somewhere I can get to in `under 15 minutes` |	
|builtin.reminder.absolute_location | remind me to call my dad when i land in `chicago` <br/> when I get back to `seattle` remind me to get gas	|
|builtin.reminder.contact_name | when `bob` calls, remind me to tell him the joke <br/> create a reminder to mention the school bus when I talk to `arthur` |
|builtin.reminder.leaving_absolute_location | reminder pick up craig when leaving `1200 main` |
|builtin.reminder.leaving_implicit_location | remind me to get gas when I leave `work`	|
|builtin.reminder.original_start_date | change the reminder about the lawn from `saturday` to Sunday <br/> move my reminder about school from `monday` to Tuesday	|
|builtin.reminder.relationship_name | when my `husband` calls, remind me to tell him about the pta meeting <br/> remind me again when `mom` calls|
|builtin.reminder.reminder_text | can you remind me to `bring up my small spot of patchy skin` when dr smith'calls	<br/> remind me to `pick up dry cleaning` at 4:40	|
|builtin.reminder.start_date | remind me the `thursday after next` at 8 pm	<br/> remind me `next thursday the 18th` at 8 pm	|
|builtin.reminder.start_time | create a reminder `in 30 minutes` <br/> create a reminder to water the plants `this evening at 7` |	
|builtin.weather.absolute_location | will it rain in `boston` <br/> what's the forecast for `seattle`?	|
|builtin.weather.date_range | weather in nyc `this weekend` <br/> 	look up the `five day` forecast in hollywood florida |
|builtin.weather.suitable_for | can I go `hiking` in shorts this weekend?	<br/> will it be nice enough to `walk` to the game today? |	
|builtin.weather.temperature_unit | what is the temperature today in `kelvin`	<br/> show me the temps in `celsius` |	
|builtin.weather.time_range | does it look like it will snow `tonight`?	<br/> is it windy right `now`?	|
|builtin.weather.weather_condition | show `precipitation` <br/>	how thick is the `snow` at lake tahoe now?	|

