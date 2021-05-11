# Suppa_Duppa_Hella_Cool_Butane_Schlieren_POD
![Alt text](/C001H001S0001000001.png?raw=true "Title")

This code preforms proper orthogonal decomposition on schlieren images by implementing the method of snapshots.

This repository contains all the files and information needed to preform Proper Orthogonal Decomposition on the provided set of Schlieren images, collected at the UTSA Hypersonics laboratory. The images were collected at 50.4kHz using a Photron SA-Z high-speed camera. 

The dataset contains 50k images. The code selects a subset of a few thousand from the AVI files in the repository and runs the analysis on them. The original dataset is approximately 10Gb of data when the files are in .tif format. The use of an AVI file makes the dataset very easy to handle and share. The entire dataset can be found at:
https://drive.google.com/drive/folders/1lZyCfYOVROu7Hvp3lukIA8v6bdr_45zS?usp=sharing

To run the code, just download the avi files from the Google Drive and have them in the same directory and the code.

Pandas is not used in the manipulation of the image sets as it does not preform well with the extremely large column vector set. It will be used for correlations and other subsequent analysis. The numpy arrays used are set to use a datatype of float32 as they take up significantly less memory to use. Float16 were initially used, however it was found that the use of float16 was actually slowing down the code. The code uses temporary files to save numpy arrays that are needed later in the code, but are not needed in the code block immedeatly following. This allows us to save a lot of computer memory. Not maxing out the computer memory will help the code run faster. When an array is needed, the temporary file is loaded back into the memory. The saving and loading the temporary files is very fast and does not take much extra time to do.

Special modules used:
  1. PIMS - PIMS is used to import the AVI files. The beauty of PIMS is that it loads the files without loading the images into the memory until a specific frame is called and it is      very fast.
  2. Scipy - The Scipy function sgemm is used to do the matrix multiplication. The Numpy matmul function does not preform well with very large matrices. Using the np.matmul function      for a matric of 2000x2000 takes around 8 minutes to compute where as the Scipy sgemm function  takes about 0.3 minutes for the same size matrix.
  3. Astrodrengro - Dendrograms from the Astrodendro module are used for analysis of the orthonormal basis that is created.

Hypothesis:
  1. It was hypothesised that because a lot of the flame is relativly laminar, that 90% of the total energy would be contained in the first 5 modes. It was quickly discovered that        this is not the case. To account for 90% of the energy when using 2000 images, approximatly 30 modes are required.
  2. It was additionally hypothesised that because the most turbulent part of the flow is at the end of the flame, that the higher modes, which show the smaller scale structures of        the flow, would have the energy concentrated at the edge of the flame. Analysis of the modes shows that this is mostly correct. It has been found that while most of the energy        is concentrated at the end of the flame for the mid-range modes, there is a higher than expected concentration of energy at the torch nozzle exit and the highest modes have all      of their energy concentrated at the nozzle exit.
  3. The next hypothesis to explore is how the modes correlate when using different numbers of images and consequently more modes. Does the energy distribution changes? How well do        the modes correlate to each other? Pandas will be used in the correlations.
