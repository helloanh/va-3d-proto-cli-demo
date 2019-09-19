# Segmentation Layer: Dicom to Stl 

This is the report on research for 3rd party library we can use to convert the VIstA images.  

## Javascript Libraries 

There are two libraries, cornerstonejs cansegmentatation but not in STL.  See sample site:  
https://dcmjs.netlify.com/createsegmentation/   

Another is a react-based app to display STL/dicom, which will help when building the view layer of the client app.  However, it also does not build to STL.  

## GUI-based desktop application 

3D Slicer is an open-sourced GUI-based software.  It has the most support and testing--meaning hospitals and universities have used the application in production.  It has a limited CLI writen in python code--however, the GUI is still required to load the python script to run.  

Conclusion: Very robust however, it will be hard to implement into our server-side application.  


## Python VMTK 

The simplest tool is to use a python-based ![vmtk (Vascular Modeling Toolkit)](http://www.vmtk.org/index.html) command-line software to convert raw dicom into stl. The software is based on VTK. VMTK is based on VTK and uses much of the same data formats.

** [Read full installation and demo test here]() **


