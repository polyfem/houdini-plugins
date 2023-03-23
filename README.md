# houdini-plugins

These are HDAs necessary to run a first attempt at a GUI for polyfem using houdini.

*THIS ASSUMES YOU HAVE A COMPILED VERSION OF POLYFEM ON YOUR MACHINE*

To use:

First initial setup. Only has to be done once.

1) Install houdini from SideFX. You will need to create a login, but the software is free to use for the purpose of this work. After installation, you will have to install the houdini license for the apprentice version. The software should guide you through. 

2) On Mac (this is the only platform these have been tested on so far), open your library directory by clicking on your desktop, selecting the "Go" dropdown menu and holding the option key. In that folder, go to Preferences-> Houdini -> directory of your version of houdini (currently 19.5 for a fresh install). Inside the 19.5 directory (or your version), create a directory called "otls" if it does not already exist. If you are on another platform, you will have to create the same folder in the correct location. Note- the directory where the "otls" directory needs to be created shares a file called "houdini.env". This is an environmental file for houdini that lets you change specific settings, which can be found in the houdini documentation.   

3) Place the two HDAs in this github into that "otls" folder. 

4) Restart houdini if it is currently open.

5) Once houdini is restarted. Select the "Windows" dropdown menu and then select "Python Shell". Note- this is a houdini specific version of python.

6) Enter the following commands:

  hython -m get-pip.py   
  hython -m pip install meshio
  
  note that this says hython and not python. hython stands for the houdini specific version
  
7) Note the polyfem hda will check for meshio when it is used and attempt to install it, but that has note been tested rigoriously so following step 6 is necessary if that process fails. The houdini specific version of python (i.e. hython) requires the meshio library to run the polyfem hda. The code should let you know if the library has not been found when you try to use the HDA per below.

That completes the initial setup.

After that, every time you open houdini you should have access to the polyfem hda. Please note that the polyfem hda uses the other hda in the background. That is why both hdas need to be added to the otls folder. 


Using the polyfem HDA:

In houdini there are 3 main windows at the start. The main window is the scene view. The window to the upper right is the parameters window and the window to the bottom right is the node network window. 

To run the HDA, hover your mouse above the node network window (lower right) and hit the tab key to bring up the node menu. Select "Digital Assest" and then the polyfem hda. Hit enter to drop the hda in the node network window. If you don't not see the digital asset, it is likely that step 2 above was not completed correctly.

Once that node is placed, you should see its parameter interface in the window to the upper right. This is where you can setup your simulation. 

First, point the hda to the polyfem_bin.

Next, provide the hda with a working directory where it will place any necessary files for your sim.

Next, go to the Geometry Tab. You can import geometry files that polyfem supports here. *Note only .msh is supprted for 3D meshes. It also assumes that .msh files are 3D meshes (tets or hexes). .stl, .ply, and .obj are only for use as obstacles.*

You can orbit, pan, and zoom in the main window by holding down the spacebar and using your mouse and mouse buttons. Specific navigation controls depend a bit on your setup (mouse 2 or 3 button, trackpad, etc.). Play around to understand how to perform all 3 movements.  Remember to hold down the space bar. 

Within the geometry tab, you can apply transforms to your geometry. This can be done by entering values directly or hovering your mouse over the main view and hitting the enter key to bring up a gizmo. The gizmo automatically locks to the geometry in tab 1. By clicking on another geometry that you want to move, the gizmo should snap to that geometry's origin and allow you to move it. To change the gizmo to scaling or translation only or back to the original gizmo, cycle using the Y key. To exit from this mode, hit the escape key. Your transforms will be updated as you perform them and you can use undo. You can also type in the transform blocks in the parameter window if you need to reset anything. 

In the Geometry tab, you can also apply sidesets to 3D geometry by adding them to your simulation. You can add as many as needed for each geometry. By pressing the arrowhead button once a sideset is added to the scene, you will be taken into a mode to select either the surface faces or the points to define your sideset in the main view. You can highlight your selection and hit enter to accept that selection. Unlike the tranforms, you must hit enter to accept your selection. To add to a selection, hold the shift key while selecting. To subsract from a selection, hold the control key. There is also a tool bar at the top of the main window to assist with your selections. Again, always hit enter in the main view to accept your selection.

The rest of the process should be pretty self-explantory if you are famiiar with polyfem. If not, please see their documentation. 

Once all of your selections are made. Simply hit the Run PolyFEM button on the main tab. This will write the json input file to the working directory and any sideset files. It will attempt to run polyfem in terminal (This works for mac assuming the proper permissions allow it. It. has not been not tested on other platforms yet).

Please note that this HDA does not yet support multivolumes or volume selection. It also does not yet support interpolation related to boundary conditions.








 
