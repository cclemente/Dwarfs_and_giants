
# Instructions for how to build and run new OpenSim models for Predictive Simulations: 

1. go to scale_setup_mass.xml ("for Chris" folder) - open in Notepad++

2. open "scale factors TD_CJC" file to determine what the scale factor is for the new model (1.0988 is new factor for 100kg)

3. edit the (i) name (line 3), (ii) mass (line 5), and (iii) all values for <scales> (so for example if was 1.3821 for a 200kg model and for 100kg is now 1.0988)

4. open "fullbody_Hamner2010_simpleknee.OSIM" model in OpenSim - > TOOLS-> SCALE MODEL ->Load the scaleset that was edited prior ->RUN scale tool -> Save new model as "scaled100_unscaledFMO" (this is because max isometric forces are not scaled yet)

5. Open MatLab script called "scalemodels.m" (in for chris folder) and within the file need to edit: (i) modelfilepath (on line 10), and set this to where the file "fullbody_Hamner2010_simpleknee.OSIM" is located, (ii) line 13, change the scaled model name to be "scaled100_unscaledFMO", (iii) line 15, change the scaled mass to be 100, (iv) line 60, change the name of the file that is to be saved "scaled100.osim" (this step scales all of the isometric forces in the model)

6. copy the newly scaled model "scaled100.osim" into the OpenSIM model subfolder called "scaled100" within the folder of desktop called "3DPredsim_dwardsGiants..." (note you need to make a new subfolder for each model

7. now we need to create the cpp file. 
                - go to opensim-ad (this is found on C drive in root)
                ->opensim-ad-core
                ->opensim
                ->external functions
                *make two new sub-folders "predsim_scale100" and  "predsim_scale100_pp" that you jsut copy over 2 files from other models folder

8. open the "cmakelists" in notepad++ and edit the name in the first line to match your model

9. then rename the cpp file to match you model mass and then open the cpp file in notepad++, line 49 -> update to be the new scale factor with 1.0988

10. repeat above for the .pp subfolder with 2 same files 

11. now go back to the root folder of ->external functions and open the "CMakelists.text" file and add a sub-directory for the 2 new folders that you just created (just keep adding to the bottom of list in file as we create more models. 

12. go to root directory OPEN the -> Cmake program - bin- GUI to open and click CONFIGURE->GENERATE

13. go to opensim-ad (this is found on C drive in root)
                ->opensim-ad-core-build
                CLICK->opensim.sln (opens in visual studio) - which should have just been recompiled in previous step
                check to ensure the folder called "External Functions" has appeared and look at it

**step 13 - need to check that cpp file (which is within the .snl file) has the updated numbers for the spheres. (do this for the 6 spheres, whereby GRF_1,...GRF_2... and so on)
Before:
    GRF_1_r[0] = Vec3(Force_values_1_r[9], Force_values_1_r[10], Force_values_1_r[11]);
    GRF_1_r[1] = Vec3(Force_values_1_r[6], Force_values_1_r[7], Force_values_1_r[8]);
New:
    GRF_1_r[0] = Vec3(Force_values_1_r[3], Force_values_1_r[4], Force_values_1_r[5]);
    GRF_1_r[1] = Vec3(Force_values_1_r[0], Force_values_1_r[1], Force_values_1_r[2]);

**also commented out line 371-372 (vect 3 normal) from both the cpp and the pp files**

On Lines: 623-678
                
14. right click on the "Predsim_scale100" - choose -> "set as startup project" -> click DEBUG + start without DEBUGGING +YES 

15. go to ->opensim-ad-core-build ->opensim -> external functions -> predsim_scale100 -> CHECK FOR foo.m file (that should have just been created)

16. - go to opensim-ad (this is found on C drive in root)
                ->opensim-ad-core
                ->cgeneration
                ->open the generate_dll file in notepad++ and edit the filename on line 27 to be predsim_scale100 (note to check the paths here are correct) - SAVE
                ->right click on this file and choose "run with powershell"


17. go to C:\Users\Owner\Documents\Visual Studio 2015\Projects\opensim_generator
                -> go to predsim_scale100 -> install ->bin should be the .dll file here that you now want to COPY to your -> external functions folder in the Dwarfs and giants folder on desktop

18. Repeat steps 14-17 for the .pp file as well . So right click and run - when editing filenames ensure you add "_pp" to these now. Once you have the new pp file in the bin folder, take this and copy over to the Dwarfs and giants folder 

19. find the .m file on Dwarfs and Giants -> Polynomials-> "Main_polnomials_scale" and open in Matlab. Then edit filenames on line 17, 29, 34 (3 places) and also edit the mass on line 19 (so edit 4 things in total) - SAVE and RUN (*note ensure that you have navigated to the filepath in matlab where you want the outputs to go). this will create two files: Muscleinfo and muscledata

20. go the OCP subfolder in the Dwarfs and Giants - copy an existing predsimXXX.m file and reanme to be 100 - open in Notpad++. Edit subject on line 38 to be 100, edit the scaling factor on line 39 to be 1.0988, edit filenames on line 171, 173, 304 (to be scale 100 or whatever your model mass is)- SAVE 

21. open this file from OCP folder in Matlab (ensure that Matlab is looking in the OCP directory as the current working folder) and then RUN (and pray to the simulation gods).

What to edit the the MATLAB code prior to starting the simulation
1. check the number of iterations - we are currently running 10,000 (iter)
2. check the speeds

