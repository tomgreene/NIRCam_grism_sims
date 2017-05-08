# NIRCam_grism_sims
Notes on using aXeSIM to simulate JWST NIRCam grism images and spectra
Tom Greene
Feb 23, 2017

This package contains configuration files for simulating JWST NIRCam
LW grism data. Results have the scale, dispersion, signal, noise, and
orientation (in x, y detector coordinates) expected for actual LWA R
grism data. Simulated images and extracted point source spectra are
produced. Data values are in units of electrons per second.

CHANGES:

Created configuration files to allow simulation of spectra with F277w (m=1 & m=2), F410M (m=1) and F480M (m=1) filters. New files:

./CONF/
	NIRCam_LWAR_F277W.conf 
	NIRCam_LWAR_F410M.conf
	NIRCam_LWAR_F480M.conf

./SIMDATA/
	F277W_passband.dat
	F410M_passband.dat 
	F480M_passband.dat 


Make new catalog files to reference correct filters and PSFs (last col):
./SAVE/
	catalog_MOS_LWA_277W_PSF.lis	
	catalog_MOS_LWA_410M_PSF.lis
	catalog_MOS_LWA_480M_PSF.lis
	
Make new demo files:
	axesim_nircam_F277W_demo.py
	axesim_nircam_F410M_demo.py
	axesim_nircam_F480M_demo.py

Also used the released JWST ETC to calculate medium backgrounds in the new filters and updated values in the axesim_nircam_*demo.py files:

Use these as bck_flux_disp values for each filter:
 F277W    0.40
 F322W2   0.56
 F356W    0.34  
 F410M    0.30 
 F430M    0.17 
 F444W    0.96  
 F460M    0.23  
 F480M    0.28 



PREVIOUS CHANGES January 5, 2017:
- Updated the expected medium background levels in the files:   
   axesim_nircam_F430M_demo.py  
   axesim_nircam_F460M_demo.py  
   axesim_nircam_demo.py
- Create a new demo file for F356W:
   axesim_nircam_F356W_demo.py (mostly a copy of axesim_nircam_demo.py)


PREVIOUS CHANGES May 2016
- Updated the sensitivity files with more accurate throughputs from STScI for NIRCam ModA (March 2016 versions). Values changed by ~10%, a bit more at lambda > 4.5 microns:
./CONF/NIRCam.A.1st.sensitivity.fits
./CONF/NIRCam.A.2nd.sensitivity.fits

PREVIOUS CHANGES March 2016:
- Updated the sensitivity files with more accurate throughputs from STScI for NIRCam ModA (Feb. 2016 versions):
./CONF/NIRCam.A.1st.sensitivity.fits
./CONF/NIRCam.A.2nd.sensitivity.fits

- Updated the read noise in the configuration files for each filter. All now have READNOISE = 10 e- . Impacted files (these were 20.0):
./CONF/NIRCam_LWAR_F322W2.conf
./CONF/NIRCam_LWAR_F356W.conf
./CONF/NIRCam_LWAR_F444W.conf

- Updated zodi backgrounds using values from J Leisenring.
ZODI BACKGROUNDS HAVE INCREASED BY ~2x over the old ones computed from the STScI prototype ETC. Use these bck_flux_disp and bck_flux_dir values for these filters: [NOTE THESE ARE OBSOLETE IN 2017 - see more recent values above]
Filter   zodi (e-/s/pxl)
F277W    0.30
F356W    0.90  
F444W    3.00  
F322W2   1.25
F430M    0.65  
F460M    0.86  

Files impacted:
axesim_nircam_F430M_demo.py
axesim_nircam_F460M_demo.py
axesim_nircam_demo.py



PREVIOUS CHANGES Sep 2015:
- Created configuration files, passband files, object lists, and demo
scripts for use with F430M and F460M filters:
./axesim_nircam_F430M_demo.py 
./axesim_nircam_F460M_demo.py 
./CONF/NIRCam_LWAR_F430M.conf
./CONF/NIRCam_LWAR_F460M.conf
./SAVE/catalog_MOS_LWA_430M_PSF.lis
./SAVE/catalog_MOS_LWA_460M_PSF.lis
./SIMDATA/F430M_passband.dat
./SIMDATA/F460M_passband.dat


PREVIOUS CHANGES Aug 2015:
- Copied missing F322W2 PSF flies to ./SIMDATA
- Edited ./SAVE/catalog_MOS_LWA_PSF.lis master catalog input file to set objects to use F356W PSF (MODIMAGE = 6)
- Edited README to list correct output file names (now LWAR)

PREVIOUS CHANGES May 2015: 
- Updated CONF/NIRCam.A.*sensitivity.fits files with more realistic optics transmission and QE values 
- Replaced CV2 point PSFs with ones created by WebbPSF centered on a pixel center (in SIMDATA directory)
- Updated imaging passband PCE files (SIMDATA/F*_passband.dat)
- Slight update to wavelength range in CONF/NIRCam_LWAR_F444W.conf file
(other .conf files unchanged from April 2015)


 **** USING aXWSIM to simulate JWST NIRCam LW grism data ****

1. Start pyraf, load aXeSIM
---------------------------
This assumes that you are running aXeSIM from STSDAS. See the HST WFC3 aXeSIM README file if you want to install aXeSIM as an external pyraf package (as taxesim and use it that way  - included below too). It may be best to install pyraf (including aXeSIM) as part of one of your python environments, as assumed below. Note that pyraf requires python 2.7 (or perhaps 2.x), not 3.x.

Do in terminal, assuming that you have installed pyraf in the astroconda python environment. Just change the name if you have pyraf in a different environment:

$ source activate astroconda

$ pyraf
setting terminal type to xgterm...

--> stsdas
--> slitless
--> axesim

The aXeSIM software package 1.4 was developed by the
Slitless Spectroscopy Group of the ST-ECF.  Maintenance
is provided by the Space Telescope Science Institute. 
Further information is available at:
http://www.stsci.edu/resources/software_hardware/stsdas/axe

Any questions regarding this software can  be directed to:
help@stsci.edu

axesim/:
 prepimages     prepspectra     simdata         simdirim        simdispim

-->

$ cd aXeSIM_NC_2016Mar  (the untarred directory)


2. aXeSIM/taxesim14
-------------------
aXeSIM and taxesim14 have the identical five commands that are listed whenthe package is loaded. However, in 'taxesim14' all commands have the letter
't' as a prefix (aXeSIM: simdata <--> taxesim14: tsimdata)

See the DOCUMENTATION directory for the aXeSIM manual

There exist a brief online-help for all commands (use e.g. "--> help simdata").

3. Testing aXeSIM
-----------------
There is the script 'axesim_nircam_demo.py' (need to edit for taxesim),
which runs a simulation for the NIRCam LWA R grism + F356W filter.
The simulations (results named 'WFC3_templ_<grism name>_*.fits')
use a spectral template spectrum and a template PSF image while producing
both, a simulated slitless and direct image. Also a default extraction is
performed on the slitless image.
To run the script in PyRAF, type on the command line for aXeSIM:
--> pyexecute('axesim_nircam_demo.py')

The output images and spectra land in the ./OUTSIM directory:
NIRCam_LWAR_F356W_direct.fits   - Simulated direct image
NIRCam_LWAR_F356W_images.fits   - I think a single image made from the PSF file
NIRCam_LWAR_F356W_slitless.fits - Simulated grism image
NIRCam_LWAR_F356W_slitless_2.SPC.fits - Extracted 1-D spectra, 1 per object
NIRCam_LWAR_F356W_slitless_2.STP.fits - 2D grism spectrum template?
NIRCam_LWAR_F356W_spectra.fits  - Input 1-D spectra, 1 for each object

Note that only 1 order (m = 1) is created or extracted for each
object. The F356W filter will always only have 1 order, but F322W2
can have 2 orders (m=1 and m=2) depending on the object location
in the image.

NOTE: It is also very instructive to do this interactively via:

--> epar simdata


*** NOTE that the aXeSIM output images have data units of electrons per second! They must be multiplied by the integration time to get the correct absolute signal, noise, and background levels. However, S/N can be measured from the output files as the are ***

=====================================================================

NOTES How to start aXeSIM as external pyraf package:
In case that you have downloaded and installed aXeSIM as the external
IRAF/PyRAF package 'taxesim14', you must make its location available
to IRAF/PyRAF. To do this, put the following lines into the IRAF
file "login.cl" (in "$HOME/iraf"; replace <your path> with
the location of your aXeSIM installation):

reset   taxesim14          = <your path>/taxe/iraf/
task    taxesim14.pkg   = taxesim14$taxesim14.cl
reset   helpdb          = (envget("helpdb") //",taxesim14$lib/helpdb.mip")

(Then add a 't' in front of all axesim commands above... becomes 'taxesim')
Would also need to edit the axesim_nircam_demo.py script file
