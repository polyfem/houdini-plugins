# houdini-plugins

These are HDAs necessary to run a first attempt at a GUI for polyfem using houdini.

**THIS ASSUMES YOU HAVE A COMPILED VERSION OF POLYFEM ON YOUR MACHINE**

## Setup

The first initial setup only has to be done once.

1. **Install Houdini**: Install Houdini from SideFX. You will need to create a login, but the software is free to use for the purpose of this work. After installation, you will have to install the Houdini license for the Apprentice version. The software should guide you through. When given the option to install "SideFXLabs", check that option. 

2. **Create "otls" directory**: 
   - **Mac**: Open your library directory by clicking on your desktop, selecting the "Go" dropdown menu and holding the option key. In that folder, go to Preferences-> Houdini -> directory of your version of Houdini (currently 20.0 for a fresh install). Inside the 20.0 directory (or your version), create a directory called "otls" if it does not already exist.    
   - **Linux**: Houdini will have a folder in your home directory (e.g. houdini20.0). Create the "otls" directory within that directory.
   - **Other platforms**: If the folders are not where just described, you will have to create the "otls" directory in the correct location.
   - **Updating Houdini**: If you are updating Houdini from a previous verision to a newer version and you are using these HDAs, be sure to copy the 'otls' directory from the old version to the corresponding directory of the new verision. You will also need to rerun the setup steps below for the new version. 

3. **Place the HDAs**: Place the HDAs in this GitHub into the "otls" folder. 

4. **Set up Houdini Shell**: With Houdini installed and running on your desktop, select the "Windows" dropdown menu and then select "Shell". If you are on Linux, this should have opened an xterm window. Houdini assumes that you are using xterm as your terminal. To proceed, install xterm on your system (e.g. `sudo apt install xterm`) and try again. You may have to close and reopen Houdini.

5. **Install Python libraries**: Enter the following commands:

  hython -m get-pip.py   
  hython -m pip install meshio
 
  Note that this says "hython" and not "python". "hython" stands for the Houdini-specific version of Python.

6. **Restart Houdini**: Restart Houdini if it is currently open.

7. **Meshio library requirement**: The Houdini-specific version of Python (i.e. hython) requires the meshio library to run the polyfem hda. The code should let you know if the library has not been found when you try to use the HDA.

------

## Usage PolyFEM Node

In Houdini, there are three main windows at the start. The main window is the scene view. The window to the upper right is the parameters window, and the window to the bottom right is the node network window.

To run the HDA, follow these steps:

1. **Activate Node Menu**: Hover your mouse above the node network window (lower right) and hit the `Tab` key to bring up the node menu. 

2. **Select Digital Asset**: Select "Digital Asset" and then the polyfem HDA. Hit `Enter` to drop the HDA in the node network window. If you do not see the digital asset, it is likely that step 2 in the setup section was not completed correctly.

3. **View Parameters**: Once that node is placed, you should see its parameter interface in the window to the upper right. This is where you can set up your simulation. 

### Setting Up the Simulation

To set up the simulation, follow these steps:

1. **Point to polyfem_bin**: First, point the HDA to the `polyfem_bin`.

2. **Provide Working Directory**: Next, provide the HDA with a working directory where it will place any necessary files for your simulation.

3. **Import Geometry Files**: Next, go to the Geometry Tab. You can import geometry files that PolyFem supports here. Note that only `.msh` is supported for 3D meshes. It also assumes that `.msh` files are 3D meshes (tets or hexes). `.stl`, `.ply`, and `.obj` are only for use as obstacles.

### Navigating the Main Window

You can orbit, pan, and zoom in the main window by holding down the `Spacebar` and using your mouse and mouse buttons. Specific navigation controls depend a bit on your setup (mouse 2 or 3 button, trackpad, etc.). Play around to understand how to perform all 3 movements. Remember to hold down the `Spacebar`. Hitting either the "G" or "H" hotkeys with your mouse hovering over the main view is helpful for centering the view on your geometry. 

### Applying Transforms to Geometry

Within the geometry tab, you can apply transforms to your geometry. This can be done by entering values directly into the parameter interface or hovering your mouse over the main view and hitting the `Enter` key to bring up a gizmo. The gizmo automatically locks to the geometry in tab 1. By clicking on another geometry that you want to move, the gizmo should snap to that geometry's origin and allow you to move it. To change the gizmo to scaling or translation only or back to the original gizmo, cycle using the `Y` key. To exit from this mode, hit the `Esc` key. Your transforms will be updated as you perform them, and you can use undo. You can also type in the transform blocks in the parameter window if you need to reset anything.

### Using Multi-Material Support

This HDA now allows for multi-material support. Multi-materials are different subdomains of your geometry that have different material properties. The `.msh` file that you import may already have subdomains of your geometry defined. As long as the file is `.msh` version 4.1, this HDA will identify those and set up the geometry tab accordingly. Use gmsh to convert to the 4.1 version if necessary. 

**Adding or Changing Subdomains**: If you wish to add or change subdomains, there is a dropdown menu to do so.

Here, you will see the Elements and the subdomain label that corresponds to those elements. You can visualize those elements by selecting the arrow to the right of the elements. That will highlight the current elements selected for that label. You can hit `Esc` to maintain the selection.

**Assigning Elements to a New Subdomain**: If you want to assign elements to a new subdomain, first select the label that you wish to assign to, then hit the previously mentioned arrow button. You can use Houdini's selection tools to either add (by holding the `Shift` key) or subtract elements (by holding the `Ctrl` key) from the highlighted elements. Once you are happy with the selection, hit the `Enter` key to have those element numbers placed in the elements field (alternatively, the `Esc` key cancels). The final step to assign these elements to the new subdomain label is to hit the "Apply Changes?" button. The geometry tab will update to reflect your changes. 

**Returning to Original Subdomains**: If you wish to go back to the subdomains that existed when the geometry was first imported, hit the "Back to Original?" button. Elements can only be assigned to a new subdomain label. If they are removed from a subdomain label, they will return to their original assignment when the file was first imported. This is to prevent elements from not having an assignment.

### Applying Sidesets to 3D Geometry

In the Geometry tab, you can also apply sidesets to 3D geometry by adding them to your simulation. You can add as many as needed for each geometry. By pressing the arrowhead button once a sideset is added to the scene (i.e., the button to the right of the "Base Group" box), you will be taken into a mode to select either the surface faces or the points to define your sideset in the main view. You can highlight your selection and hit `Enter` to accept that selection. Unlike the transforms, you must hit `Enter` to accept your selection. 

**Adding to a Selection**: To add to a selection, hold the `Shift` key while selecting. 

**Subtracting from a Selection**: To subtract from a selection, hold the `Ctrl` key. 

There is also a toolbar at the top of the main window to assist with your selections. Always hit `Enter` in the main view to accept your selection.

### Running the Simulation

The rest of the process should be self-explanatory if you are familiar with PolyFem. If not, please refer to their documentation. 

Once all of your selections are made, simply hit the `Run PolyFEM` button on the main tab. This will write the JSON input file to the working directory and any volume and sideset files. It will attempt to run PolyFem in terminal or xterm (Linux). This works assuming the proper permissions allow it. It has not been not tested on Windows yet.

Please note that this HDA does not yet support interpolation related to boundary conditions.

------

## Usage ReadPVD Node (post-processing within Houdini)

Point the HDA to the PVD file for your simulation

The refresh button will refresh the playbar at the bottom of the screen to correspond with the number of timesteps written in the PVD file. You can refresh while the simulation is running.

Data is cached by default. You can force the cache to be cleared by hitting the clear cache button. Note: Refreshing does not clear the cache unless a new file is read.

You can adjust the colors to your preference. "Auto" sets the max and mix values to the corresponding parameter's values for the current timestep. 

The check box to compute the tensor principal values, vectors, etc., will perform calculations per timestep to provide outputs for the Right Cauchy-Green Stretch tensor (RCGST), the 1st PK stress tensor (PK1ST), the 2nd PK stress tensor (PK2ND), and Cauchy stress tensor (cauchy). This will reset the cache and make relvent parameters availible for selection in the color map. In addition, it will enable the the user to check a box to show normalized glyphs.

Showing the glyphs provides a normalized visual of the selected tensor at each surface node. The glyphs are oriented with respect to the prinipal directions of the tensor and the relative radii represent the normalized magnitude of the corresponding principal values. Changes to this section will clear the cache so all frames reflect the updated selections.   

The support of NSF Grant 2053851 is gratefully acknowlegded.





 
