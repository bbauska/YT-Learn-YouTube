# Embedding YouTube into your Responsive Website

Step #1. Get the YouTube Embed Code
Go to Youtube.
Find the video you want to use on your website.
Click on Share >> Embed >> Right click and copy the HTML code.

Step #2. Use the Embed Code
Paste this embed code in your website. It will look similar to this:

```
{codecitation}
<iframe width="420" height="315" src="http://www.youtube.com/embed/6xisazZX9bA" frameborder="0" allowfullscreen></iframe>
{/codecitation}
```

Step #3. Modify the Embed Code
Add a div tag around the embed code. Use the CSS class video-responsive so that your code now looks like this:

```
{codecitation}
<div class="video-responsive">
<iframe width="420" height="315" src="http://www.youtube.com/embed/6xisazZX9bA" frameborder="0" allowfullscreen></iframe>
</div>{/codecitation}
```

Step #4. Add the CSS
Now add some CSS properties inside one of your stylesheet files:

```
{codecitation}
.video-responsive{
overflow:hidden;
padding-bottom:56.25%;
position:relative;
height:0;
}
.video-responsive iframe{
left:0;
top:0;
height:100%;
width:100%;
position:absolute;
}{/codecitation}
```

Your videos now will be responsive. Try resizing your browser to see it in action.
