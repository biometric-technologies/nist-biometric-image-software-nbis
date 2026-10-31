![](.github/readme/banner.svg)

# NIST Biometric Image Software (NBIS), mirror

An unmodified copy of NBIS release 5.0.0, the fingerprint toolkit published by the US National Institute of Standards and Technology. NBIS is the reference C code behind a lot of fingerprint processing: it finds minutiae (the ridge endings and splits a fingerprint is matched on), scores image quality, matches two minutiae sets and compresses images in WSQ, the FBI's wavelet format for fingerprints. Iriscan keeps this mirror so that [biometric-converter](https://github.com/biometric-technologies/biometric-converter) builds against a known version. Nothing here is changed from what NIST published; if you need a different version, get it from NIST.

## What is inside

- `mindtct`: minutiae detector. Takes a fingerprint image, finds ridge endings and bifurcations with their position, angle and quality.
- `bozorth3`: minutiae matcher. Compares two minutiae sets and returns a match score; usable for 1:1 and 1:N.
- `nfiq`: NIST Fingerprint Image Quality, a 1 (best) to 5 (worst) score for a fingerprint image.
- `imgtools`: WSQ codec (`cwsq`, `dwsq`), lossless and baseline JPEG codecs (`cjpegl`, `djpegl`, `cjpegb`, `djpegb`), image viewer and conversion utilities.
- `an2k`: library and tools for ANSI/NIST-ITL records, the transaction format used to exchange fingerprints between agencies.
- `nfseg`: segments a four-finger slap image into single fingers. `pcasys`: pattern classifier (arch, loop, whorl).
- `ijg`, `jpeg2k`, `openjp2`, `png`: third-party image libraries bundled by NIST. `doc/`, `man/`: the user guide and man pages.

## How to build

Linux and macOS, from `INSTALL_LINUX_MACOSX.txt` (there are also `INSTALL_WINDOWS.txt` and `INSTALL_CYGWIN.txt`):

```sh
./setup.sh /opt/nbis --without-X11 --64   # target dir; --32 for 32-bit; --STDLIBS skips JPEG 2000 and PNG
make config
make it
make install LIBNBIS=no                   # LIBNBIS=yes combines everything into one libnbis.a
```

`--without-X11` leaves out the display tools (`dpyimage`, `dpyan2k`, `pcasysx`), which is what a server build wants. The binaries land in `/opt/nbis/bin`, headers in `/opt/nbis/include` and static libraries (`libmindtct.a`, `libbozorth3.a`, `libwsq.a`, `libnfiq.a`, `liban2k.a` and so on) in `/opt/nbis/lib`. `make catalog` optionally lists the API.

A quick try after installing:

```sh
dwsq raw finger.wsq -raw_out    # decode WSQ to finger.raw (plus finger.ncm with the image attributes)
mindtct -m1 finger.wsq finger   # minutiae in ANSI INCITS 378 orientation: finger.xyt, finger.min, finger.mdt
nfiq finger.wsq                 # prints the quality score
bozorth3 probe.xyt gallery.xyt  # prints the match score
```

## Where it is used

[biometric-converter](https://github.com/biometric-technologies/biometric-converter) links `libwsq`, `libmindtct` and the image libraries from this build to turn WSQ, JPEG, JPEG 2000 and PNG fingerprint images into ISO/IEC 19794-2 and ANSI INCITS 378 minutiae templates. Its README tells you to clone this mirror and run exactly the commands above.

## Related repositories

- [biometric-converter](https://github.com/biometric-technologies/biometric-converter): the converter that depends on this build.
- [nist-biometric-interchange-software-biomdi](https://github.com/biometric-technologies/nist-biometric-interchange-software-biomdi): mirror of NIST's BiomDI application package.
- [docs](https://github.com/biometric-technologies/docs): source of the developer documentation at docs.iriscan.com.

## License

Public domain. The software was developed at NIST by employees of the US Federal Government in the course of their official duties and, under 17 U.S.C. 105, is not subject to copyright. NIST has determined that it is not subject to the EAR (export regulations). The full statement and disclaimer are at the top of `INSTALL_LINUX_MACOSX.txt` and `CHANGELOG.txt`. There is no separate LICENSE file in this repository.

## Contact

Questions about this mirror or the converter: info@iriscan.net, https://www.iriscan.com. For NBIS itself, see NIST's fingerprint software page.
