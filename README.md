Xtream Codes API
Isaac Pateau edited this page on 24 Apr 2018 · 4 revisions
Base Endpoint
This endpoint is common to every Xtream Codes based provider and is used to download a M3U playlist to the user. It's structure will in most cases have this form/syntax:

http://url:port/get.php?username=xxxx&password=xxxx&type=m3u_plus&output=ts

url: The URL of the service provider
port: The port used by the service provider
username: The username of an account from this provider
password: The password of an account from this provider
type: The type of playist. By default, m3u and m3u_plus are available for options but m3u_plus is the option given by default. m3u_plus is a special type of playlist where the EPG ID and logo URL for a given AvContent
output: The output type of the stream. ts, hls and rtmp are available. By default it will be ts which represents the MPEG-TS container. The two other formats are hls for HTTP Live Streaming (HLS) and rtmp for Real Time Messaging Protocol (RTMP)
Note: The playlist used by this URL normally represents all the media available from a given provider. xipl filters Live TV channels if importing channels using a ProviderEpgService.

Android API
As a provided service by Xtream Codes, providers are generally given a custom made Android application in which a special API is used to retrieve different media types. All responses from the API returns a M3U based playlist available for processing.

Endpoint: http://mikkm.xyz/android/androidApi.php?url=http://abcd.xyz:port/&mode=xxxx&username=xxxx&password=xxxx

Parameters:

url: The URL of the service provider
port: The port used by the service provider
mode: The given media type wanted by the client application. The choices are between live, series, vod and timeshift
username: The username of an account from this provider
password: The password of an account from this provider
Media modes
These are the different media modes supported by the Xtream Codes API when obtaining content through the Android API. Do note that the classification of the media is solely the responsibility of the provider and that some content might not be categorized correctly as such.

live: Returns a list of live TV channels. The content returned is using MPEG-TS as a container format.
vod: Returns a list of Video-On-Demand (VOD) content. (This can include movies, sport events, TV shows, etc.) The format is likely to vary depending on the provider itself.
timeshift: Returns a list of prerecorded shows for a given TV channel. The format supported is MPEG-TS.
series: Returns a list of TV series that might be considered like Video-on-Demand (VOD). The format is dependent on the provider itself.
Timeshift EPG adjustment
A special parameter epg_shift is available if a user's timezone doesn't quite fit with the provider's when obtaining the information regarding TV Catchup.

Endpoint: http://mikkm.xyz/android/androidApi.php?url=http://abcd.xyz:port/&mode=timeshift?epg_shift=x&username=xxxx&password=xxxx

Where epg_shift is an integer value in hours between the provider and the user timezone. (Ideally between -12 and 12)

API V2


Hello,

These are the new API Calls, that you have to use. It is highly recommended to use this API as it is much faster than the previous one, and also more accurate and with abilities to be extended much more easier and faster than before.


Note: The API Does not provide Full links to the requested stream. You have to build the url to the stream in order to play it.

For Live Streams the main format is
           http(s)://domain:port/live/username/password/streamID.ext ( In  allowed_output_formats element you have the available ext )

For VOD Streams the format is:

http(s)://domain:port/movie/username/password/streamID.ext ( In  target_container element you have the available ext )
 
For Series Streams the format is

http(s)://domain:port/series/username/password/streamID.ext ( In  target_container element you have the available ext )

On the First Authentication Call, you will get a list of some useful elements. If the server_protocol element is https you have to force all the API & Player Requests to be through https. The same Call, also provides the http(s) ports in case you need them as well as the domain name you should use.
The current datetime & timestamps are offered to you and can help you for time corrections for EPG & TV Archive.

If you want to limit the displayed output data, you can use params[offset]=X & params[items_per_page]=X on your call.

It is higly recommended to fetch all data at once and cache them to your Application.

 

 

Authentication

player_api.php?username=X&password=X

GET Live Stream Categories

player_api.php?username=X&password=X&action=get_live_categories

GET VOD Stream Categories

player_api.php?username=X&password=X&action=get_vod_categories
 

GET SERIES Categories

player_api.php?username=X&password=X&action=get_series_categories
 

GET LIVE Streams

player_api.php?username=X&password=X&action=get_live_streams  (This will get All LIVE Streams)
player_api.php?username=X&password=X&action=get_live_streams&category_id=X  (This will get All LIVE Streams in the selected category ONLY)

GET VOD Streams 

player_api.php?username=X&password=X&action=get_vod_streams  (This will get All VOD Streams)
player_api.php?username=X&password=X&action=get_vod_streams&category_id=X  (This will get All VOD Streams in the selected category ONLY) 
 

GET SERIES Streams 

player_api.php?username=X&password=X&action=get_series  (This will get All Series)
player_api.php?username=X&password=X&action=get_series&category_id=X  (This will get All Series  in the selected category ONLY) 

GET SERIES Info

player_api.php?username=X&password=X&action=get_series_info&series_id=X
 

The seasons array, might be filled or might be completely empty. If it is not empty, it will contain the cover, overview and the air date of the selected season.
In your APP if you want to display the series, you have to take that from the episodes array.

 

The array keys of episodes array, are the seasons. IF the season key exists in seasons array, you can display a cover and information for that season. Here is an example of PHP Code.

<?php
$output = json_decode(file_get_contents("http://ip:port/player_api.php?username=api_test&password=api_test&action=get_series_info&series_id=X"),true);
foreach ( array_keys( $output['episodes'] ) as $season_number )
{
    echo "Season $season_number";

    if ( array_key_exists( $season_number, $output['seasons'] ) )
    {
        echo "Cover: " . $output['seasons'][$season_number]['cover'];
        echo "Overview: " . $output['seasons'][$season_number]['overview'];

    }
}


GET VOD Info

player_api.php?username=X&password=X&action=get_vod_info&vod_id=X  (This will get info such as video codecs, duration, description, directors for 1 VOD)

GET short_epg for LIVE Streams (same as stalker portal, prints the next X EPG that will play soon)

player_api.php?username=X&password=X&action=get_short_epg&stream_id=X 
player_api.php?username=X&password=X&action=get_short_epg&stream_id=X&limit=X  (You can specify a limit too, without limit the default is 4 epg listings)  

 GET ALL EPG for LIVE Streams (same as stalker portal, but it will print all epg listings regardless of the day)
 

player_api.php?username=X&password=X&action=get_simple_data_table&stream_id=X       
 

Full EPG List for all Streams

xmltv.php?username=X&password=X

