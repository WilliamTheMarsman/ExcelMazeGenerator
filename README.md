# ExcelMazeGenerator
This is a maze generator made in excel. A fixed square area is defined by the user as well as a variable number of maze paths. These maze paths then expand inside the area, competing for space, using either the longest path or random path maze generation algorithm. The cool part of this macro is the colorizer, which takes the depth of a block when it is placed in a path and applies a sine function to it to obtain a color. This color is then applied to the cell. This creates a 'rainbow' effect as the maze path becomes longer. *An image of this can be seen inside the Example folder.*

In the latest revision, this color can also be set to 'spill' out of the selected path cell and modify any cellblocks not yet part of a path which surround it, mixing the target cells color with the color of the new path cell. This effect is probably the most interesting to view generating live. *An image of this can be seen inside the Example folder.*

## Using This Workbook

The workbook has three main worksheets: Maze Sheet, Maze Settings and Special Options. Maze Sheet contains the generated maze, Maze Settings contains the properties which define how the maze should generate and Special Options contains several special modifiers which change the output in unique ways.

### Maze Sheet

This is where the maze will be viewed after generation is finished. If **Disable Screen Updating** is set to false [Under Special Settings] this is also where the maze generation can be seen live.

### Maze Settings

The Maze Settings worksheet is the powerhouse behind this macro. **Parameters have mouseover comments** which explain what the more obtuse parameters mean and do. There are two main groups of settings; **Global Settings**  and **Individual Maze Properties**.

##### Global Settings

The global settings affect all of the maze paths during generation. They include:
- the position and size of the square in which mazes will be generated
- the number of maze paths that will be generated
- a random seed value, which is used by the macro to generate the maze
- a color period, which defines the rate at which the color changes from path depth

It is also possible to save specified setting by hitting the 'Save' button. This is a very low-tech save, and simply copies the contents of the sheet onto another sheet so that they could be copied back at a later point.

##### Individual Maze Properties

The Individual Maze Properties table contains the properties for each of the maze paths that will be generated. In the rows of this table are the maze paths, and each column contains a value which will modify how the maze path is generated.

- Path type; can either be 'long' or 'rand' (random)
- Initial X and Y; this is the position where the maze path will begin. This should be unique and less than the width and height of the maze
- Maze Bias; a bias of X% means the path will have a X% chance of growing longer on a given frame.

The remaining properties control how the Colorizer will be controlled. The colorizer creates three sinewaves and obtains a color using the color properties defined here. The general sine equation is shown below.
```
newcolor = RGB( _
        (1 - dis(1)) * Round(max(1) - rng(1) * Sin((2 * pi) * pos / prd(1)))), _
        (1 - dis(2)) * Round(max(2) - rng(2) * Sin((2 * pi) * pos / prd(2)) + 2 * pi / phs(2))), _
        (1 - dis(3)) * Round(max(3) - rng(3) * Sin((2 * pi) * pos / prd(3)) + 4 * pi / phs(3))) _
    )
```

It's pretty unreadable, so here's how it works:
- dis is Disable, and if set to one it disables that component of the RGB
- max is the max value defined in the settings sheet for that color
- rng is the range at which the sine function should be allowed to waver. 
- pos is the current position which is attempting to acquire a color
- prd defines the period of the sine, which is multiplied by 2 * pi to make it easy to select specific periods. A period of 4 will cycle through 4 colors.
- finally phs defines the phase and should typically be set to three, unless one of the RGB colors is disabled.

Several other options exist which control what properties are randomized when the 'Randomize Properties' button is pressed. This was included to keep paths which produced interesting output static while randomizing the remaining mazes.

When the settings have been configured as desired, pressing the Generate Maze button will create the maze.

##### Special Options

Several special configurations exist.
- Disable Screen Updating. This toggle causes the maze generation to be much faster, but you don't see it happening real time, you only get the output.
- Rainbow only. When this toggle is set, only rainbow style paths will be generated when 'Randomize Properties' is selected.
- Clear Previous Mazes. When this is on, all previously drawn mazes on the MazeSheet will be erased whenever a new maze is generated. Disabling this allows for a potential collage of generation.
- Add Delay. This slows down the rate of maze generation, as the process is typically too slow already you'll want this to be zero.
- Fill Mode. *Incomplete.* The idea was to pass a bitmap which would define where a maze could generate instead of simply using a user defined square. That way it would be possible to fill out pictures 'maze style'.
- Color Spill. When toggled, this causes color to spill out into surrounding blocks. Instead of a rigid maze you get a more convoluted output. The color is mixed with whatever color is there currently
- Color Spill Dim. Dims the color that is spilled, reducing the hue by multiplying by this number
- Color Spill Average. Defines how much of the previous color to use and how much of the new color to use when mixing the two together during the spill. A value of 0.5 uses fifty percent of the previous color with the new color. 

##### New! Bitmap Support!

This feature was one I had wanted to implement since I started this project. After I created this repo and read-me, I was drawn back into the project in order to complete this task.

A new file was included in the project. This file can take a bitmap image and translate it into cell by cell colors. This 'cell image' can then be moved into the newly created 'ImageMapping' sheet, and the maze generation will use any non-black squares to generate the maze, meaning you can fill out silouettes with the maze generation. I have included a new example image of this effect in the output folder. 

**To activate fill-mode, open the Special Options sheet and the hit the toggle for 'Fill Mode'** Then, convert a bitmap to cells using the other worksheet and then paste the image cells into the ImageMapping sheet. an example image is already there. Finally, in order to get this mode to work at the moment, **the width and height of the maze should be set to that of the width and height of the image. **

### Future Improvements
Although I only work on this every couple of months when inspiration strikes, there are a few things I wouldn't mind implementing:
- Integrated bitmap support, to remove the need for the extra worksheet
- save as an animated gif from inside the workbook
- infinite mode / multi-maze zone mode; continually randomly generate mazes in the output area to create a collage of mazes
- more interesting maze algorithms

#### A Little History...

I began work on this in the summer of 2014 after seeing some visually impressive maze generation stuff. I was also interested in learning VBA, and it seemed like a good sized project for me to work on to improve my programming skills in general. Looking back on it now, it's pretty messy but I definitely learned a lot from it. not only is it more or less finished, it also does something kind of cool, so I decided to include it here in my GitHub portfolio.

Thanks for taking the time to read about this project of mine, and I hope if you decide to try out the workbook you enjoy using it for a few minutes.
