# pincram
PINCRAM is a method for masking cranial MR images based on multiple atlases

#### References and credits:  
    Heckemann RA, Ledig C, Gray KR, Aljabar P, Rueckert D, Hajnal JV, Hammers A. Brain Extraction Using Label Propagation and Group Agreement: Pincram. PLoS One. 2015 Jul 10;10(7):e0129211. doi: 10.1371/journal.pone.0129211. eCollection 2015.  

    https://soundray.org/pincram/  
    https://github.com/soundray/pincram  

#### Requirements:  
* 3D Slicer (https://www.slicer.org/)
* FSL (https://fsl.fmrib.ox.ac.uk/fsl/fslwiki)
* Nifty Reg (http://cmictig.cs.ucl.ac.uk/research/software/software-nifty/niftyreg)    
* Nifty Seg (http://cmictig.cs.ucl.ac.uk/wiki/index.php/NiftySeg)  
* IRTK (https://github.com/BioMedIA/IRTK)
* CMake (and GUI) (apt-get install cmake cmake-qt-gui)
* GNU Parallel  (apt-get install parallel)

#### Example
```
for nii in /[niftifir]/\*.nii.gz ; do ./pincram $nii -result [resultdir]/${(echo $nii | cut -c[num]-[num])}_pincram.nii.gz -altresult ${(echo $nii | cut -c[num]-[num])}_pincram_alt.nii.gz -atlas [see usage] -par $(nproc) -levels 1 ; done`
```

#### Usage:
```
./pincram --help
pincram version 0.2

Copyright (C) 2012-2015 Rolf A. Heckemann
Web site: http://www.soundray.org/pincram

Usage: ./pincram <input> <options> <-result result.nii.gz> -altresult altresult.nii.gz \
                       [-probresult probresult.nii.gz] \
                       [-workdir working_directory] [-savewd] \
                       [-atlas atlas_directory | -atlas file.csv] [-atlasn N ] \
                       [-levels {1..3}] [-par max_parallel_jobs] [-ref ref.nii.gz]

<input>     : T1-weighted magnetic resonance image in gzipped NIfTI format.

-result     : Name of file to receive output brain label. The output is a binary label image.

-altresult  : Name of file to receive alternative output label.  The output is a binary label image.

-probresult : (Optional) name of file to receive output, a probabilistic label image.

-workdir    : Working directory. Default is present working directory. Should be a network-accessible location

-savewd     : (Optional) By default, the temporary directory under the working directory
              will be deleted after processing. Set this flag to keep intermediate files.

-atlas      : Atlas directory.
              Has to contain limages/full/m{1..n}.nii.gz, lmasks/full/m{1..n}.nii.gz and posnorm/m{1..n}.dof.gz
              Alternatively, it can point to a csv spreadsheet: first row should be base directory for atlas
              files. Entries should be relative to base directory. Each row refers to one atlas.  
              Column 1: atlasname, column 2: full image, column 3: margin image, column 4: mask, column 5: transformation 
              (.dof format) for positional normalization. Atlasname should be unique across entries.

-tpn        : Rigid transformation for positional normalization of the target image (optional)

-atlasn     : Use a maximum of N atlases.  By default, all available are used.

-levels     : Integer, minimum 1, maximum 3. Indicates level of refinement required.

-ref        : Reference label against which to log Jaccard overlap results.

-par        : Number of jobs to run in parallel (shell level).  Please use with consideration.
```
