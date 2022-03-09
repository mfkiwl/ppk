# Workflow for static PPK using RINEX, UBX or ZIP files, with calibrated antenna supoort

## Using RINEX files

```bash
./ppk.sh -i 15 -c conf/ppk.conf -r example-rinex/rover/2022-02-25_13-26-06_GNSS-1.obs \
  -b example-rinex/base/2022-02-25_00-00-00_GNSS-1.obs \
  -n example-rinex/base/2022-02-25_00-00-00_GNSS-1.nav \
  -f ant/AS-ANT2BCAL.atx -t AS-ANT2BCAL -H 2
```

## Using ZIP files

```bash
./ppk.sh -i 15 -c conf/ppk.conf -r example-zip/rover/2022-02-25_13-26-06_GNSS-1.ubx.zip \
  -b example-zip/base/2022-02-25_00-00-00_GNSS-1.ubx.zip \
  -f ant/AS-ANT2BCAL.atx -t AS-ANT2BCAL -H 2
```

## Using UBX files

```bash
./ppk.sh -i 15 -c conf/ppk.conf -r example-ubx/rover/2022-02-25_13-26-06_GNSS-1.ubx \
  -b example-ubx/base/2022-02-25_00-00-00_GNSS-1.ubx \
  -f ant/AS-ANT2BCAL.atx -t AS-ANT2BCAL -H 2
```

# Old workflow for post-processing raw GNSS data using RTKLIB and Bash

## Example of use

- The following script will create the necessary folders. Before running, copy the .ubx files generated by the ZED-F9P receiver in the same folder where this repo was cloned. This workflow assumes that the .ubx files contain both the NMEA-0183 messages and the raw GNSS observations. In addition, the workflow assists in copying the RTCM corrections collected during the RTK phase, but are not necessary for PPK (may be used for cross-check).

```
./prepare.sh
```

- Now, download and copy files of the base station (observables: in rinex v2.11, matching in time with those of the collected data; ephemeris: in rinex v3.03, whole day)

- The following script unzip zipped base files and merge rinex if necessary. The first argument (e.g. 21) refers to the year of the base rinex files

```
./unzip_merge_rinex.sh 21
```

- The following script converts RTK-fix solutions to KML format, and also calculates the solution in PPK-mode using ppk.conf, and converts PPK-fix to KML format. The coordinates of the base may be expressed in ECEF format:

```
./solutions.sh 1 ppk.conf r 2078678.9081 -5683737.3052 2006886.9294
```

- ...or using latitude/longitude/height format (decimal degrees and meters)

```
./solutions.sh 1 ublox_f9p.conf l 18.461397786 -69.911323083 -9.147
```

- The following script deletes unzipped files that won't be needed again, to free-up some space

```
./clean.sh
```

- Note: add the repo directory to the PATH, so scripts may be called directly (e.g. prepare.sh instead of ./prepare.sh)
