# Visual Reports of fMRIPrep

# General html Output file

Based on:

        1) ([“Introduction to MRIQC [TRAIN-05-2022]”, 2022](zotero://select/library/items/GBRPHHIX))

        2) ([“fMRIPrep Tutorial #3: Examining the Preprocessed Data — Andy's Brain Book 1.0 documentation”](zotero://select/library/items/GEEHDAYD))

        3) Notes after my discussion with Gaëlle Leroux (CRNL) on 25/04/2024

## Error reports

- First check for the error reports at the very end of the html file

## Summary

- Overall summary of the pipeline --> information about anatomical and functional images
    
    - This should much the options we defined on the script that we used to run fmriprep

## Anatomical Section

### Brain mask and brain tissue segmentation of the T1w

- A good overview of the surfaces as well as the brain masks created by Freesurfer
    
    - White matter boundary is outlined in blue
    - Estimated brain mask is outlined in red
    - Grey matter boundary is outlined in magenda

**Example:**

[image]

- **Inaccurate brain mask** **([Provins et al., 2023](zotero://select/library/items/KSI3K2X5))**
    
    - The brain mask should closely follow the contour of the brain.
    - An inaccurate brain mask presents **“bumps”** surrounding high-intensity areas of signal outside of the cortex (e.g., a mask inclusding patches of he skull) and/or holes surrounding signal drop-out regions.
        
- **Error in brain tissue segmentation of T1w images ([Provins et al., 2023](zotero://select/library/items/KSI3K2X5))**
    
    - To confirm the good quality of the segmentation verify that
        
        - The magenda contour accurately outline the ventricles
        - The blue contour followed the boundary between GM and WM
    - **Exclusion criteria: inclusion of tissues other than the tissue of interest in the contour delineations**

**Example:**

[image]

### Spatial normalization of the anatomical T1w reference

- A gif that if you hover your mouse over the image, the image will flicker between participant space and to the chosen template (e.g. here MNI152NLin6Asym space. --> This can show you how well the native space was registered to the standard space overall
    
- **Failure in normalization to MNI space ([Provins et al., 2023](zotero://select/library/items/KSI3K2X5))**
    
          --> Normalization must be perfect
    
          --> To verify successful normalization assess the correct alignment of the following structures (in order of importance):
    
            1) Ventricles
    
            2) subcortical regions
    
            3) corpus callosum
    
            4) cerebellum
    
            5) cortical gray matter (GM)
    
            \--> Misalignment of 1,2 or 3: immediate exclusion
    
            \--> For 5 a bit more loose because volumetric (image) registration may not resolve substantial inter-individual differences
    
            \--> Any extreme stretching or distortion of the T1w image also indicates a failed normalization
    
    \--> **Tip: Put your mouse at the edge of the participant brain, should be perfect when flickers1 between the 2 spaces (This is not the case for the MRIQC output that has only the 1st iteration while here the algorithm has multiple iteration and trying to optimize the procedure).**
    
    **\--> Tip:** Make sure to check the alignment not only in the outlines of the brain (see above) but **also in the internal structures such as the ventricles.**
    
    **\--> Tip:** (From discussion with Gaëlle) Do not pay so much emphasis in **image x=0** as it it difficult and errors must be expected
    

**Example:**

\--> add the gif here with the mouse on the edge

### Surface reconstruction

- White matter in blue
- Pial surface in red
- These are overlaid on the participant’s T1w image
- Usually you do not exclude data here except the reconstructed surfaces are extremely inaccurate but you should already have seen that in MRIQC

## Functional Section

### Alignment between the anatomical reference of the fieldmap and the target EPI

- After discussion with Gaëlle patterns like the one’s highlighted below are normal
    

[image]

### Susceptibility distortion correction

- a gif that shows before and after distortion correction
- Note that the correction is affected by the direction AP vs PA
- **Any observation of susceptibility distortion artifacts leads to the exclusion of the scan**

**Example:**

[image]

### CompCor by default in fmriprep

- Data driven approach to  add nuisance regressors --> if you want to use this approach check also the correlation heatmap

### **Alignment of functional and anatomical MRI data (coregistration)**

- **Co - registration problem** ([Provins et al., 2023](zotero://select/library/items/KSI3K2X5))
    
    - Check the alignment of image intensity edges and the anatomical landmarks (e.g. the ventricles and the corpus callosum) between the BOLD and the T1w images

### Time-series carpet plot and correlation matrices

- GS: Global Signal
- GSCSF: Global Signal of the Cerebral Spinal Fluid
- GSWM: Global Signal of the White Matter
- 2 measures of motion
    
    - DVARS
    - FD: Framewise displacement

\--> Changes in motion tend to be correlated with GS and it is up to us to decide if we include any of these vars in the model. In general DVARS and FD are good ways to account for signal caused by motion artifacts

## Important Note: Smoothing is not in the pipeline of fmriprep, therefore it is a step that has to be executed after running the fmriprep pipeline.

- Smoothing has been ommited by design --> The fmriprep does not make assumption on how you are going to analyze your data
    
    - e.g. Some MVPA studies do not do any smoothing at all before the first level analysis
- The amount of smoothing is a matter of judgement
    
    - Experiments that are focused on larger cortical areas, probably want to use larger smoothing kernels
    

**\-->Tip: Since we have multiple tasks with different goals, smoothing should be executed with this in mind and probably individually for each task/goal. When performing that use a prefix (e.g. smo0.8 or sth along those lines) in the name of the data so it is quite clear and easy to find what was the smoothing!**

## Exclusion criteria of pre-processed data based on fMRIPrep visual report