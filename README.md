<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ readme.md for YT-Learn-YouTube ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h1>YT-Learn-YouTube</h1>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>YouTube Player API Reference for iFrame Embeds</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The <mark>IFrame</mark> player API lets you embed a YouTube video player on your website and control 
the player using JavaScript.</p>

<p>Using the API's JavaScript functions, you can queue videos for playback; play, pause, or 
stop those videos; adjust the player volume; or retrieve information about the video 
being played. You can also add event listeners that will execute in response to certain 
player events, such as a player state change.</p>

<p>This guide explains how to use the IFrame API. It identifies the different types of 
events that the API can send and explains how to write event listeners to respond to 
those events. It also details the different JavaScript functions that you can call to 
control the video player as well as the player parameters you can use to further 
customize the player.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Requirements</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The user's browser must support the HTML5 postMessage feature. Most modern browsers 
support postMessage.</p>

<p>Embedded players must have a viewport that is at least 200px by 200px. If the player 
displays controls, it must be large enough to fully display the controls without 
shrinking the viewport below the minimum size. We recommend 16:9 players be at least 480 
pixels wide and 270 pixels tall.</p>

<p>Any web page that uses the IFrame API must also implement the following JavaScript 
function:</p>

<p><mark>onYouTubeIframeAPIReady</mark> – The API will call this function when the page has finished 
downloading the JavaScript for the player API, which enables you to then use the API 
on your page. Thus, this function might create the player objects that you want to 
display when the page loads.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Getting started</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The sample HTML page below <b>creates</b> an embedded player that will <b>load a video</b>, <b>play</b> it for 
<b>six seconds</b>, and then <b>stop</b> the playback. The numbered comments in the HTML are explained 
in the list below the example.</p>

<details>
  <summary>YouTube on the Web</summary>

<pre>
&lt;!DOCTYPE html&gt;
&lt;html&gt;
  &lt;body&gt;
    // 1. The &lt;iframe&gt; (and video player) will replace this &lt;div&gt; tag.
    &lt;div id="player"&gt;&lt;/div&gt;

    &lt;script&gt;
      // 2. This code loads the IFrame Player API code asynchronously.
      var tag = document.createElement('script');

      tag.src = "https://www.youtube.com/iframe_api";
      var firstScriptTag = document.getElementsByTagName('script')&lbrack;0&rbrack;;
      firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

      // 3. This function creates an &lt;iframe&gt; (and YouTube player)
      //    after the API code downloads.
      var player;
      function onYouTubeIframeAPIReady() {
        player = new YT.Player('player', {
          height: '390',
          width: '640',
          videoId: 'M7lc1UVf-VE',
          playerVars: {
            'playsinline': 1
          },
          events: {
            'onReady': onPlayerReady,
            'onStateChange': onPlayerStateChange
          }
        });
      }

      // 4. The API will call this function when the video player is ready.
      function onPlayerReady(event) {
        event.target.playVideo();
      }

      // 5. The API calls this function when the player's state changes.
      //    The function indicates that when playing a video (state=1),
      //    the player should play for six seconds and then stop.
      var done = false;
      function onPlayerStateChange(event) {
        if (event.data == YT.PlayerState.PLAYING && !done) {
          setTimeout(stopVideo, 6000);
          done = true;
        }
      }
      function stopVideo() {
        player.stopVideo();
      }
    &lt;/script&gt;
    &lt;/body&gt;
    &lt;/html&gt;
</pre>

</details>

<p>The following list provides more details about the sample above:</p>

<p>The &lt;div&gt; tag in this section identifies the location on the page where the IFrame API 
will place the video player. The constructor for the player object, which is described 
in the Loading a video player section, identifies the &lt;div&gt; tag by its id to ensure 
that the API places the &lt;iframe&gt; in the proper location. Specifically, the IFrame API 
will replace the &lt;div&gt; tag with the &lt;iframe&gt; tag.</p>

<p>As an alternative, you could also put the &lt;iframe&gt; element directly on the page. The 
Loading a video player section explains how to do so.</p>

<p>The code in this section loads the IFrame Player API JavaScript code. The example uses 
DOM modification to download the API code to ensure that the code is retrieved 
asynchronously. (The &lt;script&gt; tag's async attribute, which also enables asynchronous 
downloads, is not yet supported in all modern browsers as discussed in this Stack 
Overflow answer.</p>

<p>The onYouTubeIframeAPIReady function will execute as soon as the player API code 
downloads. This portion of the code defines a global variable, player, which refers to 
the video player you are embedding, and the function then constructs the video player 
object.</p>

<p>The onPlayerReady function will execute when the onReady event fires. In this example, 
the function indicates that when the video player is ready, it should begin to play.</p>

<p>The API will call the onPlayerStateChange function when the player's state changes, 
which may indicate that the player is playing, paused, finished, and so forth. The 
function indicates that when the player state is 1 (playing), the player should play for 
six seconds and then call the stopVideo function to stop the video.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Loading a video player</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>After the API's JavaScript code loads, the API will call the onYouTubeIframeAPIReady 
function, at which point you can construct a YT.Player object to insert a video player 
on your page. The HTML excerpt below shows the onYouTubeIframeAPIReady function from 
the example above:</p>

<pre>
var player;
function onYouTubeIframeAPIReady() {
  player = new YT.Player('player', {
    height: '390',
    width: '640',
    videoId: 'M7lc1UVf-VE',
    playerVars: {
      'playsinline': 1
    },
    events: {
      'onReady': onPlayerReady,
      'onStateChange': onPlayerStateChange
    }
  });
}
</pre>

<p>The constructor for the video player specifies the following parameters:</p>
<ul>
  <li>The first parameter specifies either the DOM element or the id of the HTML element where 
the API will insert the &lt;iframe&gt; tag containing the player.</li>
</ul>

<p>The IFrame API will replace the specified element with the &lt;iframe&gt; element containing 
the player. This could affect the layout of your page if the element being replaced has 
a different display style than the inserted &lt;iframe&gt; element. By default, an &lt;iframe&gt; 
displays as an inline-block element.</p>
<ul>
  <li>The second parameter is an object that specifies player options. The object contains the 
    following properties:
    <ul>
      <li><b>width (number)</b> – The width of the video player. The default value is 640.</li>
	  <li><b>height (number)</b> – The height of the video player. The default value is 390.</li>
      <li><b>videoId (string)</b> – The YouTube video ID that identifies the video that the player will load.</li>
      <li><b>playerVars (object)</b> – The object's properties identify player parameters that can be used 
	  to customize the player.</li>
      <li><b>events (object)</b> – The object's properties identify the events that the API fires and the 
	  functions (event listeners) that the API will call when those events occur. In the example, 
	  the constructor indicates that the onPlayerReady function will execute when the onReady 
	  event fires and that the onPlayerStateChange function will execute when the onStateChange 
	  event fires.</li>
	</ul>
  </li>
</ul>
	
<p>As mentioned in the Getting started section, instead of writing an empty &lt;div&gt; element 
on your page, which the player API's JavaScript code will then replace with an &lt;iframe&gt; 
element, you could create the &lt;iframe&gt; tag yourself. The first example in the Examples 
section shows how to do this.</p>

<pre>
&lt;iframe id="player" type="text/html" width="640" height="390"
  src="http://www.youtube.com/embed/M7lc1UVf-VE?enablejsapi=1&origin=http://example.com"
  frameborder="0"&gt;&lt;/iframe&gt;
</pre>

<p>Note that if you do write the &lt;iframe&gt; tag, then when you construct the YT.Player object, 
you do not need to specify values for the width and height, which are specified as 
attributes of the &lt;iframe&gt; tag, or the videoId and player parameters, which are are 
specified in the src URL. As an extra security measure, you should also include the 
origin parameter to the URL, specifying the URL scheme (http:// or https://) and full 
domain of your host page as the parameter value. While origin is optional, including it 
protects against malicious third-party JavaScript being injected into your page and 
hijacking control of your YouTube player.</p>

<p>For other examples on constructing video player objects, see Examples.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Operations</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>To call the player API methods, you must first get a reference to the player object 
you wish to control. You obtain the reference by creating a YT.Player object as 
discussed in the Getting started and Loading a video player sections of this document.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Functions</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h5>Queueing functions</h5>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Queueing functions allow you to load and play a video, a playlist, or another list 
of videos. If you are using the object syntax described below to call these functions, 
then you can also queue or load a list of a user's uploaded videos.</p>

<p>The API supports two different syntaxes for calling the queueing functions.</p>

<p>The argument syntax requires function arguments to be listed in a prescribed order.</p>

<p>The object syntax lets you pass an object as a single parameter and to define object 
properties for the function arguments that you wish to set. In addition, the API may 
support additional functionality that the argument syntax does not support.</p>

<p>For example, the <em>loadVideoById</em> function can be called in either of the following ways. 
Note that the object syntax supports the endSeconds property, which the argument syntax 
does not support.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Argument syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>loadVideoById("bHQqvYy5KYo", 5, "large")</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Object syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
loadVideoById({'videoId': 'bHQqvYy5KYo',
               'startSeconds': 5,
               'endSeconds': 60});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Queueing functions for videos</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>cueVideoById</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Argument syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.cueVideoById(videoId:String,
                    startSeconds:Number):Void
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Object syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.cueVideoById({videoId:String,
                     startSeconds:Number,
                     endSeconds:Number}):Void
</pre>

<p>This function loads the specified video's thumbnail and prepares the player to play the 
video. The player does not request the FLV until playVideo() or seekTo() is called.</p>

<p>The required videoId parameter specifies the YouTube Video ID of the video to be played. 
In the YouTube Data API, a video resource's id property specifies the ID.</p>

<p>The optional startSeconds parameter accepts a float/integer and specifies the time from 
which the video should start playing when playVideo() is called. If you specify a 
startSeconds value and then call seekTo(), then the player plays from the time specified 
in the seekTo() call. When the video is cued and ready to play, the player will broadcast 
a video cued event (5).</p>

<p>The optional endSeconds parameter, which is only supported in object syntax, accepts a 
float/integer and specifies the time when the video should stop playing when playVideo() 
is called. If you specify an endSeconds value and then call seekTo(), the endSeconds 
value will no longer be in effect.
loadVideoById</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Argument syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.loadVideoById(videoId:String,
                     startSeconds:Number):Void
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Object syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.loadVideoById({videoId:String,
                      startSeconds:Number,
                      endSeconds:Number}):Void
</pre>

<p>This function loads and plays the specified video.</p>

<p>The required <b>videoId</b> parameter specifies the YouTube Video ID of the video to be played. 
In the YouTube <b>Data API</b>, a video resource's id property specifies the ID.</p>

<p>The optional <b>startSeconds</b> parameter accepts a float/integer. If it is specified, then the 
video will start from the closest keyframe to the specified time.</p>

<p>The optional <b>endSeconds</b> parameter accepts a float/integer. If it is specified, then the 
video will stop playing at the specified time.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>cueVideoByUrl</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Argument syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.cueVideoByUrl(mediaContentUrl:String,
                     startSeconds:Number):Void
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Object syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.cueVideoByUrl({mediaContentUrl:String,
                      startSeconds:Number,
                      endSeconds:Number}):Void
</pre>

<p>This function loads the specified video's thumbnail and prepares the player to play the 
video. The player does not request the FLV until <b>playVideo()</b> or <b>seekTo()</b> is called.</p>

<p>The required <mark>mediaContentUrl</mark> parameter specifies a fully qualified YouTube player 
URL in the format <b>http://www.youtube.com/v/VIDEO_ID?version=3</b>.</p>

<p>The optional <mark>startSeconds</mark> parameter accepts a float/integer and specifies the time from 
which the video should start playing when <b>playVideo()</b> is called. If you specify 
<b>startSeconds</b> and then call <b>seekTo()</b>, then the player plays from the time specified in the 
<b>seekTo() </b>call. When the video is cued and ready to play, the player will broadcast a 
video cued event (5).</p>

<p>The optional <mark>endSeconds</mark> parameter, which is only supported in object syntax, accepts a 
float/integer and specifies the time when the video should stop playing when <b>playVideo()</b> 
is called. If you specify an <b>endSeconds</b> value and then call <b>seekTo()</b>, the <b>endSeconds</b> 
value will no longer be in effect.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>loadVideoByUrl</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Argument syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.loadVideoByUrl(mediaContentUrl:String,
                      startSeconds:Number):Void
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Object syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.loadVideoByUrl({mediaContentUrl:String,
                       startSeconds:Number,
                       endSeconds:Number}):Void
</pre>

<p>This function loads and plays the specified video.</p>

<p>The required <mark>mediaContentUrl</mark> parameter specifies a fully qualified YouTube player URL in 
the format <b>http://www.youtube.com/v/VIDEO_ID?version=3</b>.</p>

<p>The optional <mark>startSeconds</mark> parameter accepts a float/integer and specifies the time from 
which the video should start playing. If <b>startSeconds</b> (number can be a float) is 
specified, the video will start from the closest keyframe to the specified time.</p>

<p>The optional <mark>endSeconds</mark> parameter, which is only supported in object syntax, accepts a 
float/integer and specifies the time when the video should stop playing.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Queueing functions for lists</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The <mark>cuePlaylist</mark> and <mark>loadPlaylist</mark> functions allow you to load and play 
a playlist. If you are using object syntax to call these functions, you can also queue (or load) a 
list of a user's uploaded videos.</p>

<p>Since the functions work differently depending on whether they are called using the 
argument syntax or the object syntax, both calling methods are documented below.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>cuePlaylist</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Argument syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.cuePlaylist(playlist:String|Array,
                   index:Number,
                   startSeconds:Number):Void
</pre>

<p>Queues the specified playlist. When the playlist is cued and ready to play, the player 
will broadcast a video cued event (5).</p>

<p>The required <mark>playlist</mark> parameter specifies an array of YouTube video IDs. In the YouTube 
<mark>Data API</mark>, the video resource's id property identifies that video's ID.</p>

<p>The optional <mark>index</mark> parameter specifies the index of the first video in the playlist that 
will play. The parameter uses a zero-based index, and the default parameter value is 0, 
so the default behavior is to load and play the first video in the playlist.</p>

<p>The optional <mark>startSeconds</mark> parameter accepts a float/integer and specifies the time from 
which the first video in the playlist should start playing when the <mark>playVideo()</mark> function 
is called. If you specify a <b>startSeconds</b> value and then call <b>seekTo()</b>, then the player 
plays from the time specified in the <b>seekTo()</b> call. If you cue a playlist and then call 
the <b>playVideoAt()</b> function, the player will start playing at the beginning of the 
specified video.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Object syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.cuePlaylist({listType:String,
                    list:String,
                    index:Number,
                    startSeconds:Number}):Void
</pre>

<p>Queues the specified list of videos. The list can be a playlist or a user's uploaded 
videos feed. The ability to queue a list of search results is deprecated and will no 
longer be supported as of 15 November 2020.</p>

<p>When the list is cued and ready to play, the player will broadcast a video cued event (5).</p>

<p>The optional <mark>listType</mark> property specifies the type of results feed that you are retrieving. 
Valid values are <b>playlist</b> and <b>user_uploads</b>. The default value is <b>playlist</b>.</p>

<p>The required <mark>list</mark> property contains a key that identifies the particular list of videos 
that YouTube should return.</p>

<p>If the <mark>listType</mark> property value is <b>playlist</b>, then the list property specifies the 
<b>playlist ID</b> or an array of video IDs. In the YouTube <b>Data API</b>, the playlist resource's id property 
identifies a playlist's ID, and the video resource's id property specifies a video ID.

<b>If the <b>listType</b> property value is <b>user_uploads</b>, then the list property identifies the user 
whose uploaded videos will be returned.</p>

<p>If the <b>listType</b> property value is search, then the list property specifies the search 
query. Note: This functionality is deprecated and will no longer be supported as of 
15 November 2020.

<p>The optional <mark>index</mark> property specifies the index of the first video in the list that 
will play. The parameter uses a zero-based index, and the default parameter value is 
0, so the default behavior is to load and play the first video in the list.</p>

<p>The optional <mark>startSeconds</mark> property accepts a float/integer and specifies the time from 
which the first video in the list should start playing when the <b>playVideo()</b> function is 
called. If you specify a <b>startSeconds</b> value and then call <b>seekTo()</b>, then the player 
plays from the time specified in the <b>seekTo()</b> call. If you cue a list and then call 
the <b>playVideoAt()</b> function, the player will start playing at the beginning of the 
specified video.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>loadPlaylist</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Argument syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.loadPlaylist(playlist:String|Array,
                    index:Number,
                    startSeconds:Number):Void
</pre>

<p>This function loads the specified playlist and plays it.</p>

<p>The required <mark>playlist</mark> parameter specifies an array of YouTube video IDs. In the YouTube 
Data API, the video resource's id property specifies a video ID.</p>

<p>The optional <mark>index</mark> parameter specifies the index of the first video in the playlist that 
will play. The parameter uses a zero-based index, and the default parameter value is 0, 
so the default behavior is to load and play the first video in the playlist.</p>

<p>The optional <mark>startSeconds</mark> parameter accepts a float/integer and specifies the time from 
which the first video in the playlist should start playing.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Object syntax</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>
player.loadPlaylist({list:String,
                     listType:String,
                     index:Number,
                     startSeconds:Number}):Void
</pre>

<p>This function loads the specified list and plays it. The list can be a <mark>playlist</mark> or a user's 
uploaded videos feed. The ability to load a list of search results is deprecated and will 
no longer be supported as of 15 November 2020.</p>

<p>The optional listType property specifies the type of results feed that you are retrieving. 
Valid values are playlist and user_uploads. A deprecated value, search, will no longer be 
supported as of 15 November 2020. The default value is playlist.</p>

<p>The required list property contains a key that identifies the particular list of videos 
that YouTube should return.</p>

<p>If the listType property value is playlist, then the list property specifies a playlist 
ID or an array of video IDs. In the YouTube Data API, the playlist resource's id property 
specifies a playlist's ID, and the video resource's id property specifies a video ID.</p>

<p>If the listType property value is user_uploads, then the list property identifies the user 
whose uploaded videos will be returned.</p>

<p>If the listType property value is search, then the list property specifies the search query. 
Note: This functionality is deprecated and will no longer be supported as of 15 November 2020.</p>

<p>The optional index property specifies the index of the first video in the list that will 
play. The parameter uses a zero-based index, and the default parameter value is 0, so the 
default behavior is to load and play the first video in the list.</p>

<p>The optional startSeconds property accepts a float/integer and specifies the time from 
which the first video in the list should start playing.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Playback controls and player settings</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Playing a video</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.playVideo():Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Plays the currently cued/loaded video. The final player state after this function 
executes will be playing (1).</p>

<p>Note: A playback only counts toward a video's official view count if it is initiated via 
a native play button in the player.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.pauseVideo():Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Pauses the currently playing video. The final player state after this function executes 
will be paused (2) unless the player is in the ended (0) state when the function is 
called, in which case the player state will not change.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.stopVideo():Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Stops and cancels loading of the current video. This function should be reserved for rare 
situations when you know that the user will not be watching additional video in the 
player. If your intent is to pause the video, you should just call the pauseVideo 
function. If you want to change the video that the player is playing, you can call one of 
the queueing functions without calling stopVideo first.</p>

<p>Important: Unlike the pauseVideo function, which leaves the player in the paused (2) 
state, the stopVideo function could put the player into any not-playing state, including 
ended (0), paused (2), video cued (5) or unstarted (-1).</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.seekTo(seconds:Number, allowSeekAhead:Boolean):Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Seeks to a specified time in the video. If the player is paused when the function is 
called, it will remain paused. If the function is called from another state (playing, 
video cued, etc.), the player will play the video.</p>

<p>The seconds parameter identifies the time to which the player should advance.</p>

<p>The player will advance to the closest keyframe before that time unless the player has 
already downloaded the portion of the video to which the user is seeking.</p>

<p>The allowSeekAhead parameter determines whether the player will make a new request to 
the server if the seconds parameter specifies a time outside of the currently buffered 
video data.</p>

<p>We recommend that you set this parameter to false while the user drags the mouse along 
a video progress bar and then set it to true when the user releases the mouse. This 
approach lets a user scroll to different points of a video without requesting new video 
streams by scrolling past unbuffered points in the video. When the user releases the 
mouse button, the player advances to the desired point in the video and requests a new 
video stream if necessary.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Controlling playback of 360° videos</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Note: The 360° video playback experience has limited support on mobile devices. On 
unsupported devices, 360° videos appear distorted and there is no supported way to change 
the viewing perspective at all, including through the API, using orientation sensors, or 
responding to touch/drag actions on the device's screen.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getSphericalProperties():Object</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Retrieves properties that describe the viewer's current perspective, or view, for a 
video playback. In addition:</p>

<p>This object is only populated for 360° videos, which are also called spherical videos.
If the current video is not a 360° video or if the function is called from a 
non-supported device, then the function returns an empty object.</p>

<p>On supported mobile devices, if the enableOrientationSensor property is set to true, then 
this function returns an object in which the fov property contains the correct value and 
the other properties are set to 0.</p>

<p>The object contains the following properties:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Properties</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<ul>
  <li><mark>yaw</mark> - A number in the range [0, 360) that represents the horizontal angle 
    of the view in degrees, which reflects the extent to which the user turns the view to face 
    further left or right. The neutral position, facing the center of the video in its 
    equirectangular projection, represents 0°, and this value increases as the viewer turns left.</li>
  <li><mark>pitch</mark> - A number in the range [-90, 90] that represents the vertical 
    angle of the view in degrees, which reflects the extent to which the user adjusts the 
    view to look up or down. The neutral position, facing the center of the video in its 
    equirectangular projection, represents 0°, and this value increases as the viewer looks 
    up.</li>
  <li><mark>roll</mark>	A number in the range [-180, 180] that represents the clockwise or 
    counterclockwise rotational angle of the view in degrees. The neutral position, with the 
	horizontal axis in the equirectangular projection being parallel to the horizontal axis 
	of the view, represents 0°. The value increases as the view rotates clockwise and decreases 
	as the view rotates counterclockwise.</li>
</ul>

<p>Note that the embedded player does not present a user interface for adjusting the roll of 
the view. The roll can be adjusted in either of these mutually exclusive ways:
Use the orientation sensor in a mobile browser to provide roll for the view. If the 
orientation sensor is enabled, then the getSphericalProperties function always returns 0 
as the value of the roll property.</p>

<p>If the orientation sensor is disabled, set the roll to a nonzero value using this API.
fov	A number in the range [30, 120] that represents the field-of-view of the view in 
degrees as measured along the longer edge of the viewport. The shorter edge is automatically 
adjusted to be proportional to the aspect ratio of the view.</p>

<p>The default value is 100 degrees. Decreasing the value is like zooming in on the video 
content, and increasing the value is like zooming out. This value can be adjusted either 
by using the API or by using the mousewheel when the video is in fullscreen mode.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.setSphericalProperties(properties:Object):Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Sets the video orientation for playback of a 360° video. (If the current video is not 
spherical, the method is a no-op regardless of the input.)</p>

<p>The player view responds to calls to this method by updating to reflect the values of 
any known properties in the properties object. The view persists values for any other 
known properties not included in that object.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>In addition:</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>If the object contains unknown and/or unexpected properties, the player ignores them.
As noted at the beginning of this section, the 360° video playback experience is not 
supported on all mobile devices.</p>

<p>By default, on supported mobile devices, this function sets only sets the fov property 
and does not affect the yaw, pitch, and roll properties for 360° video playbacks. See 
the enableOrientationSensor property below for more detail.</p>

<p>The properties object passed to the function contains the following properties:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Properties</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<ul>
  <li><b>yaw</b> - See definition above.</li>
  <li><b>pitch</b> - See definition above.</li>
  <li><b>roll</b> - See definition above.</li>
  <li><b>fov</b> - See definition above.</li>
</ul>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>enableOrientationSensor</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Note: This property affects the 360° viewing experience on supported devices only.
A boolean value that indicates whether the IFrame embed should respond to events that 
signal changes in a supported device's orientation, such as a mobile browser's 
<mark>DeviceOrientationEvent</mark>. The default parameter value is true.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Supported mobile devices</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>When the value is true, an embedded player relies only on the device's movement to adjust 
the yaw, pitch, and roll properties for 360° video playbacks. However, the fov property 
can still be changed via the API, and the API is, in fact, the only way to change the fov 
property on a mobile device. This is the default behavior.</p>

<p>When the value is false, then the device's movement does not affect the 360° viewing 
experience, and the yaw, pitch, roll, and fov properties must all be set via the API.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Unsupported mobile devices</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The <mark>enableOrientationSensor</mark> property value does not have any effect on the playback experience.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Playing a video in a playlist</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.nextVideo():Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function loads and plays the next video in the playlist.</p>

<p>If <mark>player.nextVideo()</mark> is called while the last video in the playlist is being watched, and 
the playlist is set to play continuously (loop), then the player will load and play the 
first video in the list.</p>

<p>If <mark>player.nextVideo()</mark> is called while the last video in the playlist is being watched, and 
the playlist is not set to play continuously, then playback will end.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.previousVideo():Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function loads and plays the previous video in the playlist.</p>

<p>If <mark>player.previousVideo()</mark> is called while the first video in the playlist is being 
watched, and the playlist is set to play continuously (loop), then the player will 
load and play the last video in the list.</p>

<p>If <mark>player.previousVideo()</mark> is called while the first video in the playlist is being 
watched, and the playlist is not set to play continuously, then the player will 
restart the first playlist video from the beginning.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.playVideoAt(index:Number):Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function loads and plays the specified video in the playlist.</p>

<p>The required index parameter specifies the index of the video that you want to play in 
the playlist. The parameter uses a zero-based index, so a value of 0 identifies the 
first video in the list. If you have shuffled the playlist, this function will play the 
video at the specified position in the shuffled playlist.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.mute():Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Mutes the player.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.unMute():Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Unmutes the player.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.isMuted():Boolean</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Returns true if the player is muted, false if not.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Changing the player volume</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.setVolume(volume:Number):Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Sets the volume. Accepts an integer between 0 and 100.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getVolume():Number</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Returns the player's current volume, an integer between 0 and 100. Note that <b>getVolume()</b> 
will return the volume even if the player is muted.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Setting the player size</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.setSize(width:Number, height:Number):Object</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Sets the size in pixels of the &lt;iframe&gt; that contains the player.
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Setting the playback rate</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getPlaybackRate():Number</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function retrieves the playback rate of the currently playing video. The default 
playback rate is 1, which indicates that the video is playing at normal speed. Playback 
rates may include values like 0.25, 0.5, 1, 1.5, and 2.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.setPlaybackRate(suggestedRate:Number):Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function sets the suggested playback rate for the current video. If the playback 
rate changes, it will only change for the video that is already cued or being played. 
If you set the playback rate for a cued video, that rate will still be in effect when 
the playVideo function is called or the user initiates playback directly through the 
player controls. In addition, calling functions to cue or load videos or playlists 
(cueVideoById, loadVideoById, etc.) will reset the playback rate to 1.</p>

<p>Calling this function does not guarantee that the playback rate will actually change. 
However, if the playback rate does change, the onPlaybackRateChange event will fire, 
and your code should respond to the event rather than the fact that it called the 
setPlaybackRate function.</p>

<p>The getAvailablePlaybackRates method will return the possible playback rates for the 
currently playing video. However, if you set the suggestedRate parameter to a 
non-supported integer or float value, the player will round that value down to the 
nearest supported value in the direction of 1.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getAvailablePlaybackRates():Array</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function returns the set of playback rates in which the current video is available. 
The default value is 1, which indicates that the video is playing in normal speed.</p>

<p>The function returns an array of numbers ordered from slowest to fastest playback speed. 
Even if the player does not support variable playback speeds, the array should always 
contain at least one value (1).</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Setting playback behavior for playlists</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.setLoop(loopPlaylists:Boolean):Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function indicates whether the video player should continuously play a playlist 
or if it should stop playing after the last video in the playlist ends. The default 
behavior is that playlists do not loop.</p>

<p>This setting will persist even if you load or cue a different playlist, which means 
that if you load a playlist, call the setLoop function with a value of true, and then 
load a second playlist, the second playlist will also loop.</p>

<p>The required loopPlaylists parameter identifies the looping behavior.</p>

<p>If the parameter value is true, then the video player will continuously play playlists. 
After playing the last video in a playlist, the video player will go back to the 
beginning of the playlist and play it again.</p>

<p>If the parameter value is false, then playbacks will end after the video player plays 
the last video in a playlist.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.setShuffle(shufflePlaylist:Boolean):Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function indicates whether a playlist's videos should be shuffled so that they play 
back in an order different from the one that the playlist creator designated. If you 
shuffle a playlist after it has already started playing, the list will be reordered 
while the video that is playing continues to play. The next video that plays will 
then be selected based on the reordered list.</p>

<p>This setting will not persist if you load or cue a different playlist, which means 
that if you load a playlist, call the setShuffle function, and then load a second 
playlist, the second playlist will not be shuffled.</p>

<p>The required shufflePlaylist parameter indicates whether YouTube should shuffle the 
playlist.</p>

<p>If the parameter value is true, then YouTube will shuffle the playlist order. If you 
instruct the function to shuffle a playlist that has already been shuffled, YouTube 
will shuffle the order again.</p>

<p>If the parameter value is false, then YouTube will change the playlist order back to 
its original order.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Playback status</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getVideoLoadedFraction():Float</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Returns a number between 0 and 1 that specifies the percentage of the video that the 
player shows as buffered. This method returns a more reliable number than the 
now-deprecated getVideoBytesLoaded and getVideoBytesTotal methods.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getPlayerState():Number</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Returns the state of the player. Possible values are:</p>
<ul>
  <li><b>-1 – unstarted</b></li>
  <li><b>0 – ended</b></li>
  <li><b>1 – playing</b></li>
  <li><b>2 – paused</b></li>
  <li><b>3 – buffering</b></li>
  <li><b>5 – video cued</b></li>
</ul>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getCurrentTime():Number</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Returns the elapsed time in seconds since the video started playing.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getVideoStartBytes():Number</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Deprecated as of October 31, 2012. Returns the number of bytes the video file started 
loading from. (This method now always returns a value of 0.) Example scenario: the user 
seeks ahead to a point that hasn't loaded yet, and the player makes a new request to 
play a segment of the video that hasn't loaded yet.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getVideoLoadFraction</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Determines the percentage of the video that has buffered.</p>

<p>This method returns a value between 0 and 1000 that approximates the amount of the 
video that has been loaded. You could calculate the fraction of the video that has 
been loaded by dividing the getVideoBytesLoaded value by the getVideoBytesTotal value.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getVideoLoadFraction</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Determine the percentage of the video that has buffered.</p>

<p>Returns the size in bytes of the currently loaded/playing video or an approximation 
of the video's size.</p>

<p>This method always returns a value of 1000. You could calculate the fraction of the 
video that has been loaded by dividing the getVideoBytesLoaded value by the 
getVideoBytesTotal value.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Retrieving video information</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getDuration():Number</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Returns the duration in seconds of the currently playing video. Note that getDuration() 
will return 0 until the video's metadata is loaded, which normally happens just after 
the video starts playing.</p>

<p>If the currently playing video is a live event, the getDuration() function will return 
the elapsed time since the live video stream began. Specifically, this is the amount of 
time that the video has streamed without being reset or interrupted. In addition, this 
duration is commonly longer than the actual event time since streaming may begin before 
the event's start time.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getVideoUrl():String</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Returns the YouTube.com URL for the currently loaded/playing video.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getVideoEmbedCode():String</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Returns the embed code for the currently loaded/playing video.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Retrieving playlist information</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getPlaylist():Array</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function returns an array of the video IDs in the playlist as they are currently 
ordered. By default, this function will return video IDs in the order designated by 
the playlist owner. However, if you have called the setShuffle function to shuffle the 
playlist order, then the getPlaylist() function's return value will reflect the shuffled 
order.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getPlaylistIndex():Number</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This function returns the index of the playlist video that is currently playing.
If you have not shuffled the playlist, the return value will identify the position 
where the playlist creator placed the video. The return value uses a zero-based index, 
so a value of 0 identifies the first video in the playlist.</p>

<p>If you have shuffled the playlist, the return value will identify the video's order 
within the shuffled playlist.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Adding or removing an event listener</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.addEventListener(event:String, listener:String):Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Adds a listener function for the specified event. The Events section below identifies the 
different events that the player might fire. The listener is a string that specifies the 
function that will execute when the specified event fires.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.removeEventListener(event:String, listener:String):Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Removes a listener function for the specified event. The listener is a string that 
identifies the function that will no longer execute when the specified event fires.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Accessing and modifying DOM nodes</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getIframe():Object</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This method returns the DOM node for the embedded &lt;iframe&gt;.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.destroy():Void</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Removes the &lt;iframe&gt; containing the player.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Events</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The API fires events to notify your application of changes to the embedded player. As 
noted in the previous section, you can subscribe to events by adding an event listener 
when constructing the YT.Player object, and you can also use the addEventListener 
function.</p>

<p>The API will pass an event object as the sole argument to each of those functions. The 
event object has the following properties:</p>

<p>The event's target identifies the video player that corresponds to the event.
The event's data specifies a value relevant to the event. Note that the onReady and 
onAutoplayBlocked events do not specify a data property.</p>

<p>The following list defines the events that the API fires:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>onReady</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This event fires whenever a player has finished loading and is ready to begin receiving 
API calls. Your application should implement this function if you want to automatically 
execute certain operations, such as playing the video or displaying information about 
the video, as soon as the player is ready.</p>

<p>The example below shows a sample function for handling this event. The event object 
that the API passes to the function has a target property, which identifies the player.</p>

<p>The function retrieves the embed code for the currently loaded video, starts to play 
the video, and displays the embed code in the page element that has an id value of 
embed-code.</p>
<pre>
function onPlayerReady(event) {
  var embedCode = event.target.getVideoEmbedCode();
  event.target.playVideo();
  if (document.getElementById('embed-code')) {
    document.getElementById('embed-code').innerHTML = embedCode;
  }
}
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>onStateChange</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This event fires whenever the player's state changes. The data property of the event 
object that the API passes to your event listener function will specify an integer 
that corresponds to the new player state. Possible values are:</p>
<ul>
  <li><b>-1 (unstarted)</b></li>
  <li><b>0 (ended)</b></li>
  <li><b>1 (playing)</b></li>
  <li><b>2 (paused)</b></li>
  <li><b>3 (buffering)</b></li>
  <li><b>5 (video cued).</b></li>
</ul>

<p>When the player first loads a video, it will broadcast an unstarted (-1) event. When a 
video is cued and ready to play, the player will broadcast a video cued (5) event. In 
your code, you can specify the integer values or you can use one of the following 
namespaced variables:</p>
<ul>
  <li><b>YT.PlayerState.ENDED</b></li>
  <li><b>YT.PlayerState.PLAYING</b></li>
  <li><b>YT.PlayerState.PAUSED</b></li>
  <li><b>YT.PlayerState.BUFFERING</b></li>
  <li><b>YT.PlayerState.CUED</b></li>
  <li><b>onPlaybackQualityChange</b></li>
</ul>

<p>This event fires whenever the video playback quality changes. It might signal a change 
in the viewer's playback environment. See the <a href="">YouTube Help Center</a> for more information 
about factors that affect playback conditions or that might cause the event to fire.</p>

<p>The data property value of the event object that the API passes to the event listener 
function will be a string that identifies the new playback quality. Possible values are:</p>

<ul>
  <li><b>small</b></li>
  <li><b>medium</b></li>
  <li><b>large</b></li>
  <li><b>hd720</b></li>
  <li><b>hd1080</b></li>
  <li><b>highres</b></li>
</ul>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>onPlaybackRateChange</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This event fires whenever the video playback rate changes. For example, if you call the 
setPlaybackRate(suggestedRate) function, this event will fire if the playback rate 
actually changes. Your application should respond to the event and should not assume 
that the playback rate will automatically change when the setPlaybackRate(suggestedRate) 
function is called. Similarly, your code should not assume that the video playback rate 
will only change as a result of an explicit call to setPlaybackRate.</p>

<p>The data property value of the event object that the API passes to the event listener 
function will be a number that identifies the new playback rate. The 
getAvailablePlaybackRates method returns a list of the valid playback rates for the 
currently cued or playing video.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>onError</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This event fires if an error occurs in the player. The API will pass an event object to 
the event listener function. That object's data property will specify an integer that 
identifies the type of error that occurred. Possible values are:</p>
<ul>
  <li><b>2</b> – The request contains an invalid parameter value. For example, this error occurs 
    if you specify a video ID that does not have 11 characters, or if the video ID contains 
	invalid characters, such as exclamation points or asterisks.</li>
  <li><b>5 – The requested content cannot be played in an HTML5 player or another error related 
    to the HTML5 player has occurred.</li>
  <li><b>100 – The video requested was not found. This error occurs when a video has been removed 
    (for any reason) or has been marked as private.</li>
  <li><b>101 – The owner of the requested video does not allow it to be played in embedded 
    players.</li>
  <li><b>150 – This error is the same as 101. It's just a 101 error in disguise!</li>
</ul>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>onApiChange</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This event is fired to indicate that the player has loaded (or unloaded) a module with 
exposed API methods. Your application can listen for this event and then poll the player 
to determine which options are exposed for the recently loaded module. Your application 
can then retrieve or update the existing settings for those options.</p>

<p>The following command retrieves an array of module names for which you can set player 
options:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getOptions();</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Currently, the only module that you can set options for is the captions module, which 
handles closed captioning in the player. Upon receiving an onApiChange event, your 
application can use the following command to determine which options can be set for the 
captions module:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getOptions('captions');</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>By polling the player with this command, you can confirm that the options you want to 
access are, indeed, accessible. The following commands retrieve and update module options:</p>

<p>Retrieving an option:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.getOption(module, option);</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Setting an option</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>player.setOption(module, option, value);</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The table below lists the options that the API supports:</p>

<dl>
  <dt>Module</dt>
  <dd>captions</dd>
  <dt>Option</dt>
  <dd>fontSize</dd>
  <dt>Description</dt>
  <dd>This option adjusts the font size of the captions displayed in the player.</dd>
</dl>

<p>Valid values are -1, 0, 1, 2, and 3. The default size is 0, and the smallest size is -1. 
Setting this option to an integer below -1 will cause the smallest caption size to display, 
while setting this option to an integer above 3 will cause the largest caption size to display.
captions	reload	This option reloads the closed caption data for the video that is playing. 
The value will be null if you retrieve the option's value. Set the value to true to reload 
the closed caption data.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
onAutoplayBlocked
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This event fires any time the browser blocks autoplay or scripted video playback features, 
collectively referred to as "autoplay". This includes playback attempted with any of the 
following player APIs:</p>

<ul>
  <li>autoplay parameter</li>
  <li>loadPlaylist function</li>
  <li>loadVideoById function</li>
  <li>loadVideoByUrl function</li>
  <li>playVideo function</li>
</ul>

<p>Most browsers have policies that can block autoplay in desktop, mobile, and other 
environments if certain conditions are true. Instances where this policy may be 
triggered include unmuted playback without user interaction, or when a Permissions 
Policy to permit autoplay on a cross-origin iframe has not been set.</p>

<p>For complete details, refer to browser-specific policies (Apple Safari / Webkit, Google 
Chrome, Mozilla Firefox) and Mozilla's autoplay guide.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Examples</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Creating YT.Player objects</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Example 1: Use API with existing &lt;iframe&gt;</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>In this example, an <b>&lt;iframe&gt;</b> element on the page already defines the player with which the 
API will be used. Note that either the player's src URL must set the <mark>enablejsapi</mark> parameter 
to 1 or the <b>&lt;iframe&gt;</b> element's <mark>enablejsapi</mark> attribute must be set to true.</p>

<p>The <mark>onPlayerReady</mark> function changes the color of the border around the player to orange when 
the player is ready. The onPlayerStateChange function then changes the color of the border 
around the player based on the current player status. For example, the color is green when 
the player is playing, red when paused, blue when buffering, and so forth.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>&lt;!-- video here --&gt;</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This example uses the following code:</pre>

<details>
  <summary>Controlling 360° videos</summary>

<pre>
&lt;iframe id="existing-iframe-example"
        width="640" height="360"
        src="https://www.youtube.com/embed/M7lc1UVf-VE?enablejsapi=1"
        frameborder="0"
        style="border: solid 4px #37474F"&gt;
&lt;/iframe&gt;

&lt;script type="text/javascript"&gt;
  var tag = document.createElement('script');
  tag.id = 'iframe-demo';
  tag.src = 'https://www.youtube.com/iframe_api';
  var firstScriptTag = document.getElementsByTagName('script')[0];
  firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

  var player;
  function onYouTubeIframeAPIReady() {
    player = new YT.Player('existing-iframe-example', {
        events: {
          'onReady': onPlayerReady,
          'onStateChange': onPlayerStateChange
        }
    });
  }
  function onPlayerReady(event) {
    document.getElementById('existing-iframe-example').style.borderColor = '#FF6D00';
  }
  function changeBorderColor(playerStatus) {
    var color;
    if (playerStatus == -1) {
      color = "#37474F"; // unstarted = gray
    } else if (playerStatus == 0) {
      color = "#FFFF00"; // ended = yellow
    } else if (playerStatus == 1) {
      color = "#33691E"; // playing = green
    } else if (playerStatus == 2) {
      color = "#DD2C00"; // paused = red
    } else if (playerStatus == 3) {
      color = "#AA00FF"; // buffering = purple
    } else if (playerStatus == 5) {
      color = "#FF6DOO"; // video cued = orange
    }
    if (color) {
      document.getElementById('existing-iframe-example').style.borderColor = color;
    }
  }
  function onPlayerStateChange(event) {
    changeBorderColor(event.data);
  }
&lt;/script&gt;
</pre>
</details>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Example 2: Loud playback</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This example creates a 1280px by 720px video player. The event listener for the onReady 
event then calls the setVolume function to adjust the volume to the highest setting.</p>

<details>
  <summary>Controlling 360° videos</summary>

<pre>
function onYouTubeIframeAPIReady() {
  var player;
  player = new YT.Player('player', {
    width: 1280,
    height: 720,
    videoId: 'M7lc1UVf-VE',
    events: {
      'onReady': onPlayerReady,
      'onStateChange': onPlayerStateChange,
      'onError': onPlayerError
    }
  });
}

function onPlayerReady(event) {
  event.target.setVolume(100);
  event.target.playVideo();
}
</pre>

</details>

<p><b>Example 3</b>: This example sets player parameters to automatically play the video when it 
loads and to hide the video player's controls. It also adds event listeners for several 
events that the API broadcasts.</p>

<pre>
function onYouTubeIframeAPIReady() {
  var player;
  player = new YT.Player('player', {
    videoId: 'M7lc1UVf-VE',
    playerVars: { 'autoplay': 1, 'controls': 0 },
    events: {
      'onReady': onPlayerReady,
      'onStateChange': onPlayerStateChange,
      'onError': onPlayerError
    }
  });
}
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Controlling 360° videos</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<pre>&lt;!-- video here https://youtu.be/FAtdv94yzp4 --&gt;</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>This example uses the following code:</p>

<details>
  <summary>Controlling 360° videos</summary>

<pre>
&lt;style&gt;
  .current-values {
    color: #666;
    font-size: 12px;
  }
&lt;/style&gt;
&lt;!-- The player is inserted in the following div element --&gt;
&lt;div id="spherical-video-player"&gt;&lt;/div&gt;

&lt;!-- Display spherical property values and enable user to update them. --&gt;
&lt;table style="border: 0; width: 640px;"&gt;
  &lt;tr style="background: #fff;"&gt;
    &lt;td&gt;
      &lt;label for="yaw-property"&gt;yaw: &lt;/label&gt;
      &lt;input type="text" id="yaw-property" style="width: 80px"&gt;&lt;br&gt;
      &lt;div id="yaw-current-value" class="current-values"&gt; &lt;/div&gt;
    &lt;/td&gt;
    &lt;td&gt;
      &lt;label for="pitch-property"&gt;pitch: &lt;/label&gt;
      &lt;input type="text" id="pitch-property" style="width: 80px"&gt;&lt;br&gt;
      &lt;div id="pitch-current-value" class="current-values"&gt; &lt;/div&gt;
    &lt;/td&gt;
    &lt;td&gt;
      &lt;label for="roll-property"&gt;roll: &lt;/label&gt;
      &lt;input type="text" id="roll-property" style="width: 80px"&gt;&lt;br&gt;
      &lt;div id="roll-current-value" class="current-values"&gt; &lt;/div&gt;
    &lt;/td&gt;
    &lt;td&gt;
      &lt;label for="fov-property"&gt;fov: &lt;/label&gt;
      &lt;input type="text" id="fov-property" style="width: 80px"&gt;&lt;br&gt;
      &lt;div id="fov-current-value" class="current-values"&gt; &lt;/div&gt;
    &lt;/td&gt;
    &lt;td style="vertical-align: bottom;"&gt;
      &lt;button id="spherical-properties-button"&gt;Update properties&lt;/button&gt;
    &lt;/td&gt;
  &lt;/tr&gt;
&lt;/table&gt;

&lt;script type="text/javascript"&gt;
  var tag = document.createElement('script');
  tag.id = 'iframe-demo';
  tag.src = 'https://www.youtube.com/iframe_api';
  var firstScriptTag = document.getElementsByTagName('script')[0];
  firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);

  var PROPERTIES = ['yaw', 'pitch', 'roll', 'fov'];
  var updateButton = document.getElementById('spherical-properties-button');

  // Create the YouTube Player.
  var ytplayer;
  function onYouTubeIframeAPIReady() {
    ytplayer = new YT.Player('spherical-video-player', {
        height: '360',
        width: '640',
        videoId: 'FAtdv94yzp4',
    });
  }

  // Don't display current spherical settings because there aren't any.
  function hideCurrentSettings() {
    for (var p = 0; p &lt; PROPERTIES.length; p++) {
      document.getElementById(PROPERTIES[p] + '-current-value').innerHTML = '';
    }
  }

  // Retrieve current spherical property values from the API and display them.
  function updateSetting() {
    if (!ytplayer || !ytplayer.getSphericalProperties) {
      hideCurrentSettings();
    } else {
      let newSettings = ytplayer.getSphericalProperties();
      if (Object.keys(newSettings).length === 0) {
        hideCurrentSettings();
      } else {
        for (var p = 0; p &lt; PROPERTIES.length; p++) {
          if (newSettings.hasOwnProperty(PROPERTIES[p])) {
            currentValueNode = document.getElementById(PROPERTIES[p] +
                                                       '-current-value');
            currentValueNode.innerHTML = ('current: ' +
                newSettings[PROPERTIES[p]].toFixed(4));
          }
        }
      }
    }
    requestAnimationFrame(updateSetting);
  }
  updateSetting();

  // Call the API to update spherical property values.
  updateButton.onclick = function() {
    var sphericalProperties = {};
    for (var p = 0; p &lt; PROPERTIES.length; p++) {
      var propertyInput = document.getElementById(PROPERTIES[p] + '-property');
      sphericalProperties[PROPERTIES[p]] = parseFloat(propertyInput.value);
    }
    ytplayer.setSphericalProperties(sphericalProperties);
  }
&lt;/script&gt;
</pre>
</detail>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Android WebView Media Integrity API integration</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>YouTube has extended the Android WebView Media Integrity API to enable embedded media 
players, including YouTube player embeds in Android applications, to verify the 
embedding app's authenticity. With this change, embedding apps automatically send an 
attested app ID to YouTube. The data collected through usage of this API is the app 
metadata (the package name, version number, and signing certificate) and a device 
attestation token generated by Google Play services.</p>

<p>The data is used to verify the application and device integrity. It is encrypted, 
not shared with third parties, and deleted following a fixed retention period. App 
developers can configure their app identity in the WebView Media Integrity API. The 
configuration supports an opt-out option.</p>

<p>The <b>getSphericalProperties</b> function retrieves the current orientation for the video 
playback. The orientation includes the following data:</p>
<ul>
  <li><b>yaw</b> - represents the horizontal angle of the view in degrees, which reflects the 
extent to which the user turns the view to face further left or right.</li>
  <li><b>pitch</b> - represents the vertical angle of the view in degrees, which reflects the 
extent to which the user adjusts the view to look up or down.</li>
  <li><b>roll</b> - represents the rotational angle (clockwise or counterclockwise) of the view in 
degrees.</li>
  <li><b>fov</b> - represents the field-of-view of the view in degrees, which reflects the extent to 
which the user zooms in or out on the video.</li>
</ul>

<p>The setSphericalProperties function modifies the view to match the submitted property 
values. In addition to the orientation values described above, this function supports 
a Boolean field that indicates whether the IFrame embed should respond to 
DeviceOrientationEvents on supported mobile devices.</p>

<p>This example demonstrates and lets you test these new features.</p>

<p>Documentation for the YouTube Flash Player API and YouTube JavaScript Player API has been 
removed and redirected to this document. The deprecation announcement for the Flash and 
JavaScript players was made on January 27, 2015. If you haven't done so already, please 
migrate your applications to use IFrame embeds and the IFrame Player API.</p>

<p>The new removeEventListener function lets you remove a listener for a specified event.</p>

<p>The API supports several new functions and one new event that can be used to control 
the video playback speed:</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Functions</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<ul>
  <li><b>getAvailablePlaybackRates</b> – Retrieve the supported playback rates for the cued or 
	  playing video. Note that variable playback rates are currently only supported in 
	  the HTML5 player.</li>
  <li><b>getPlaybackRate</b> – Retrieve the playback rate for the cued or playing video.</li>
  <li><b>setPlaybackRate</b> – Set the playback rate for the cued or playing video.</li>
</ul>	
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h4>Events</h4>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<ul>
  <li><b>onPlaybackRateChange</b> – This event fires when the video's playback rate changes.</li>
  <li>The new <b>getVideoLoadedFraction</b> method replaces the now-deprecated getVideoBytesLoaded 
	  and getVideoBytesTotal methods. The new method returns the percentage of the video that 
	  the player shows as buffered.</li>
  <li>The <b>onError</b> event may now return an error code of 5, which indicates that the requested 
	  content cannot be played in an HTML5 player or another error related to the HTML5 player 
	  has occurred.</li>
</ul>

<p>The Requirements section has been updated to indicate that any web page using the IFrame 
API must also implement the onYouTubeIframeAPIReady function. Previously, the section 
indicated that the required function was named onYouTubePlayerAPIReady. Code samples 
throughout the document have also been updated to use the new name.</p>

<p>Note: To ensure that this change does not break existing implementations, both names 
will work. If, for some reason, your page has an onYouTubeIframeAPIReady function and 
an onYouTubePlayerAPIReady function, both functions will be called, and the 
onYouTubeIframeAPIReady function will be called first.</p>

<p>The code sample in the Getting started section has been updated to reflect that the 
URL for the IFrame Player API code has changed to http://www.youtube.com/iframe_api.</p>

<p>To ensure that this change does not affect existing implementations, the old URL 
(http://www.youtube.com/player_api) will continue to work.</p>

