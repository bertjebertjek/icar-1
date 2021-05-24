##Running ICAR

###Example

$ mpiexec -n 360 icar options_file_name.nml

###Input files

####Primary settings file:
icar\_options.nml [other filename can be specified on the commandline e.g. icar your\_options\_file]
        This file specifies all other files and options.  See the example in run/complete\_icar\_options.nml for all options or run/short\_icar\_options.nml for the more common options.

Other settings files can be defined in the primary settings file, or all settings can be included in one file.  

Most settings are documented in the sample settings files provided, additional documentation for all settings is provided in the [settings documentation](settings_documentation.md).

####Necessary netcdf files:

1) Boundary / Initial conditions file(s) (from e.g. wrf, reanalysis, or GCM output).  These files are specified in the options file as boundary\_files.

Must contain the following variables (optional variables are in square brackets) :
NOTE: any variable name can be used as specified in the options file.

    QV      = Water Vapor mixing ratio                  (kg/kg)
    T       = Air Temperature                           (K with an optional offset)
    P       = Pressure                                  (Pa with an optional [PB] offset)
    U       = East-West wind                            (m/s)
    V       = North-South wind                          (m/s)
    HGT     = Terrain Height                            (m)
    Z       = 3D model level heights                    (m)
    LAT     = Latitude on mass (P/T/etc.) grid          (degrees)
    LONG    = Longitude on mass (P/T/etc.) grid         (degrees)
    [PB]    = base pressure to be added to P            (Pa)
    [QC]    = cloud water content mixing ratio          (kg/kg)
    [QI]    = cloud ice content mixing ratio            (kg/kg)
    [SST]   = Sea Surface Temperature                   (K)
    [SWD]   = Shortwave down at the surface             (W/m^2)
    [LWD]   = Longwave down at the surface              (W/m^2)
    [SH]    = Sensible heat flux from the surface       (W/m^2)
    [LH]    = Latent heat flux from the surface         (W/m^2)
    [PBLH]  = Specified height of PBL                   (m)

Note that only Z or P need to be specified, as long as PSL or PS and HGT are specified, ICAR will compute one from the other. If it is given Z, it needs T to be real air temperature, not potential temperature.

2) High-resolution file (all variables are on the high-resolution grid ICAR will run on).  This filename is specified in the options file as the (poorly named ) init\_conditions\_file.

Must [optionally] contain:

    HGT     = Terrain Height                            (m)
    LAT     = Latitude on ICAR mass grid                (degrees)
    LONG    = Longitude on ICAR mass grid               (degrees)
    [LATU]  = Latitude on the ICAR EW-staggered wind grid         (degrees)
    [LONGU] = Longitude on the ICAR EW-staggered wind grid        (degrees)
    [LATV]  = Latitude on the ICAR NS-staggered wind grid         (degrees)
    [LONGV] = Longitude on the ICAR NS-staggered wind grid        (degrees)
    [LU]    = Land use cover classification for land surface model
    [SOIL]  = Soil type classification for land surface model

Must be in a (nearly) constant dx,dy projection (e.g. Lambert Conformal Conic)

3) (Optional) External files: in case starting conditions are to be set (e.g. Snow depth, soil temperature), external files can be used to define these. These can be measurements, or often just the first forcing file from which state variables are read in. 
    
    [optionally] contain:

    [lat_ext]      =  Lattitude on mass grid
    [lon_ext]      =  Longitude on mass grid
    [swe_ext]      =  snow water equivalent  (kg m-2)
    [rho_snow_ext] =  snow density (will otherwise be computed by Noah?) (kg m-3)
    [tsoil2D_ext]  =  Soil temperature (for all soil)     (K)     ! N.B. This will overwrite any input from soil_t_var or soil_deept_var  !!
    [tsoil3D_ext]  =  Soil temperature (per soil layer)   (K)          ! N.B. This will overwrite any input from soil_t_var or soil_deept_var  !!

