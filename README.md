# VMTK Guide to Install and Demo Dicom to STL format

Test anaconda enviroment is [here](vmtk-demo/teststl/vmtk)

## Definitions 
Vmtk is a collection of libraries and tools for 3D reconstruction, geometric analysis, mesh generation and surface data analysis for image-based modeling of blood vessels. 

[vmtk (Vascular Modeling Toolkit)](http://www.vmtk.org/index.html) command-line software to convert raw dicom into stl. The software is based on VTK. VMTK is based on VTK and uses much of the same data formats.

## 1. Preinstallation Instructions

Make sure you have Anaconda installed and updated.

1) Install Anaconda or Miniconda (preferably the python 3.6 version)  
2) Open the anaconda prompt   
3) Install and update anaconda-client:   

```bash
conda install anaconda-client
conda update conda anaconda-client
conda config --set restore_free_channel true
```

## 2. Set Up VMTK
Enter the following commands to create a new virtual environment with vmtk installed (replace the name “teststl” with whatever you want to call the environment, or replace python=3.6 with python=3.5 if you want python 3.5):

``` bash
conda create -n teststl -c vmtk python=3.6 itk vtk vmtk
source activate teststl
cd teststl
 ```
## 3. Working with VMTK

#### Vmtk workflow
The workflow of VMTK seems to be centered around: 

3D scan → Surface model → Centerline → Analysis → Output geometry   

To use most VMTK scripts, though, we first have to convert our surfaces to the VTK mesh format (.vtp), which is an XML-based format. VMTK has a set of scripts for conversion to/from STL (and other common surface mesh formats) to vtp:  

```bash
vmtksurfacereader converts from external formats to VTP
vmtksurfacewriter converts from VTP to external formats.
```

#### Sample conversion
1. To view images

``` bash
# view image
vmtk vmtkimagereader -ifile path/to/file.dcm --pipe vmtkimageviewer
```
![image-dcm](images/dcm-view.png)

2. Conversion steps from dcm to stl

Example conda environment is in teststl

```bash
# write the image volume in vti format (the VTK XML format for images - it’s convenient because it’s internally gzip’d)
vmkt vmtkimagereader testdicom.dcm --pipe vmtkimagewriter -ofile image_volume.vti

# convert vti to vtp
vmtk vmtkimagereader -ifile Anonymous.MR._.13.1.2017.06.28.09.41.02.294.59320516.dcm -ofile test.vti

# Connvert dcm file to vtp format
vmtkmarchingcubes -ifile test.vti -l 300.0 -ofile test_surface.vtp

# if you want to also convert dcm to vtp and preview, pipe the result to viewer
vmtk vmtkmarchingcubes -ifile test.vti -l 300.0 -ofile test_surface.vtp --pipe vmtksurfaceviewer
```

Here is what you will see if converted to vtp in the surface viewer.
![image-converted-vtp](images/vtp-view.png)

```bash 
Convert vtp to stl
vmtk vmtksurfacereader -ifile mc_surface.vtp --pipe vmtksurfacewriter -ofile mc_surface.stl
```
![image converted stl](images/mc_stl.png)

## 4. Resources
https://pseudoprofound.wordpress.com/2016/05/17/using-vmtk-for-analyzing-pipe-like-structures/   
http://www.vmtk.org/download/ 

## 5. Sample DICOM Files 
This collection includes datasets from 20 subjects with primary newly diagnosed glioblastoma who were treated with surgery and standard concomitant chemo-radiation therapy (CRT) followed by adjuvant chemotherapy.  

Source is from the Cancer Imaging Archive.   

[Brain Tumor Progression](https://wiki.cancerimagingarchive.net/display/Public/Brain-Tumor-Progression). Raw file is 3.2GB in size unzipped.  

Obtain from s3 bucket as a 1.3GB zipped file here:  
https://dirac-dicom-test.s3.amazonaws.com/brain-tumor-progression/Brain-Tumor-Progression.zip

**From Siemens** 
Small dicom files.  
1. https://dirac-dicom-test.s3.amazonaws.com/Siemens_spectroEnhDICOM/Anonymous.MR._.13.1.2017.06.28.09.41.02.294.59320516.dcm 
2. https://dirac-dicom-test.s3.amazonaws.com/Siemens_spectroEnhDICOM/Anonymous.MR._.12.1.2017.06.28.09.41.02.294.59320505.dcm
3. https://dirac-dicom-test.s3.amazonaws.com/Siemens_spectroEnhDICOM/Anonymous.MR._.14.1.2017.06.28.09.41.02.294.59320527.dcm  


