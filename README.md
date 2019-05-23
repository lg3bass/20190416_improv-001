## 20190416_improv-001

### HISTORY

- 20190523 README.md updated. Project archived


### LINKS

- [Instagram](https://www.instagram.com/p/BxXn3mfnliv/)
- [YouTube](https://www.youtube.com/watch?v=MUpxbmwwrE8)
- [Vimeo](https://vimeo.com/337434368)

### FILES

- 20190416_improv001 Project
	- Ableton Live Project including M4L objects
- CHAN
	- The original exported .chan files from M4L
- HOUDINI
	- All the Houdini project files including rendering and AE
- Samples
	- 20190416_improv001.mp3
	- 20190416_improv001.wav
- VIDEO
	- 1280x720_OF-version.mp4
	- 20190414_improv001-720p-2.jpg
	- 20190414_improv001-720p-2.mp4
	- 20190414_improv001-instagram.mp4
	- 20190416_improv001-OF-DVX.mov
- VMM
	- VMM xml files and project


### ISSUES

1. Rendering Time is too long. 
	- In order for this to work out we need a shorter rendering time.  I was averaging about 3m per frame.  It took me 4-5 days of rendering to finish this animation.   I need to find a way to get rendering under 1:30m per frame. For a 1800 frame animation (Instagram ready) that totals 45 hrs.  That is wau too long which means the real target is between 30s - 1m per frame. 15-30 hr.  That's about 2 days of rendering.  

2. Keyframe cleanup is a problem. 
	- I spent a lot of time cleaning and redoing keyframes.  I think the problem has to do with how the key are being written.  First I've seen regular instances where data has been dropped out.  Second converting all the samples to single keys was a problem even with an advanced interface like Houdini.    
	- One solution is to try to write keyframes from the M4L device.  This requires writing code to produce a .chn file right from M4L.  I would record the keyframes.. The problem is that the keys are in seconds where I need frames.  Figure if this is possible is a strong candidate for next development sprint.

3. Reduce shape complexity.
	- I spent a lot of time designing 3d elements.  A lot of this could be because I was unfamiliar with Houdini.   For the next rendition I need to simplify the 3d element designs.  General shapes need to be determined and only the refinement phase will I flesh these out. 
	
4. VMM has lost interest to me.
	- Just figureing out how to match coordinates in Houdini was a problem that took a long time.  It took forever to match the coordinates produced by VMM.  the Mandala patterns created by VMM were very much adhoc when I created them.  I did not have a full understanding of how to create the designs in Openframeworks.  
	- A much better approach would be to learn to create elements in Openframeworks attached to particle points and then try to duplicate the logic in Houdini using a point wrangle.


### IDEAS

1. Create a simulator type animation where elements are always moving toward the camera.  Once out of focus they dissappear. 
	- First step would be to start working on a new obj sequence addon.  I need a tool than VMM to make this work effectivly.  Once the element goes out of frustrum then it needs to be destroyed. 

2. Explore non-manadala designed animation.   Becuase Houdidi is best utilized when you have an underlying point coordinates system to attach elements to. Perhaps do a series using a grid.  Create an animation that breaks from the Mandala shapes in VMM.  
	- First Step in this exploration would be to manipulate a point cluster in OpenFrameworks and translate this to a pointWrangle in Houdini.
	
3. Create multi segmented animations like in VMM.  I need to port this feature into HOUDINI/M4L.  
	- First step: After you get the .chn keys written in M4L then work out a way to do multiple segments. 
	
4. Try to create an animation directly in Houdini to see how long it will take. 

### AUDIO

Location: /Users/lg3/BW_MCP/BW_PROJECTS/BW_PUBLISH/20190416_improv-001

This project has 3 scenes.  The first is the Demo used for the Houdini animation. The second is a section of a recorded jam I did when trying to write music. The 3rd is a groove I came up with while writing this piece.

Inside this Ableton Live project are the M4L objects you need to interact with VMM and VMMTimeline. 

### HOUDINI

FINAL Working file.  This file is commented with all the various components.  In the end I decided to use a regular animation curves.  To do so I imported the .chan file directly on to the animation channels.  But before I could do that I had to break the chan file up into a file with 4 channels each.  Then I could easily import to a null with a vec4. Once the chan file was imported I manually reduced the amount of keyframes using the animation editor.  Additionally I found that the animation did not time exactly as it looked.  I suspect that recording the animation from VMM incorporated a lag time. 

The original intention was to create digital assets for each of the geometry elements.  In this file I only used one DA that I had created previously.  I would still like to create the Digital Assets but It's fairly complicated to do so. I just dont want to take the time.  Also instead of playing back the original assets (that have to cook each frame) I decided to use a .bgeo sequeunce.  .bgeo is a lightweight geometry format that Houdini uses which is really fast.  It still lags in playback but it's is fast enough for me to tweak animation in Houdini.  Going forward using .bgeo files is a must.  If you want variation in the geometry then at rendertime you can swap back to using the cooked geo networks. 

Finally there are VMM track nodes which replicate the radial patterns in from the original openframeworks application. 1. I found that it doesn't look good to move the geo as it is animating.  Best to set a scene before the first animated object is drawn. Another observation is the rotate controls are hard to predict.  You pretty much need to play with settings till you get something you like.  I found myself deleting the numbers and starting over cause I couldn't mentally figure out the best way to correct a shape. 

The file was rendered with 1 light and a backdrop.  I initially wanted to get shadows.  This was costly in terms of rendering also I had trouble getting the shadows to look like they do in the lowrez OpenGL mode.   


FINAL FILE:
	20190416_improv-001.hiplc
	

Test to convert chan from the CHOP editor to an animation channel using exports.  This is a good method to remember but you can also just open the .chan file directly in the animation editor.   I suppose working in the CHOP editor could be more convienent if you need to work on all the channels at once or do some filtering.  This file is annotated with what I did.

	/TEST_houdini_files/chop2keyframes.hiplc

Test to load .chan files directly onto a NULL node in houdini.  I created a null and added 2 params of vector4f (8 channels).  You can import directly into the channels in the animation editor 4 at a time maximum this is why I broke up the original .chan files to files containing 4 channels each.  

	./TEST_houdini_fileschanImport2Null.hiplc
	


#### .chan files

The .chan files loose in this directory are:

Original .chan files from OF

	./track1.chan
	./track2.chan
	./track3.chan
	./track4.chan

These are the chan files that drive the animation.  I manually separated .chan files by breaking them up in Libre Office. Import these directly into the animation channels on a null in Houdini.  They are broken up into 4 each because I am loading them onto a vec4 in the animation editor.

	./track1_ch1-4.chan
	./track1_ch5-8.chan
	./track2_ch1-4.chan
	./track2_ch5-8.chan
	./track3_ch1-4.chan
	./track3_ch5-8.chan
	./track4_ch1-4.chan

Once the animation is imported you will want to clean it up in the animation editor.  This is a timeconsuming process but if you view the curves in the animation editor the right way you can select multiple keys and delete.  

Tips:
-- make sure the segments are set to linear()
-- turn off display of all handles.  turn off display of timing marks.  You just want the keys.  Turn off the value and frame indicators. 
-- maximize the content and scale vertically so you can select across.


#### /TEST_CHN

This is an example of a .chn file.  The .chn file format is a json file listing collections of segments.  

This file format is a better choice to convert animation from OF as you only have to write out the keyframes.  However you will need to covert the time from frames or seconds to a normalized value of 0-1 to measure each segment.  You will also have to draw segments in between important key frames. 

	test5.chn
	
	
	
### RENDERING
	
#### /render

This folder contains all the renders of the project. All in all the project took 4-5 days to render.   Very long.  This is the #1 issue I had with this project.   If any of this project is going to be feasible I will need to reduce the time it take to render by investing in a new machine with GPU rendering. 


/1_TESTS

This file was a test to push the sample boundaries.

	basicShapes_2.mantra1-high-12min.exr
	
/2_RENDER_TIME_TESTS

The RENDER_TIME_TESTS.xls spreadsheet lists a number of render tests I did where I recored the time.  The effort here was to determine the sweet spot where I can get good enough quality AND have the render time less than 1:30.   In reality some of the frames especially the ones with transparency took over 3min per frame. 

/3_RENDER

This is my final render file.  All of my .exr files were saved here.  

I will be removing these files fromt he public archives as they are over 7GB in size.  I will try to save them on an external drive. 


### GEOMETRY

#### /abc

I did a test using an .abc file instead of an .obj or .bgeo sequence.  The issue I had was that sometimes the .abc file would glitch out.  It would skip frames and display unpredictable results when previewing.  I decided not to use .abc

#### /geo

This folder contains all of the .bgeo sequences.  In the geometry nodes in Houdini you will see a rop output node that writes out all of the .bgeo sequences.

#### /obj

I just obj files for VMM so why not use them here.  Well, because they are slow when previewing.  .bgeo definitely are faster to work with in a preview situation.  When animating the shape position you need a sequence format that is fast in houdini.  


