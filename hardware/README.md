# Custom Hardware

We created custom food-safe finger attachments for safe robot-assisted feeding of finger foods. 
These consist of 3D printed bases (which screw into any Robotiq gripper) and laser-cut acrylic fingers which 
pressure fit into the bases. The acrylic is dipped in [Smooth-Sil 940](https://www.smooth-on.com/products/smooth-sil-940/), a food and dishwasher safe silicone. There is a textured pattern on the ends of the fingers to facilitate food acquisition.
This design is allows for the attachments to be easily removed and cleaned, while also being grippy enough to pick up a variety of items.


## Components
1.  **finger_base**: The 3D printed base design which screws into the Robotiq gripper.
2.  **finger_dxf**: The sketch to laser-cut the actual fingers. We cut [Optix Acrylic Sheet](https://www.homedepot.com/pep/OPTIX-20-in-x-32-in-Acrylic-Sheet-1AU0202A/100402509), with a thickness of 0.093 in.
3.  **finger_mold**: The mold design for coating the acrylic in silicone. We would insert the fingers into the mold, pour the silicone mixture over it, then wait 12 hours for the silicone to cure.
4.  **robotiq_camera_mount**: Unrelated to the fingers, this is the camera mount design to attach the RealSense to the Robotiq gripper. This was made by the team behind [FLAIR](https://emprise.cs.cornell.edu/flair/), and can also be found in their hardware files.
