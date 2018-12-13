## Intro

The **wasteboards configurator** is a simple tool that allows the user to 'paint' with plastic bottle caps. When the user is done painting, a preview mode can be activated to view a representation of the melting process Wasteboards uses to build their skateboard decks.

## Running the project
The project is made with the game engine **Construct 3** and requires a **Construct 3 license** to run, edit and build.
A construct 3 project file is a folder structure inside a .zip archive with the extension renamed to .c3p

The game engine opens these .c3p files. You'll find the .c3p in the project root.

### Building the project
Simply open the .c3p file and choose **Menu -> Project -> Export** to open the build menu.

Several build options are available, they all come down to different flavors of HTML5. Some have prefilled parameters for certain wrappers such as Cordova or Facebook Messenger for example.

For more info on the ins and outs of working with Construct, check out the [official documentation.](https://www.construct.net/en/make-games/manuals/construct-3)

## Project structure
The project uses a combination of:

 - layouts
 - event sheets
 - resources
 - system objects (you could see this as the Construct 3 API)

### Layouts
Layouts are Constructs way of storing scene data. A layout contains all the layers, ui objects, sprites etc you want to use. These layouts are editable in the Construct 3 IDE.

The project uses template layouts for each board deck. within these layouts you'll find a manually placed grid of caps. These caps can be placed in any kind of structure.

The pre-made layouts 'Squid' and 'Star' are aligned to resemble the standard Wasteboards decks. Additionally the layouts contain some misc graphics for masking purposes and displaying wheels etc.

In theory you could align the caps in any shape. The editor logic will stay the same because these are described in the **event sheets.**

## Event Sheets

Event sheets are the way Construct describes logic. It's a flavor of programming you might have to get used to. If you prefer to write Javascript. I recommend to use the [Construct 3 plugin SDK](https://www.construct.net/en/make-games/manuals/addon-sdk), which uses Javascript. 
Another way to use Javascript in C3 is to use the [Javascript Addon by Valery Popov](https://www.construct.net/en/make-games/addons/1/javascript).

Each layout can be assigned an event sheet. To use multiple event sheets in one layout you can 'include' several event sheets into one. I've opted to use this approach for the configurator. This way the logic is separated by functionality and keeps the whole thing readable and maintainable.

The project uses 5 event sheets:

 - Events-BoardBuilder
	 This is the 'main' event sheet and includes:
	 - Events-Touch (panning, zooming etc)
	 - Events-Cap Selection (cap selection menu)
	 - Events-Preview (displaying preview mode)

- Events-Board Select
	A simple event sheet for selecting the board template

## Cap selection menu
The cap selection menu is actually just a plain html file with some bottle cap images. The file is loaded into an iframe which serves as the bottle cap selection menu. The reason for this is that the iframe scrolling is handled by the browser and will feel native and good on any device. Building a smoothly scrolling menu system from scratch was not an attractive option due to time constrains.

The cap menu html is contained in the **capmenu.html** project file and is editable within the C3 IDE.

Within the cap menu you'll find several buttons using these kind of functions

    parent.c2_callFunction('changecapcolor', ['black', '0.99']);
This is the function used to tell the app to change the cap color. 2 parameters are passed along. A color (string) and a price (string, unused). Take a look at the function in the Construct 3 IDE to see how to these parameters are used.

## Preview mode
For this multi-platform version of the Configurator I've opted to use smudged versions of bottle cap textures instead of distorting the entire board with a GPU shader. This was a severe performance bottleneck on mobile devices and the smudge sprites completely fix this issue.

When the user clicks on the oven icon, a 'smots' sprite is created for each cap in the layout. The smots object looks at the cap object it spawned at, and copies it's animation name (the current color). The smots object is simply a container for all of the melted cap sprites.

Toggling the preview mode again simply removes all smots sprites and shows the normal cap sprites again.

### adding caps
So you want to add some more caps to the project, this is easy!
Within the graphics folder are 2 objects you'll need to edit:
 1. 'cap'
 2. 'smots'

Open the animation editor by double clicking on one of these objects. Every color is set up to be a new 'animation'. Once the animation editor is open, you'll see a list of animations in the right side menu. Adding a color to the project is as simple as making a new animation and adding the texture of your choice. 

**Warning:** Make sure you use identical names for your cap and smots sprites, otherwise your new color might not show up in preview mode.

### Adding caps to the selection menu
If you want your new cap to be selectable, it needs to be added to the **capmenu.html**

    <div class="cap-button orange-fanta" onClick="parent.c2_callFunction('changecapcolor', ['OrangeFanta', '0.99']);">

Make sure to use the correct name in the function call, otherwise the cap selector will reset to the default color (which is red currently).

### Randomizing cap placement
A requested feature was to add random Coca Cola caps while the user is painting with the color red. I've included an example on how to implement a feature like this. You can find the example by doing a project search for **'Random Caps'.**

### cap psd's
The psd's for the caps are included in the project. The texture resolutions have been chosen with performance in mind.
You can use other resolutions for the cap textures, but some adjustments might need to be made inside Construct to make sure they display correctly. Also keep in mind there will be a performance hit if you increase texture sizes. Be sure to test on a wide range of devices before making any final size changes.
