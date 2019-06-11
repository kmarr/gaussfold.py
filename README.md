# gaussfold.py
A python version of the gaussfold function for IDL

Original gaussfold.pro - http://astro.uni-tuebingen.de/software/idl/aitlib/misc/gaussfold.html

### Purpose:
Smoothes a plot by convolving with a Gaussian profile. Main purpose is to convolve a spectrum
(flux against wavelength) with a given instrument resolution. Also applicable e.g. to smooth
ligthcurves or in time-series analysis.
                                    
### Input Parameters
           lambda = In ascending order sorted array (double or float)
                    containing the values of the x-axis (e.g. the
                    wavelength- or frequencygrid). Irregularly spaced
                    grids are acepted.
           flux   = Array (double or float) same size as lambda
                    containing the values of the y-axis (e.g. the
                    flux).
           fwhm   = FWHM in units of the x-axis (e.g. Angstrom) of
                    the Gaussian profile.

### Output Parameters
           smoothedFlux = Array (double of float) same size as lambda
                          containing the smoothed flux.

### Calling Sequence (Convolving spectral lines)
```python
       from gaussfold import gaussfold as gf
       conv_flux = gf(lambda, flux, fwhm)
```

### Example: Using gaussfold.py to convolve spectral lines
flux is the original flux to be convolved. Units do not matter. velocity is the
velocity of the line in km/s

fac e is the faction of the total flux that is scattered by electrons. This is
typically between 0.2 - 0.4 for Be stars.

v h is the most probably speed of the H atoms. This can be assumed to be
the sound speed c s of the disk.

v e is the most probably speed of the electrons.

```python
       from gaussfold import gaussfold as gf
       fac_e = 0.4
       v_h = 10.0 #Hydrogen velocity
       v_e = 600 #Electron velocity
       notscat_flux = [i*(1-fac_e) for i in flux]
       scat_flux = [i*fac_e for i in flux]
       notscat_conv = gf(velocity, notscat_flux, v_h)
       scat_conv = gf(velocity, scat_flux, v_e)
       flux_conv = scat_conv + notscat_conv
```
