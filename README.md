Here is my code I use for Plex Media Playback through Home Assistant Voice Commands

If you are interested in using this I would recommend copying the code in the plex_video_stream.yaml file and pasting it into a new blank Automation. Then you can use the UI to make any needed changes rather thn going through all the code. 

Below are some brief descriptions of some of the elements and logic behind the code. In anyone has any tips or suggestions on how to improve the code let me know. 

Each script/sentence type has two versions, a specific device/player script which requires the name of the device you want your media to play on and a no device/player script (these will include noplayer in the trigger id). The no device script needs voice satellites set up in a specific area and a specific choose set up that uses the condition of the area id of the voice satellite to determine which choose to use in the script. 

Script Types:

Stream Movie (trigger id: streammovie or streammovienoplayer): This will play the movie requested through voice. I had to separate Movies and Shows as different sentences due to how Plex deals with them as different libraries. 

Stream Unwatched (trigger id: streamunwatched or streamunwatchednoplayer): This script will play the first unwatched episode for the requested show. It will also check to see if any playback has started (defaulted to 8 seconds of waiting but this can be changed). If nothing has started in that time the shows first epsiode will play, whether that episode is unwatched or not. A queue of all the episodes in order will be used and it will include other shows with the same in this queue (i.e. If Clone High is requested the original Clone High episodes will start and then continue into the Clone High reboot.) This is by far the most complex script type I have in this collection.

Stream Specific Episode (trigger id: streamspecificepisode or streamspecificepisodenoplayer): This will play the specific episode of the request show. The season and episode number must be used. 

Stream Random Episodes (trigger id: streamrandomepisode or streamrandomepisodenoplayer): This will set up a queue of random episodes for a specific show across all seasons. 

Stream Random Epsiodes from Specific Season (trigger id: streamrandomseason or streamrandomseasonnoplayer): This will set up a queue of random episodes from a specific season. 

Stream Playlist (trigger id: streamplaylist or streamplaylistnoplayer): This will queue up random episodes from the requested playlist and restart and unwatched episodes. The shuffle and restarted unwatched episode can be disabled if needed. The Server ID and the Playlist ID will need to be used, I had a hard time getting tv show playlist to work any other way. I will show a simple way to get these IDs below. 

Resume/Pause/Stop Playback: These will run the specific command stated in the request. Some players do not have a stop function through Home Assistant. 


The first thing the automation does after receiving the request is set up variables to be used for the rest of the automation. The variables use the values that come up in the sentence slots (which are in {}, like { movie } for the streammoive sentence)

Variables:

area: This gets the area ID of the voice satellite/device used for the request. The area for the device can be set up through the devices info page in Home Assistant. 

show and title: This will get the requested show from the sentence. The title variable will have the value used in the sentence. The show variable checks to see if the show value in the sentence matches any of the "custom show names" set up. For example if I say The Office I have it set up to start playing the American version (The Office (US)) instead of the original UK version. If the value doesn't match anything it will fallback to using the title variable. This can be cusomtized to your needs. 

movie and movietitle: Same as the show and title variables but for movies. 

playlist: This variable is used to get the playlist name and use the playlist ID in the automation.

mediaplayer: This variable grabs the requested player and uses the specific player ID for the automation

mediaplayercontrol: I use to control the device the media is playing on, not the plex player. You may not need this but I found this to be useful. 

