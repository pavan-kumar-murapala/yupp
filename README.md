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
