# ciliR User Guide for Beginners

This guide was written by Dr Claire M. Smith, Associate Professor UCL

Please reference all authors listed in the accompanying paper in future publications that use this software. Oriane Grant, Isobel Larken, Samuel C. Reitemeier, Hannah M. Mitchison, William Dawes, Angus Phillips, Mario Cortina-Borja, Claire M. Smith. doi: https://doi.org/10.1101/2023.12.20.572306
 

**TIPS ON IMAGE ACQUISITION THAT CAN IMPROVE YOUR ANALYSES:**
Limitations of any automated software depend largely on image quality and so here are some easy steps that can improve the quality of your image and your results:

1)	Use the smallest pixel size available that shows sharp, clear cilia movement. 

2)	Use a high-power objective or crop your image so that the motile cilia fill the image. The more ‘dead space’ in your image the less ROI are being used for the analyses. 

3)	For high frequency analysis, we recommend that the microscope bulb is connected to a stable power source of at least 110-volt AC/ 60 Hz, lower voltages will encourage the bulb to flicker within the CBF range and this will enhance background noise. We also recommend that the image is not subject to down-stream processing, such as enhanced pixel gain, as this will also enhance background noise. 

4)	Capture at least double the number of frames to the frame rate of recording. We found that videos captured using high speed video cameras that capture at rates at least 120 frames per second with a length of 128 frames (to give a frequency resolution of 0.94) will give valid data for respiratory cilia. These settings capture an appropriate number of ciliary beat cycles to accurately average the CBF; the lower the frequency resolution (i.e. the more frames captured at this frame rate), the greater the accuracy of CBF. 



**A. How to prepare video files for ciliR**

A1.1 Group all your .AVI files to be analysed in one folder. Make sure this folder only contains .AVI files and no other filetype. 

A1.2 Rename each .AVI file so that it has a logical name that you can easily trace and understand. 

A1.3 Each file name must be unique. All .AVI files must start with a letter not a number, must be no longer than 16 characters and contain no punctuation.

**B. How to use ciliR – Part 1 - FiJi**
For conversion of image files to pixel intensity per frame

B1.1 Download and install FiJi from https://imagej.net/downloads 

B1.2 Choose version relevant to your operating system 

B1.3 Once ImageJ is installed, select “Plugins” and the “Install”. Find the relevant macro from the ImageJ macro folder in this repository. 

If you are using .AVI files, then this will be: “ciliR_Pixel_Intensity_AVI”. 
If you are using another file type, select: “ciliR_Pixel_Intensity_BioFormats” 

B1.4 Once installed. Restart FiJi and select “ciliR_Pixel_Intensity_AVI” from the “Plugins” tab of ImageJ (the macro will appear near the bottom of the drop down menu)

Tip: Try opening your video file with FiJi first before running the macro – it’s important to know that FiJi will read your file and that you can view beating cilia before proceeding with the macro.

B1.5 Once the macro is running, this window then opens to allow you to input all the information required about your files:

B1.6 Browse and select the folder that contains the .AVI files you want to be analysed.

B1.7 Analysis takes approximately 30s per AVI file depending on your processor speed. Do not use your computer while ciliR is running as this can interfere with processing. Wait until this message appears:

B1.8 When complete you will have a new sub-folder that contains a .txt file for each .AVI file. These files will look like this: 

 

**C. How to use ciliR – Part 2 – R Studio**

C1.1 Download and install R and R Studio from https://rstudio-education.github.io/hopr/starting.html 

C1.2 Choose version relevant to your operating system 

C1.3 Once R is running, install the following packages:

install.packages("ciliR)- via the “CiliR_1.0.tar.gz” file
install.packages("tidyverse")

C1.4 When you are ready to begin analysis, Open R studio and attach the “ciliR” package file from the “ciliR installation” folder. Then write the following lines of code in the R Script

C1.5 change these parameters to those relevant to your files

setwd("path") -refers to the address of the folder containing the .txt files.
FRate - refers to the frame rate that your video files were recorded at
NFrame - refers to number of frames per video (has to = a power 2)

Code structure :
analyse_cilia(path, FRate=125, NFrame=256, NoiseLevel=3, UpperLimit=30)

Example code :
analyse_cilia(path, 125, 256, 3, 30)


C1.6 You can also adjust the “NoiseLevel” for noise reduction. This compares the amplitude peak of the data to the amplitude peak of the background noise. Default is 3

C1.7 Select all the code you have written.

C1.8 Click Run.

C1.9 Analysis takes approximately 3sec per .txt file depending on your processor speed. A progress bar will appear.

When complete all your results will be saved as one dataframe referred to as “CiliaSummary” in the environment and saved as a “summary.csv” file in your working directory.

You can then clear objects from the workspace to free up space or continue immediately to data visualization.


**4. Data Visualization**

Your results will be saved as a dataframe called ‘CiliaSummary’ and as a “summary.csv” file in the working directory so you can use ggplot to prepare some plots.

You could visualise the mean CBF and active area values like this:

![image](https://github.com/smithlab-code/ciliR/assets/54943371/b695c7d6-6f42-49e0-a24e-1a37eea2056e)



or view histogram or the Activity Map

<img width="372" alt="image" src="https://github.com/user-attachments/assets/e7e3f7fc-8627-4051-ae43-721a31d37b8c" />


Activity map - with Hz labels:

<img width="608" alt="image" src="https://github.com/user-attachments/assets/be092178-5414-4cf0-8b4c-f6f03a7613c9" />

see "Data Vis functions in R" folder for some functions you can try



**5. Troubleshooting**
Common errors: 


5.1 Image J error:

Incorrect File name saved.
	All .avi file names must start with a letter not a number, must be no longer than 16 characters and contain no punctuation.


5.2 ImageJ error:

Unable to open file	You might need to change the ImageJ macro code to make it specific to your file type. Looks at code line 78 below. Specifically, the code highlighted refers to a 3 channel RGB image where only the 3rd channel is opened to speed up processing. 

run("Bio-Formats Importer", "open=[filename] specify_range view=[Standard ImageJ] stack_order=Default c_begin=3 c_end=3 c_step=1 t_begin=1 t_end=[CROP_TYPE] t_step=[CROP2_TYPE]");
               
   




Any other comments or suggestions please fill in this form to get in touch with us: https://forms.office.com/e/G9sRxxTUH1 

We would be happy to consider future authorship for any significant contributions to the code.
![image](https://github.com/smithlab-code/ciliR/assets/54943371/b1dd5dd2-00bb-46c5-8af5-17cde7a50dae)
